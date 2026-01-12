<workflow name="browser-setup">
<objective>
Set up Browserbase configuration, create authenticated contexts, and verify browser connectivity.
</objective>

<process>
<phase name="get-credentials">
<description>Get Browserbase API credentials</description>
<actions>
1. Sign up at https://browserbase.com
2. Go to Dashboard → Settings → API Keys
3. Copy your API key and Project ID
4. Create `.env` file:
```
BROWSERBASE_API_KEY=bb_live_xxxxx
BROWSERBASE_PROJECT_ID=proj_xxxxx
```
</actions>
</phase>

<phase name="install-dependencies">
<description>Install required packages</description>
<actions>
```bash
pip install browserbase playwright python-dotenv
playwright install chromium
```
</actions>
</phase>

<phase name="test-connection">
<description>Verify Browserbase connectivity</description>
<actions>
```python
from browserbase import Browserbase
from playwright.sync_api import sync_playwright
import os
from dotenv import load_dotenv

load_dotenv()

# Create session
bb = Browserbase(api_key=os.getenv("BROWSERBASE_API_KEY"))
session = bb.sessions.create(projectId=os.getenv("BROWSERBASE_PROJECT_ID"))

print(f"Session created: {session.id}")
print(f"Replay URL: https://www.browserbase.com/sessions/{session.id}")

# Connect with Playwright
pw = sync_playwright().start()
browser = pw.chromium.connect_over_cdp(session.connectUrl)
page = browser.contexts[0].pages[0]

# Test navigation
page.goto("https://example.com")
print(f"Page title: {page.title()}")

browser.close()
pw.stop()
```
</actions>
</phase>

<phase name="create-context">
<description>Create authenticated context for reuse</description>
<actions>
```python
def create_linkedin_context(api_key, project_id, email, password):
    bb = Browserbase(api_key=api_key)

    # Create persistent context
    context = bb.contexts.create(projectId=project_id)
    context_id = context.id
    print(f"Created context: {context_id}")

    # Authenticate
    session = bb.sessions.create(projectId=project_id, contextId=context_id)

    pw = sync_playwright().start()
    browser = pw.chromium.connect_over_cdp(session.connectUrl)
    page = browser.contexts[0].pages[0]

    # Login to LinkedIn
    page.goto("https://www.linkedin.com/login", wait_until="networkidle")
    page.fill('input[name="session_key"]', email)
    page.fill('input[name="session_password"]', password)
    page.click('button[type="submit"]')
    page.wait_for_url("**/feed/**", timeout=30000)

    print("Authenticated successfully!")

    browser.close()
    pw.stop()

    return context_id  # Save this for future sessions
```

Usage:
```python
# First time: create and authenticate
context_id = create_linkedin_context(API_KEY, PROJECT_ID, EMAIL, PASSWORD)

# Save context_id to config/env

# Future sessions: reuse context
session = bb.sessions.create(projectId=PROJECT_ID, contextId=context_id)
# No login needed - cookies are preserved!
```
</actions>
</phase>

<phase name="stealth-mode">
<description>Configure stealth and proxy settings</description>
<actions>
```python
session = bb.sessions.create(
    projectId=PROJECT_ID,
    browserSettings={
        "stealth": True,  # Avoid bot detection
        "proxy": {
            "enabled": True,
            "country": "us"  # Residential proxy
        }
    },
    timeout=300,  # 5 minute timeout
    keepAlive=False  # Close when done
)
```

Stealth mode:
- Randomizes browser fingerprint
- Mimics human behavior patterns
- Defeats common bot detection

Residential proxies:
- Uses real residential IPs
- Rotates automatically
- Appears as normal user traffic
</actions>
</phase>
</process>

<success_markers>
- Browserbase session creates successfully
- Playwright connects via CDP
- Navigation works
- Authenticated context persists across sessions
</success_markers>
</workflow>
