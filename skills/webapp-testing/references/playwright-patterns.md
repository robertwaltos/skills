# Advanced Playwright Testing Patterns

A comprehensive reference for writing robust, maintainable Playwright tests.

---

## Part I: Selector Strategies

### Selector Priority (Best to Worst)

| Priority | Selector Type | Example | Reliability |
|----------|---------------|---------|-------------|
| 1 | Role + Name | `page.get_by_role("button", name="Submit")` | ⭐⭐⭐⭐⭐ |
| 2 | Label | `page.get_by_label("Email")` | ⭐⭐⭐⭐⭐ |
| 3 | Placeholder | `page.get_by_placeholder("Enter email")` | ⭐⭐⭐⭐ |
| 4 | Text | `page.get_by_text("Welcome back")` | ⭐⭐⭐⭐ |
| 5 | Test ID | `page.get_by_test_id("login-button")` | ⭐⭐⭐⭐ |
| 6 | CSS | `page.locator(".btn-primary")` | ⭐⭐⭐ |
| 7 | XPath | `page.locator("//button[@type='submit']")` | ⭐⭐ |

### Role-Based Selectors (Preferred)

```python
# Buttons
page.get_by_role("button", name="Submit")
page.get_by_role("button", name=re.compile(r"submit", re.IGNORECASE))

# Links
page.get_by_role("link", name="Home")

# Form elements
page.get_by_role("textbox", name="Email")
page.get_by_role("checkbox", name="Remember me")
page.get_by_role("combobox", name="Country")

# Navigation
page.get_by_role("navigation").get_by_role("link", name="About")

# Headings
page.get_by_role("heading", name="Welcome", level=1)
```

### Chaining Locators

```python
# Filter within context
page.get_by_role("listitem").filter(has_text="Product A").get_by_role("button", name="Add to cart")

# Parent-child relationship
page.locator(".card").filter(has=page.get_by_text("Premium")).get_by_role("button")

# Multiple filters
page.get_by_role("row").filter(has_text="John").filter(has_text="Admin")
```

---

## Part II: Waiting Strategies

### Automatic Waiting (Default)

Playwright auto-waits for:
- Element to be attached to DOM
- Element to be visible
- Element to be stable (not animating)
- Element to receive events

### Explicit Waits

```python
# Wait for element state
await page.get_by_role("button").wait_for(state="visible")
await page.get_by_role("button").wait_for(state="hidden")
await page.get_by_role("button").wait_for(state="attached")
await page.get_by_role("button").wait_for(state="detached")

# Wait for load state
await page.wait_for_load_state("networkidle")  # Most common for SPAs
await page.wait_for_load_state("domcontentloaded")
await page.wait_for_load_state("load")

# Wait for specific URL
await page.wait_for_url("**/dashboard")
await page.wait_for_url(re.compile(r"/user/\d+"))

# Wait for response
async with page.expect_response("**/api/users") as response_info:
    await page.get_by_role("button", name="Load Users").click()
response = await response_info.value
assert response.status == 200
```

### Custom Wait Conditions

```python
# Wait for function to return truthy
await page.wait_for_function("document.querySelector('.loaded')")
await page.wait_for_function("() => window.dataReady === true")

# Wait with polling
await expect(page.get_by_role("status")).to_have_text("Complete", timeout=30000)
```

---

## Part III: Assertions

### Element Assertions

```python
from playwright.sync_api import expect

# Visibility
expect(page.get_by_role("alert")).to_be_visible()
expect(page.get_by_role("alert")).to_be_hidden()

# Text content
expect(page.get_by_role("heading")).to_have_text("Welcome")
expect(page.get_by_role("heading")).to_contain_text("Welcome")
expect(page.get_by_role("status")).to_have_text(re.compile(r"\d+ items"))

# Attributes
expect(page.get_by_role("button")).to_be_enabled()
expect(page.get_by_role("button")).to_be_disabled()
expect(page.get_by_role("checkbox")).to_be_checked()
expect(page.get_by_role("textbox")).to_have_value("test@example.com")
expect(page.get_by_role("link")).to_have_attribute("href", "/about")

# CSS
expect(page.get_by_role("alert")).to_have_class(re.compile(r"error"))
expect(page.get_by_role("button")).to_have_css("background-color", "rgb(255, 0, 0)")

# Count
expect(page.get_by_role("listitem")).to_have_count(5)
```

### Page Assertions

```python
# URL
expect(page).to_have_url("https://example.com/dashboard")
expect(page).to_have_url(re.compile(r"/user/\d+"))

# Title
expect(page).to_have_title("Dashboard | My App")
expect(page).to_have_title(re.compile(r"Dashboard"))
```

---

## Part IV: Network Interception

### Mocking API Responses

```python
# Mock specific endpoint
await page.route("**/api/users", lambda route: route.fulfill(
    status=200,
    content_type="application/json",
    body='[{"id": 1, "name": "Test User"}]'
))

# Mock with file
await page.route("**/api/products", lambda route: route.fulfill(
    path="fixtures/products.json"
))

# Conditional mocking
async def handle_route(route):
    if "error" in route.request.url:
        await route.fulfill(status=500, body="Server Error")
    else:
        await route.continue_()

await page.route("**/api/**", handle_route)
```

### Request Interception

```python
# Abort requests (block ads, analytics)
await page.route("**/*", lambda route: route.abort() if "analytics" in route.request.url else route.continue_())

# Modify requests
async def modify_request(route):
    headers = {**route.request.headers, "X-Custom-Header": "test"}
    await route.continue_(headers=headers)

await page.route("**/api/**", modify_request)
```

### Capturing Network Traffic

```python
# Log all requests
page.on("request", lambda req: print(f">> {req.method} {req.url}"))
page.on("response", lambda res: print(f"<< {res.status} {res.url}"))

# Capture specific API calls
responses = []
page.on("response", lambda res: responses.append(res) if "/api/" in res.url else None)
```

---

## Part V: Advanced Interactions

### File Upload

```python
# Single file
page.get_by_label("Upload").set_input_files("file.pdf")

# Multiple files
page.get_by_label("Upload").set_input_files(["file1.pdf", "file2.pdf"])

# Clear files
page.get_by_label("Upload").set_input_files([])

# From buffer
page.get_by_label("Upload").set_input_files({
    "name": "test.txt",
    "mimeType": "text/plain",
    "buffer": b"Hello World"
})
```

### Drag and Drop

```python
# Simple drag
await page.drag_and_drop("#source", "#target")

# With locators
source = page.get_by_text("Drag me")
target = page.get_by_test_id("drop-zone")
await source.drag_to(target)
```

### Keyboard Actions

```python
# Type with delay (human-like)
await page.get_by_role("textbox").type("Hello", delay=100)

# Key combinations
await page.keyboard.press("Control+a")
await page.keyboard.press("Meta+c")  # Cmd on Mac
await page.keyboard.press("Shift+Tab")

# Hold and release
await page.keyboard.down("Shift")
await page.get_by_role("listitem").first.click()
await page.get_by_role("listitem").last.click()
await page.keyboard.up("Shift")
```

### Mouse Actions

```python
# Hover
await page.get_by_role("button").hover()

# Click positions
await page.get_by_role("canvas").click(position={"x": 100, "y": 50})

# Double click, right click
await page.get_by_role("listitem").dblclick()
await page.get_by_role("listitem").click(button="right")

# Wheel scroll
await page.mouse.wheel(0, 500)  # Scroll down
```

---

## Part VI: Multi-Context Testing

### Multiple Pages/Tabs

```python
# Handle popup
async with page.expect_popup() as popup_info:
    await page.get_by_role("link", name="Open").click()
popup = await popup_info.value
await popup.wait_for_load_state()
await expect(popup.get_by_role("heading")).to_have_text("Popup Content")
```

### Frames and iFrames

```python
# Access frame by name
frame = page.frame("frame-name")

# Access frame by URL
frame = page.frame(url=re.compile(r"payment"))

# Frame locator (recommended)
await page.frame_locator("iframe.payment").get_by_role("button", name="Pay").click()
```

### Browser Contexts (Isolation)

```python
# Create isolated context
context = await browser.new_context(
    viewport={"width": 1920, "height": 1080},
    user_agent="Custom UA",
    locale="en-US",
    timezone_id="America/New_York"
)
page = await context.new_page()

# Multiple users
admin_context = await browser.new_context(storage_state="admin-auth.json")
user_context = await browser.new_context(storage_state="user-auth.json")
```

---

## Part VII: Debugging Techniques

### Screenshots and Videos

```python
# Full page screenshot
await page.screenshot(path="screenshot.png", full_page=True)

# Element screenshot
await page.get_by_role("card").screenshot(path="card.png")

# On failure screenshot (in fixture)
yield page
if request.node.rep_call.failed:
    await page.screenshot(path=f"failure-{request.node.name}.png")
```

### Tracing

```python
# Start tracing
await context.tracing.start(screenshots=True, snapshots=True, sources=True)

# ... run test ...

# Stop and save
await context.tracing.stop(path="trace.zip")

# View trace: npx playwright show-trace trace.zip
```

### Console Logging

```python
# Capture console messages
page.on("console", lambda msg: print(f"CONSOLE: {msg.type}: {msg.text}"))

# Capture errors
page.on("pageerror", lambda error: print(f"PAGE ERROR: {error}"))
```

### Pause for Debugging

```python
# Pause execution (opens inspector)
await page.pause()
```

---

## Part VIII: Test Organization Patterns

### Page Object Model

```python
class LoginPage:
    def __init__(self, page):
        self.page = page
        self.email = page.get_by_label("Email")
        self.password = page.get_by_label("Password")
        self.submit = page.get_by_role("button", name="Sign In")
        self.error = page.get_by_role("alert")
    
    async def login(self, email, password):
        await self.email.fill(email)
        await self.password.fill(password)
        await self.submit.click()
    
    async def expect_error(self, message):
        await expect(self.error).to_have_text(message)

# Usage
login_page = LoginPage(page)
await login_page.login("test@example.com", "password")
await login_page.expect_error("Invalid credentials")
```

### Fixtures for Authentication

```python
# Save auth state
await page.goto("/login")
await page.get_by_label("Email").fill("admin@example.com")
await page.get_by_label("Password").fill("password")
await page.get_by_role("button", name="Sign In").click()
await page.wait_for_url("/dashboard")
await page.context.storage_state(path="auth.json")

# Reuse auth state
context = await browser.new_context(storage_state="auth.json")
page = await context.new_page()
# Now logged in!
```

---

*This reference covers professional-grade Playwright patterns. Use these techniques to build reliable, maintainable test automation that catches real bugs.*
