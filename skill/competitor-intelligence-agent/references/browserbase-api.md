<reference name="browserbase-api">
<objective>
Quick reference for Browserbase Python SDK and key concepts.
</objective>

<core_concepts>
<concept name="sessions">
A Session is a browser instance. Create → connect → use → close.

```python
from browserbase import Browserbase

bb = Browserbase(api_key="bb_live_xxx")
session = bb.sessions.create(projectId="proj_xxx")

# session.id - unique identifier
# session.connectUrl - CDP endpoint for Playwright
```
</concept>

<concept name="contexts">
A Context persists browser state (cookies, localStorage) across sessions.

```python
# Create context
context = bb.contexts.create(projectId="proj_xxx")

# Use context in session
session = bb.sessions.create(
    projectId="proj_xxx",
    contextId=context.id
)
# Session inherits all stored auth state
```

Use cases:
- Persist login sessions
- Maintain shopping carts
- Keep preferences across runs
</concept>

<concept name="session-replays">
Every session is recorded automatically.

```python
replay_url = f"https://www.browserbase.com/sessions/{session.id}"
```

Use for:
- Debugging failed scrapers
- Verifying bot behavior
- Auditing automation runs
</concept>
</core_concepts>

<session_options>
```python
session = bb.sessions.create(
    projectId="proj_xxx",          # Required
    contextId="ctx_xxx",           # Optional: persist state
    browserSettings={
        "stealth": True,           # Anti-bot detection
        "proxy": {
            "enabled": True,
            "country": "us"        # us, uk, de, etc.
        }
    },
    timeout=300,                   # Seconds before auto-close
    keepAlive=False                # Keep open after disconnect?
)
```
</session_options>

<playwright_connection>
```python
from playwright.sync_api import sync_playwright

# Connect to Browserbase session
pw = sync_playwright().start()
browser = pw.chromium.connect_over_cdp(session.connectUrl)

# Get page from existing context
context = browser.contexts[0]
page = context.pages[0]

# Use normally
page.goto("https://example.com")
page.fill("input", "text")
page.click("button")

# Cleanup
browser.close()
pw.stop()
```
</playwright_connection>

<common_patterns>
<pattern name="context-manager">
```python
class BrowserManager:
    def __enter__(self):
        self.session = bb.sessions.create(...)
        self.pw = sync_playwright().start()
        self.browser = self.pw.chromium.connect_over_cdp(self.session.connectUrl)
        return self

    def __exit__(self, *args):
        self.browser.close()
        self.pw.stop()

# Usage
with BrowserManager() as mgr:
    page = mgr.browser.contexts[0].pages[0]
    page.goto("...")
```
</pattern>

<pattern name="error-handling">
```python
try:
    session = bb.sessions.create(projectId=PROJECT_ID)
except Exception as e:
    print(f"Session creation failed: {e}")
    # Check API key, project ID, quota
```
</pattern>
</common_patterns>

<debugging_tips>
1. Always print replay URL on session creation
2. Use `page.screenshot()` at failure points
3. Add `page.wait_for_timeout(1000)` for dynamic content
4. Check session replay for exact failure moment
5. Verify selectors in browser DevTools first
</debugging_tips>
</reference>
