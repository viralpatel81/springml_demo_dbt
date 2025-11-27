<workflow name="full-system">
<objective>
Build the complete competitor intelligence agent with all five components: browser orchestration, platform scrapers, change detection, analysis engine, and intelligence reporter.
</objective>

<process>
<phase name="setup">
<description>Environment and credentials setup</description>
<actions>
```bash
python -m venv venv
source venv/bin/activate
pip install browserbase playwright pillow imagehash openai python-dotenv requests
playwright install chromium
```

Create `.env`:
```
BROWSERBASE_API_KEY=your-api-key
BROWSERBASE_PROJECT_ID=your-project-id
OPENAI_API_KEY=sk-your-key
```

Create `config.py`:
```python
import os
from dotenv import load_dotenv

load_dotenv()

BROWSERBASE_API_KEY = os.getenv("BROWSERBASE_API_KEY")
BROWSERBASE_PROJECT_ID = os.getenv("BROWSERBASE_PROJECT_ID")
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

COMPETITORS = [
    {"name": "Competitor A", "linkedin": "competitor-a"},
    {"name": "Competitor B", "linkedin": "competitor-b"},
]
```
</actions>
</phase>

<phase name="browser-manager">
<description>Create the BrowserManager class</description>
<actions>
Create `browser_manager.py`:
```python
from browserbase import Browserbase
from playwright.sync_api import sync_playwright
from typing import Optional

class BrowserManager:
    def __init__(self, api_key: str, project_id: str, context_id: Optional[str] = None):
        self.api_key = api_key
        self.project_id = project_id
        self.context_id = context_id
        self.bb = Browserbase(api_key=api_key)
        self.session = None
        self.playwright = None
        self.browser = None
        self.page = None

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.cleanup()

    def create_session(self, timeout=300, enable_stealth=True, enable_proxy=True):
        config = {
            "projectId": self.project_id,
            "browserSettings": {
                "stealth": enable_stealth,
                "proxy": {"enabled": enable_proxy, "country": "us"} if enable_proxy else None
            },
            "timeout": timeout
        }
        if self.context_id:
            config["contextId"] = self.context_id

        self.session = self.bb.sessions.create(**config)
        return {
            "session_id": self.session.id,
            "connect_url": self.session.connectUrl,
            "replay_url": f"https://www.browserbase.com/sessions/{self.session.id}"
        }

    def connect_browser(self):
        self.playwright = sync_playwright().start()
        self.browser = self.playwright.chromium.connect_over_cdp(self.session.connectUrl)
        self.page = self.browser.contexts[0].pages[0]
        return self.page

    def cleanup(self):
        if self.browser:
            self.browser.close()
        if self.playwright:
            self.playwright.stop()
```
</actions>
</phase>

<phase name="scrapers">
<description>Build platform-specific scrapers</description>
<actions>
Create `scrapers/linkedin.py`:
```python
import time
from datetime import datetime

def scrape_linkedin_ads(page, company_name: str, max_ads: int = 20):
    page.goto("https://www.linkedin.com/ad-library", wait_until="networkidle")

    search_box = page.locator('input[aria-label*="Search"]')
    search_box.fill(company_name)
    time.sleep(1)
    search_box.press("Enter")
    time.sleep(3)

    ads_data = []
    scroll_attempts = 0

    while len(ads_data) < max_ads and scroll_attempts < 10:
        ad_cards = page.locator('[data-test-id*="ad-card"], .ad-library-card').all()

        for card in ad_cards:
            if len(ads_data) >= max_ads:
                break

            try:
                ad = {
                    "platform": "linkedin",
                    "company": company_name,
                    "headline": safe_text(card, '[class*="headline"]'),
                    "body": safe_text(card, '[class*="body"]'),
                    "cta_text": safe_text(card, '[class*="cta"], button'),
                    "scraped_at": datetime.now().isoformat()
                }
                if ad["headline"] or ad["body"]:
                    ads_data.append(ad)
            except Exception as e:
                print(f"Error extracting ad: {e}")

        page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        time.sleep(2)
        scroll_attempts += 1

    return ads_data

def safe_text(element, selector):
    try:
        el = element.locator(selector).first
        return el.text_content().strip() if el else None
    except:
        return None
```
</actions>
</phase>

<phase name="change-detection">
<description>SQLite-based change tracking</description>
<actions>
Create `change_detector.py`:
```python
import sqlite3
import json
from datetime import datetime

class ChangeDetector:
    def __init__(self, db_path="competitor_intel.db"):
        self.conn = sqlite3.connect(db_path)
        self._create_tables()

    def _create_tables(self):
        self.conn.execute("""
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
            )
        """)
        self.conn.commit()

    def store_ads(self, competitor_id: str, ads: list):
        for ad in ads:
            ad_id = f"{ad.get('headline', '')}:{ad.get('body', '')}"[:200]
            try:
                self.conn.execute("""
                    INSERT INTO ads (competitor_id, platform, ad_identifier,
                                     headline, body, cta_text, snapshot_date)
                    VALUES (?, ?, ?, ?, ?, ?, ?)
                """, (competitor_id, ad["platform"], ad_id,
                      ad.get("headline"), ad.get("body"),
                      ad.get("cta_text"), datetime.now()))
            except sqlite3.IntegrityError:
                pass  # Duplicate
        self.conn.commit()

    def detect_changes(self, competitor_id: str, days_back: int = 7):
        cursor = self.conn.execute("""
            SELECT * FROM ads
            WHERE competitor_id = ?
            AND snapshot_date > datetime('now', ?)
            ORDER BY snapshot_date DESC
        """, (competitor_id, f'-{days_back} days'))
        return cursor.fetchall()
```
</actions>
</phase>

<phase name="intelligence-reporter">
<description>GPT-powered analysis</description>
<actions>
Create `reporter.py`:
```python
import openai
import json

def generate_report(changes: list, api_key: str):
    client = openai.OpenAI(api_key=api_key)

    high_severity = [c for c in changes if is_high_severity(c)]
    medium_severity = [c for c in changes if not is_high_severity(c)]

    prompt = f"""Generate an executive summary of competitive intelligence.

High Priority Changes ({len(high_severity)}):
{json.dumps(high_severity[:10], indent=2)}

Medium Priority Changes ({len(medium_severity)}):
{json.dumps(medium_severity[:10], indent=2)}

Provide:
1. **TL;DR**: 2-3 sentence summary
2. **Key Threats**: What competitors are doing that could hurt us
3. **Opportunities**: Gaps we could exploit
4. **Recommended Actions**: Top 3 things to do

Keep concise and actionable. Format in markdown."""

    response = client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content

def is_high_severity(change):
    # New ad or significant content change
    return change.get("is_new", False)
```
</actions>
</phase>

<phase name="main-orchestrator">
<description>Tie it all together</description>
<actions>
Create `main.py`:
```python
from config import *
from browser_manager import BrowserManager
from scrapers.linkedin import scrape_linkedin_ads
from change_detector import ChangeDetector
from reporter import generate_report

def run_intelligence_cycle():
    detector = ChangeDetector()

    for competitor in COMPETITORS:
        print(f"Monitoring {competitor['name']}...")

        with BrowserManager(BROWSERBASE_API_KEY, BROWSERBASE_PROJECT_ID) as mgr:
            session_info = mgr.create_session()
            print(f"Replay: {session_info['replay_url']}")

            page = mgr.connect_browser()

            # Scrape LinkedIn
            if competitor.get("linkedin"):
                ads = scrape_linkedin_ads(page, competitor["linkedin"])
                detector.store_ads(competitor["name"], ads)
                print(f"Found {len(ads)} ads")

        # Detect changes
        changes = detector.detect_changes(competitor["name"])

        # Generate report
        if changes:
            report = generate_report(changes, OPENAI_API_KEY)
            print(report)

if __name__ == "__main__":
    run_intelligence_cycle()
```
</actions>
</phase>
</process>

<success_markers>
- All components created and connected
- Scraper extracts ads from target platforms
- Changes stored and tracked in SQLite
- AI report generated with actionable insights
</success_markers>
</workflow>
