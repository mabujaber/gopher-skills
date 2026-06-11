# Go-Rod Implementation Guides

Detailed reference for advanced go-rod patterns. Loaded on demand — the skill body covers the 20% you need 80% of the time.

---

## 1. Launcher Configuration

Use the `launcher` package to customize browser launch flags:

```go
import "github.com/go-rod/rod/lib/launcher"

url := launcher.New().
    Headless(true).             // false for debugging
    Proxy("127.0.0.1:8080").    // upstream proxy
    Set("disable-gpu", "").     // custom Chrome flag
    Delete("use-mock-keychain"). // remove a default flag
    MustLaunch()

browser := rod.New().ControlURL(url).MustConnect()
defer browser.MustClose()
```

**Debugging mode (visible browser + slow motion):**

```go
l := launcher.New().
    Headless(false).
    Devtools(true)
defer l.Cleanup()

browser := rod.New().
    ControlURL(l.MustLaunch()).
    Trace(true).
    SlowMotion(2 * time.Second).
    MustConnect()
```

---

## 2. Proxy Support

```go
// Set proxy at launch
url := launcher.New().
    Proxy("socks5://127.0.0.1:1080").
    MustLaunch()

browser := rod.New().ControlURL(url).MustConnect()

// Handle proxy authentication
go browser.MustHandleAuth("username", "password")()

// Ignore SSL certificate errors (for MITM proxies)
browser.MustIgnoreCertErrors(true)
```

---

## 3. Input Simulation

```go
import "github.com/go-rod/rod/lib/input"

// Type into an input field (replaces existing value)
page.MustElement("#email").MustInput("user@example.com")

// Simulate keyboard keys
page.Keyboard.MustType(input.Enter)

// Press key combinations
page.Keyboard.MustPress(input.ControlLeft)
page.Keyboard.MustType(input.KeyA)
page.Keyboard.MustRelease(input.ControlLeft)

// Mouse click at coordinates
page.Mouse.MustClick(input.MouseLeft)
page.Mouse.MustMoveTo(100, 200)
```

---

## 4. Network Request Interception (Hijacking)

```go
router := browser.HijackRequests()
defer router.MustStop()

// Block all image requests
router.MustAdd("*.png", func(ctx *rod.Hijack) {
    ctx.Response.Fail(proto.NetworkErrorReasonBlockedByClient)
})

// Modify request headers
router.MustAdd("*api.example.com*", func(ctx *rod.Hijack) {
    ctx.Request.Req().Header.Set("Authorization", "Bearer token123")
    ctx.MustLoadResponse()
})

// Modify response body
router.MustAdd("*.js", func(ctx *rod.Hijack) {
    ctx.MustLoadResponse()
    ctx.Response.SetBody(ctx.Response.Body() + "\n// injected")
})

go router.Run()
```

Note: you must call `go router.Run()` after setting up routes, and `defer router.MustStop()` for cleanup.

---

## 5. Waiting Strategies

```go
// Wait for page load event
page.MustWaitLoad()

// Wait for no pending network requests (AJAX idle)
wait := page.MustWaitRequestIdle()
page.MustElement("#search").MustInput("query")
wait()

// Wait for element to be stable (not animating)
page.MustElement(".modal").MustWaitStable().MustClick()

// Wait for element to become invisible
page.MustElement(".loading").MustWaitInvisible()

// Wait for JavaScript condition
page.MustWait(`() => document.title === 'Ready'`)

// Wait for specific navigation/event
wait := page.WaitEvent(&proto.PageLoadEventFired{})
page.MustNavigate("https://example.com")
wait()
```

---

## 6. Race Selectors (Multiple Outcomes)

Handle pages where the result can be one of several outcomes (e.g., login success vs error):

```go
page.MustElement("#username").MustInput("user")
page.MustElement("#password").MustInput("pass").MustType(input.Enter)

// Race between success and error selectors
elm := page.Race().
    Element(".dashboard").MustHandle(func(e *rod.Element) {
        fmt.Println("Login successful:", e.MustText())
    }).
    Element(".error-message").MustDo()

if elm.MustMatches(".error-message") {
    log.Fatal("Login failed:", elm.MustText())
}
```

---

## 7. Screenshots & PDF Generation

```go
// Full-page screenshot
page.MustScreenshot("page.png")

// Custom screenshot (JPEG, specific region)
img, _ := page.Screenshot(true, &proto.PageCaptureScreenshot{
    Format:  proto.PageCaptureScreenshotFormatJpeg,
    Quality: gson.Int(90),
    Clip: &proto.PageViewport{
        X: 0, Y: 0, Width: 1280, Height: 800, Scale: 1,
    },
})
utils.OutputFile("screenshot.jpg", img)

// Scroll screenshot (captures full scrollable page)
img, _ := page.MustWaitStable().ScrollScreenshot(nil)
utils.OutputFile("full_page.jpg", img)

// PDF export
page.MustPDF("output.pdf")
```

---

## 8. Concurrent Page Pool

```go
pool := rod.NewPagePool(5) // max 5 concurrent pages

create := func() *rod.Page {
    return browser.MustIncognito().MustPage()
}

var wg sync.WaitGroup
for _, url := range urls {
    wg.Add(1)
    go func(u string) {
        defer wg.Done()

        page := pool.MustGet(create)
        defer pool.Put(page)

        page.MustNavigate(u).MustWaitLoad()
        fmt.Println(page.MustInfo().Title)
    }(url)
}
wg.Wait()

pool.Cleanup(func(p *rod.Page) { p.MustClose() })
```

---

## 9. Event Handling

```go
// Listen for console.log output
go page.EachEvent(func(e *proto.RuntimeConsoleAPICalled) {
    if e.Type == proto.RuntimeConsoleAPICalledTypeLog {
        fmt.Println(page.MustObjectsToJSON(e.Args))
    }
})()

// Wait for a specific event before proceeding
wait := page.WaitEvent(&proto.PageLoadEventFired{})
page.MustNavigate("https://example.com")
wait()
```

---

## 10. File Download

```go
wait := browser.MustWaitDownload()

page.MustElementR("a", "Download PDF").MustClick()

data := wait()
utils.OutputFile("downloaded.pdf", data)
```

---

## 11. JavaScript Evaluation

```go
// Execute JS on the page
page.MustEval(`() => console.log("hello")`)

// Pass parameters and get return value
result := page.MustEval(`(a, b) => a + b`, 1, 2)
fmt.Println(result.Int()) // 3

// Eval on a specific element ("this" = the DOM element)
title := page.MustElement("title").MustEval(`() => this.innerText`).String()

// Direct CDP calls for features Rod doesn't wrap
proto.PageSetAdBlockingEnabled{Enabled: true}.Call(page)
```

---

## 12. Loading Chrome Extensions

```go
extPath, _ := filepath.Abs("./my-extension")

u := launcher.New().
    Set("load-extension", extPath).
    Headless(false). // extensions require headed mode
    MustLaunch()

browser := rod.New().ControlURL(u).MustConnect()
```

Note: Chrome extensions do not work in headless mode. Use `Headless(false)` with XVFB for server environments.

---

## iframe & Shadow DOM

- `page.MustSearch(".deeply-nested-element")` searches across all iframes and shadow DOMs (equivalent to DevTools Ctrl+F). Use this instead of `MustElement` when an element is not found at the top-level DOM.
- `MustElement` only searches the top-level document; `MustSearch` is the safe default for unknown page structures.
