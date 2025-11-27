# Competitor Intelligence Agent with Browserbase

Build automated competitor monitoring systems that track ads across LinkedIn, Facebook/Meta, and other platforms using Browserbase for browser orchestration.

## Overview

This skill helps you build a competitor intelligence agent with five core components:

1. **Browser orchestration layer** (Browserbase) - session management, authentication, stealth
2. **Platform-specific scrapers** - LinkedIn ads, Facebook ads, landing pages
3. **Change detection system** - tracks what's new or changed
4. **Analysis engine** - identifies patterns, themes, visual changes
5. **Intelligence reporter** - synthesizes insights using AI

## Environment Setup

### Dependencies

```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

pip install browserbase playwright pillow imagehash openai python-dotenv requests
playwright install chromium
```

### Environment Variables

Create a `.env` file:

```env
BROWSERBASE_API_KEY=your-api-key-here
BROWSERBASE_PROJECT_ID=your-project-id-here
OPENAI_API_KEY=sk-your-key-here
```

## Core Components

### 1. Browser Manager Class

The foundation for all browser interactions:

```python
from typing import Optional
from browserbase import Browserbase
from playwright.sync_api import sync_playwright, Page
import time

class BrowserManager:
    """Manages Browserbase sessions with stealth and proxy support."""

    def __init__(self, api_key: str, project_id: str, context_id: Optional[str] = None):
        self.api_key = api_key
        self.project_id = project_id
        self.context_id = context_id
        self.bb = Browserbase(api_key=api_key)
        self.session = None
        self.playwright = None
        self.browser = None
        self.context = None
        self.page = None

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.cleanup()

    def create_session(self, timeout: int = 300, enable_stealth: bool = True,
                       enable_proxy: bool = True, proxy_country: str = "us",
                       keep_alive: bool = False) -> dict:
        """Create a new Browserbase session with optional stealth and proxy."""
        session_config = {
            "projectId": self.project_id,
            "browserSettings": {
                "stealth": enable_stealth,
                "proxy": {"enabled": enable_proxy, "country": proxy_country} if enable_proxy else None
            },
            "timeout": timeout,
            "keepAlive": keep_alive
        }

        if self.context_id:
            session_config["contextId"] = self.context_id

        self.session = self.bb.sessions.create(**session_config)

        return {
            "session_id": self.session.id,
            "connect_url": self.session.connectUrl,
            "replay_url": f"https://www.browserbase.com/sessions/{self.session.id}"
        }

    def connect_browser(self) -> Page:
        """Connect to the browser session and return the page."""
        self.playwright = sync_playwright().start()
        self.browser = self.playwright.chromium.connect_over_cdp(self.session.connectUrl)
        self.context = self.browser.contexts[0]
        self.page = self.context.pages[0]
        return self.page

    def cleanup(self):
        """Clean up all browser resources."""
        if self.page:
            try:
                self.page.close()
            except:
                pass
        if self.context:
            try:
                self.context.close()
            except:
                pass
        if self.browser:
            try:
                self.browser.close()
            except:
                pass
        if self.playwright:
            try:
                self.playwright.stop()
            except:
                pass
```

### 2. Authenticated Context Management

Persist login sessions across scraping runs:

```python
def create_authenticated_context(api_key: str, project_id: str,
                                  platform: str, credentials: dict) -> str:
    """Create a reusable authenticated browser context."""
    bb = Browserbase(api_key=api_key)
    context = bb.contexts.create(projectId=project_id)
    context_id = context.id

    with BrowserManager(api_key, project_id, context_id=context_id) as mgr:
        mgr.create_session()
        page = mgr.connect_browser()

        if platform == "linkedin":
            page.goto("https://www.linkedin.com/login", wait_until="networkidle")
            page.fill('input[name="session_key"]', credentials['email'])
            page.fill('input[name="session_password"]', credentials['password'])
            page.click('button[type="submit"]')
            page.wait_for_url("https://www.linkedin.com/feed/", timeout=30000)

        elif platform == "facebook":
            page.goto("https://www.facebook.com/login", wait_until="networkidle")
            page.fill('input[name="email"]', credentials['email'])
            page.fill('input[name="pass"]', credentials['password'])
            page.click('button[name="login"]')
            page.wait_for_timeout(5000)

    return context_id
```

### 3. LinkedIn Ad Scraper

```python
def scrape_linkedin_ads(page: Page, company_name: str, max_ads: int = 20) -> list:
    """Scrape ads from LinkedIn Ad Library for a specific company."""
    page.goto("https://www.linkedin.com/ad-library", wait_until="networkidle")

    # Search for company
    search_box = page.locator('input[aria-label*="Search"]')
    search_box.fill(company_name)
    time.sleep(1)
    search_box.press("Enter")
    time.sleep(3)

    ads_data = []
    scroll_attempts = 0

    while len(ads_data) < max_ads and scroll_attempts < 10:
        ad_cards = page.locator(
            '[data-test-id*="ad-card"], .ad-library-card, [class*="AdCard"]'
        ).all()

        for card in ad_cards:
            if len(ads_data) >= max_ads:
                break

            ad_data = {
                "platform": "linkedin",
                "company": company_name,
                "scraped_at": time.strftime("%Y-%m-%d %H:%M:%S")
            }

            # Extract headline
            headline_selectors = [
                '[data-test-id*="headline"]',
                '.ad-headline',
                '[class*="headline"]',
                'h3', 'h4'
            ]
            for selector in headline_selectors:
                try:
                    headline = card.locator(selector).first
                    if headline.count() > 0:
                        ad_data["headline"] = headline.inner_text().strip()
                        break
                except:
                    continue

            # Extract body text
            body_selectors = [
                '[data-test-id*="body"]',
                '.ad-body',
                '[class*="body-text"]',
                'p'
            ]
            for selector in body_selectors:
                try:
                    body = card.locator(selector).first
                    if body.count() > 0:
                        ad_data["body"] = body.inner_text().strip()
                        break
                except:
                    continue

            # Extract CTA
            cta_selectors = [
                'button',
                '[class*="cta"]',
                '[data-test-id*="cta"]',
                'a[class*="button"]'
            ]
            for selector in cta_selectors:
                try:
                    cta = card.locator(selector).first
                    if cta.count() > 0:
                        ad_data["cta_text"] = cta.inner_text().strip()
                        break
                except:
                    continue

            # Extract image URL
            try:
                img = card.locator('img').first
                if img.count() > 0:
                    ad_data["image_url"] = img.get_attribute('src')
            except:
                pass

            # Extract landing URL
            try:
                link = card.locator('a[href*="http"]').first
                if link.count() > 0:
                    ad_data["landing_url"] = link.get_attribute('href')
            except:
                pass

            # Only add if we got meaningful data
            if ad_data.get("headline") or ad_data.get("body"):
                ads_data.append(ad_data)

        # Scroll for more content
        page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        time.sleep(2)
        scroll_attempts += 1

    return ads_data
```

### 4. Facebook/Meta Ad Scraper

```python
def scrape_facebook_ads(page: Page, company_name: str, max_ads: int = 20) -> list:
    """Scrape ads from Meta Ad Library."""
    url = f"https://www.facebook.com/ads/library/?active_status=active&ad_type=all&country=US&q={company_name}"
    page.goto(url, wait_until="networkidle")
    time.sleep(3)

    ads_data = []
    scroll_attempts = 0

    while len(ads_data) < max_ads and scroll_attempts < 10:
        ad_cards = page.locator('[class*="AdCard"], [data-testid*="ad"]').all()

        for card in ad_cards:
            if len(ads_data) >= max_ads:
                break

            ad_data = {
                "platform": "facebook",
                "company": company_name,
                "scraped_at": time.strftime("%Y-%m-%d %H:%M:%S")
            }

            # Extract ad content (similar pattern to LinkedIn)
            try:
                text_content = card.inner_text()
                ad_data["body"] = text_content[:500]  # First 500 chars
            except:
                pass

            try:
                img = card.locator('img').first
                if img.count() > 0:
                    ad_data["image_url"] = img.get_attribute('src')
            except:
                pass

            if ad_data.get("body"):
                ads_data.append(ad_data)

        page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        time.sleep(2)
        scroll_attempts += 1

    return ads_data
```

### 5. Change Detection System

SQLite schema for tracking ads:

```sql
CREATE TABLE IF NOT EXISTS ads (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    competitor_id TEXT NOT NULL,
    platform TEXT NOT NULL,
    ad_identifier TEXT,
    headline TEXT,
    body TEXT,
    cta_text TEXT,
    image_url TEXT,
    landing_url TEXT,
    metadata TEXT,
    snapshot_date DATETIME NOT NULL,
    UNIQUE(competitor_id, platform, ad_identifier, snapshot_date)
);

CREATE TABLE IF NOT EXISTS changes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    competitor_id TEXT NOT NULL,
    platform TEXT NOT NULL,
    change_type TEXT NOT NULL,  -- 'new_ad', 'removed_ad', 'modified_ad'
    ad_identifier TEXT,
    severity TEXT,  -- 'high', 'medium', 'low'
    details TEXT,
    detected_at DATETIME NOT NULL
);
```

Python change detection:

```python
import sqlite3
import json
from datetime import datetime

class ChangeDetector:
    def __init__(self, db_path: str = "competitor_intel.db"):
        self.conn = sqlite3.connect(db_path)
        self._init_db()

    def _init_db(self):
        cursor = self.conn.cursor()
        cursor.executescript('''
            CREATE TABLE IF NOT EXISTS ads (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                competitor_id TEXT NOT NULL,
                platform TEXT NOT NULL,
                ad_identifier TEXT,
                headline TEXT,
                body TEXT,
                cta_text TEXT,
                image_url TEXT,
                landing_url TEXT,
                metadata TEXT,
                snapshot_date DATETIME NOT NULL,
                UNIQUE(competitor_id, platform, ad_identifier, snapshot_date)
            );

            CREATE TABLE IF NOT EXISTS changes (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                competitor_id TEXT NOT NULL,
                platform TEXT NOT NULL,
                change_type TEXT NOT NULL,
                ad_identifier TEXT,
                severity TEXT,
                details TEXT,
                detected_at DATETIME NOT NULL
            );
        ''')
        self.conn.commit()

    def generate_ad_identifier(self, ad: dict) -> str:
        """Generate unique identifier for an ad."""
        return f"{ad.get('headline', '')}:{ad.get('body', '')}"[:200]

    def detect_changes(self, competitor_id: str, platform: str,
                       new_ads: list) -> list:
        """Compare new ads against stored ads and detect changes."""
        cursor = self.conn.cursor()

        # Get most recent ads for this competitor/platform
        cursor.execute('''
            SELECT ad_identifier, headline, body, cta_text, image_url
            FROM ads
            WHERE competitor_id = ? AND platform = ?
            AND snapshot_date = (
                SELECT MAX(snapshot_date) FROM ads
                WHERE competitor_id = ? AND platform = ?
            )
        ''', (competitor_id, platform, competitor_id, platform))

        existing_ads = {row[0]: row for row in cursor.fetchall()}
        changes = []
        now = datetime.now().isoformat()

        for ad in new_ads:
            ad_id = self.generate_ad_identifier(ad)

            if ad_id not in existing_ads:
                # New ad detected
                changes.append({
                    "competitor_id": competitor_id,
                    "platform": platform,
                    "change_type": "new_ad",
                    "ad_identifier": ad_id,
                    "severity": "high",
                    "details": json.dumps(ad),
                    "detected_at": now
                })

            # Store the ad
            cursor.execute('''
                INSERT OR REPLACE INTO ads
                (competitor_id, platform, ad_identifier, headline, body,
                 cta_text, image_url, landing_url, metadata, snapshot_date)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            ''', (
                competitor_id, platform, ad_id,
                ad.get('headline'), ad.get('body'), ad.get('cta_text'),
                ad.get('image_url'), ad.get('landing_url'),
                json.dumps(ad.get('metadata', {})), now
            ))

        # Detect removed ads
        new_ad_ids = {self.generate_ad_identifier(ad) for ad in new_ads}
        for old_id in existing_ads:
            if old_id not in new_ad_ids:
                changes.append({
                    "competitor_id": competitor_id,
                    "platform": platform,
                    "change_type": "removed_ad",
                    "ad_identifier": old_id,
                    "severity": "medium",
                    "details": json.dumps({"removed": True}),
                    "detected_at": now
                })

        # Log changes
        for change in changes:
            cursor.execute('''
                INSERT INTO changes
                (competitor_id, platform, change_type, ad_identifier,
                 severity, details, detected_at)
                VALUES (?, ?, ?, ?, ?, ?, ?)
            ''', (
                change['competitor_id'], change['platform'],
                change['change_type'], change['ad_identifier'],
                change['severity'], change['details'], change['detected_at']
            ))

        self.conn.commit()
        return changes
```

### 6. AI Intelligence Reporter

```python
from openai import OpenAI
import json

class IntelligenceReporter:
    def __init__(self, openai_api_key: str):
        self.client = OpenAI(api_key=openai_api_key)

    def generate_report(self, changes: list) -> str:
        """Generate executive summary from detected changes."""
        high_severity = [c for c in changes if c.get('severity') == 'high']
        medium_severity = [c for c in changes if c.get('severity') == 'medium']
        low_severity = [c for c in changes if c.get('severity') == 'low']

        prompt = f"""Generate an executive summary of competitive intelligence findings.

High Priority Changes ({len(high_severity)}):
{json.dumps(high_severity[:10], indent=2)}

Medium Priority Changes ({len(medium_severity)}):
{json.dumps(medium_severity[:10], indent=2)}

Low Priority Changes ({len(low_severity)}):
{json.dumps(low_severity[:5], indent=2)}

Please provide:
1. **TL;DR**: 2-3 sentence summary of the most important findings
2. **Key Threats**: Competitor moves that could impact our business
3. **Opportunities**: Gaps or weaknesses we could exploit
4. **Recommended Actions**: Top 3 specific actions to take

Keep the response concise and actionable. Format in markdown."""

        response = self.client.chat.completions.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "You are a competitive intelligence analyst. Provide concise, actionable insights."},
                {"role": "user", "content": prompt}
            ],
            temperature=0.7,
            max_tokens=1000
        )

        return response.choices[0].message.content
```

### 7. Visual Change Detection (Perceptual Hashing)

Detect visual changes in ad creatives:

```python
import imagehash
from PIL import Image
import requests
from io import BytesIO

def get_image_hash(image_url: str) -> str:
    """Generate perceptual hash for an image URL."""
    try:
        response = requests.get(image_url, timeout=10)
        img = Image.open(BytesIO(response.content))
        return str(imagehash.phash(img))
    except Exception as e:
        return None

def compare_image_hashes(hash1: str, hash2: str, threshold: int = 10) -> bool:
    """Compare two image hashes. Returns True if images are similar."""
    if not hash1 or not hash2:
        return False
    h1 = imagehash.hex_to_hash(hash1)
    h2 = imagehash.hex_to_hash(hash2)
    return abs(h1 - h2) <= threshold
```

## Complete Usage Example

```python
import os
from dotenv import load_dotenv

load_dotenv()

def run_competitor_monitoring(competitors: list):
    """Run full competitor monitoring cycle."""
    api_key = os.getenv("BROWSERBASE_API_KEY")
    project_id = os.getenv("BROWSERBASE_PROJECT_ID")
    openai_key = os.getenv("OPENAI_API_KEY")

    detector = ChangeDetector()
    reporter = IntelligenceReporter(openai_key)
    all_changes = []

    for competitor in competitors:
        print(f"Monitoring: {competitor['name']}")

        with BrowserManager(api_key, project_id) as mgr:
            session_info = mgr.create_session(enable_stealth=True)
            print(f"Session replay: {session_info['replay_url']}")

            page = mgr.connect_browser()

            # Scrape LinkedIn ads
            linkedin_ads = scrape_linkedin_ads(page, competitor['name'])
            changes = detector.detect_changes(
                competitor['id'], 'linkedin', linkedin_ads
            )
            all_changes.extend(changes)

            # Scrape Facebook ads
            fb_ads = scrape_facebook_ads(page, competitor['name'])
            changes = detector.detect_changes(
                competitor['id'], 'facebook', fb_ads
            )
            all_changes.extend(changes)

    # Generate intelligence report
    if all_changes:
        report = reporter.generate_report(all_changes)
        print("\n" + "="*50)
        print("COMPETITIVE INTELLIGENCE REPORT")
        print("="*50)
        print(report)
        return report
    else:
        print("No changes detected.")
        return None

# Run monitoring
competitors = [
    {"id": "competitor1", "name": "Acme Corp"},
    {"id": "competitor2", "name": "Beta Inc"},
]

run_competitor_monitoring(competitors)
```

## Debugging Tips

- **Session replays**: Every Browserbase session provides a replay URL - use it to debug selector issues
- **Stealth mode**: Enable for sites with bot detection
- **Residential proxies**: Use `proxy_country` parameter for geo-specific scraping
- **Contexts**: Create authenticated contexts once, reuse across sessions

## Extension Ideas

- Add Twitter/X, Google, TikTok ad scrapers
- Build a Streamlit dashboard for visualization
- Track creative trends over time with image clustering
- Set up scheduled runs with cron or Airflow
- Add Slack/email notifications for high-priority changes

---
*Based on patterns from Siddharth Bharath's Browserbase tutorial*
