---
name: competitor-intelligence-agent
description: Builds automated competitor ad monitoring systems using Browserbase and Playwright. Use when you need to track competitor ads on LinkedIn, Facebook, or other platforms and generate AI-powered intelligence reports.
---

<objective>
This skill guides you through building a competitor intelligence agent that monitors competitor advertising activity across LinkedIn, Facebook, and other platforms. It uses Browserbase for browser orchestration (session management, authentication persistence, stealth mode) and Playwright for web automation.

The system scrapes ad libraries, detects changes over time, and generates AI-powered executive summaries with actionable insights. The architecture is modular—each component (scraping, change detection, analysis, reporting) is independent and swappable.
</objective>

<essential_principles>
<principle name="browserbase-as-infrastructure">
Browserbase is "AWS Lambda for browsers"—handles session management, authentication persistence via Contexts, stealth mode, and residential proxies. You focus on the intelligence layer, not browser infrastructure.
</principle>

<principle name="contexts-for-auth">
A Browserbase Context stores cookies, localStorage, and session state. Create once with authentication, reuse across sessions. No repeated logins.
</principle>

<principle name="session-replays-for-debugging">
Every Browserbase session is recorded. Use replay URLs to debug broken selectors or timing issues—watch the browser like a video.
</principle>

<principle name="modular-architecture">
Five independent components: browser orchestration, platform scrapers, change detection, analysis engine, intelligence reporter. Each is swappable.
</principle>
</essential_principles>

<intake>
What would you like to build?

1. **Full system** - Complete competitor intelligence agent with all components
2. **Ad scraper only** - LinkedIn or Facebook ad library scraper
3. **Change detection** - Track changes in competitor ads over time
4. **Intelligence reports** - AI-powered analysis of collected ad data
5. **Browser setup** - Browserbase configuration and authentication contexts

Provide: competitor names to monitor, platforms (LinkedIn, Facebook, etc.)
</intake>

<routing>
| User Intent | Workflow | Notes |
|-------------|----------|-------|
| Full system | workflows/full-system.md | All five components |
| Ad scraper | workflows/ad-scraper.md | Platform-specific scraping |
| Change detection | workflows/change-detection.md | SQLite tracking |
| Reports | workflows/intelligence-reports.md | GPT-4 analysis |
| Browser setup | workflows/browser-setup.md | Browserbase config |
</routing>

<quick_start>
<step name="environment">
```bash
python -m venv venv
source venv/bin/activate
pip install browserbase playwright pillow imagehash openai python-dotenv requests
playwright install chromium
```
</step>

<step name="credentials">
Create `.env`:
```
BROWSERBASE_API_KEY=your-api-key
BROWSERBASE_PROJECT_ID=your-project-id
OPENAI_API_KEY=sk-your-key
```
</step>

<step name="basic-session">
```python
from browserbase import Browserbase
from playwright.sync_api import sync_playwright

bb = Browserbase(api_key=API_KEY)
session = bb.sessions.create(projectId=PROJECT_ID)

pw = sync_playwright().start()
browser = pw.chromium.connect_over_cdp(session.connectUrl)
page = browser.contexts[0].pages[0]

page.goto("https://www.linkedin.com/ad-library")
# Scrape ads...
```
</step>
</quick_start>

<architecture>
```
┌─────────────────────────────────────────────────────────────┐
│                    Competitor Intelligence Agent             │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │ Browserbase │  │  Platform   │  │  Change Detection   │ │
│  │   (Browser  │──│  Scrapers   │──│  (SQLite + hashing) │ │
│  │   Orchestr) │  │  LinkedIn   │  │                     │ │
│  │             │  │  Facebook   │  │                     │ │
│  └─────────────┘  └─────────────┘  └──────────┬──────────┘ │
│                                                │            │
│                                    ┌───────────▼──────────┐ │
│                                    │  Analysis Engine     │ │
│                                    │  (patterns, themes)  │ │
│                                    └───────────┬──────────┘ │
│                                                │            │
│                                    ┌───────────▼──────────┐ │
│                                    │ Intelligence Reporter│ │
│                                    │     (GPT-4/5)        │ │
│                                    └─────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```
</architecture>

<code_patterns>
<pattern name="browser-manager">
```python
class BrowserManager:
    def __init__(self, api_key: str, project_id: str, context_id: Optional[str] = None):
        self.bb = Browserbase(api_key=api_key)
        self.project_id = project_id
        self.context_id = context_id

    def create_session(self, enable_stealth=True, enable_proxy=True):
        config = {
            "projectId": self.project_id,
            "browserSettings": {
                "stealth": enable_stealth,
                "proxy": {"enabled": enable_proxy, "country": "us"}
            }
        }
        if self.context_id:
            config["contextId"] = self.context_id
        self.session = self.bb.sessions.create(**config)
        return self.session

    def connect_browser(self):
        self.playwright = sync_playwright().start()
        self.browser = self.playwright.chromium.connect_over_cdp(self.session.connectUrl)
        self.page = self.browser.contexts[0].pages[0]
        return self.page
```
</pattern>

<pattern name="context-auth">
```python
def create_authenticated_context(api_key, project_id, platform, credentials):
    bb = Browserbase(api_key=api_key)
    context = bb.contexts.create(projectId=project_id)

    # Login once, save to context
    with BrowserManager(api_key, project_id, context_id=context.id) as mgr:
        page = mgr.connect_browser()
        if platform == "linkedin":
            page.goto("https://www.linkedin.com/login")
            page.fill('input[name="session_key"]', credentials['email'])
            page.fill('input[name="session_password"]', credentials['password'])
            page.click('button[type="submit"]')
            page.wait_for_url("**/feed/**", timeout=30000)

    return context.id  # Reuse this for future sessions
```
</pattern>

<pattern name="linkedin-scraper">
```python
def scrape_linkedin_ads(page, company_name: str, max_ads: int = 20):
    page.goto("https://www.linkedin.com/ad-library", wait_until="networkidle")

    search_box = page.locator('input[aria-label*="Search"]')
    search_box.fill(company_name)
    search_box.press("Enter")
    page.wait_for_timeout(3000)

    ads_data = []
    while len(ads_data) < max_ads:
        ad_cards = page.locator('[data-test-id*="ad-card"]').all()
        for card in ad_cards:
            ad = {
                "platform": "linkedin",
                "company": company_name,
                "headline": card.locator('[class*="headline"]').text_content(),
                "body": card.locator('[class*="body"]').text_content(),
                "scraped_at": datetime.now().isoformat()
            }
            ads_data.append(ad)

        page.evaluate("window.scrollTo(0, document.body.scrollHeight)")
        page.wait_for_timeout(2000)

    return ads_data
```
</pattern>
</code_patterns>

<change_detection>
SQLite schema for tracking ads:
```sql
CREATE TABLE ads (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    competitor_id TEXT NOT NULL,
    platform TEXT NOT NULL,
    ad_identifier TEXT,
    headline TEXT,
    body TEXT,
    cta_text TEXT,
    image_url TEXT,
    landing_url TEXT,
    snapshot_date DATETIME NOT NULL,
    UNIQUE(competitor_id, platform, ad_identifier, snapshot_date)
);
```

Generate identifier: `ad_identifier = f"{headline}:{body}"[:200]`

Compare snapshots to detect new/changed ads.
</change_detection>

<intelligence_reports>
```python
prompt = f"""Generate an executive summary of competitive intelligence.

High Priority Changes ({len(high_severity)}):
{json.dumps(high_severity[:10], indent=2)}

Medium Priority Changes ({len(medium_severity)}):
{json.dumps(medium_severity[:10], indent=2)}

Provide:
1. **TL;DR**: 2-3 sentence summary
2. **Key Threats**
3. **Opportunities**
4. **Recommended Actions** (top 3)

Keep concise and actionable."""
```
</intelligence_reports>

<success_criteria>
- [ ] Browserbase session connects successfully
- [ ] Ad scraper extracts structured data from target platform
- [ ] Change detection identifies new/modified ads
- [ ] Intelligence report generates actionable insights
- [ ] Session replay URL available for debugging
</success_criteria>

<troubleshooting>
<issue problem="Bot detection / blocked">
Enable stealth mode and residential proxies in session config:
```python
"browserSettings": {"stealth": True, "proxy": {"enabled": True, "country": "us"}}
```
</issue>

<issue problem="Selectors not finding elements">
Use Browserbase session replay to watch what happened. Selectors may have changed—use multiple fallback selectors.
</issue>

<issue problem="Authentication not persisting">
Create a Browserbase Context, authenticate once, then pass `context_id` to future sessions.
</issue>
</troubleshooting>

<attribution>
Source: https://www.siddharthbharath.com/building-a-competitor-intelligence-agent-with-browserbase/
Author: Siddharth Bharath
</attribution>
