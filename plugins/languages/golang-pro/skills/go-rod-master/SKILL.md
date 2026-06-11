---
name: go-rod-master
description: >
  Use when automating browsers or scraping with go-rod (Chrome DevTools Protocol), when the project imports github.com/go-rod/rod, or when migrating from chromedp, or when stealth anti-bot detection (Cloudflare), request hijacking, page pools, or headless Chrome lifecycle management are needed. For playwright-go projects use the go-playwright skill instead.
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(go:*)
version: 1.0.0
license: MIT
tags:
  - go-ecosystem
  - golang
  - go-rod
  - browser-automation
  - scraping
  - cdp
compatibility: Designed for Claude Code.
---

# Go-Rod Browser Automation Master

## Overview

[Rod](https://github.com/go-rod/rod) is a high-level Go driver built directly on the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) for browser automation and web scraping. Unlike wrappers around other tools, Rod communicates with the browser natively via CDP, providing thread-safe operations, chained context design for timeouts/cancellation, auto-wait for elements, correct iframe/shadow DOM handling, and zero zombie browser processes.

The companion library [go-rod/stealth](https://github.com/go-rod/stealth) injects anti-bot-detection evasions based on [puppeteer-extra stealth](https://github.com/berstend/puppeteer-extra/tree/master/packages/extract-stealth-evasions), hiding headless browser fingerprints from detection systems.

## Installation

```bash
# Core rod library
go get github.com/go-rod/rod@latest

# Stealth anti-detection plugin (ALWAYS include for production scraping)
go get github.com/go-rod/stealth@latest
```

Rod auto-downloads a compatible Chromium binary on first run. To pre-download:

```bash
go run github.com/go-rod/rod/lib/utils/get-browser@latest
```

## Browser Lifecycle

Rod manages three layers: **Browser → Page → Element**.

```go
browser := rod.New().MustConnect()
defer browser.MustClose()

page := browser.MustPage("https://example.com")
el := page.MustElement("h1")
fmt.Println(el.MustText())
```

Rod provides two API styles:

| Style | Example | Use Case |
|:------|:--------|:---------|
| **Must** | `MustElement()`, `MustClick()` | Prototyping. Panics on error. |
| **Error** | `Element()`, `Click()` | Production. Returns `error`. |

Use `.Timeout(d)` to set per-chain deadlines. Context propagates recursively to all child operations.

## Stealth & Anti-Bot Detection

> **ALWAYS use `stealth.MustPage(browser)` instead of `browser.MustPage()` for any production scraping.** This is the single most important step.

The `go-rod/stealth` package injects JS evasions that remove `navigator.webdriver`, spoof WebGL vendor/renderer, fix `PluginArray` type, patch the permissions API, set realistic `Accept-Language`, and fix broken image dimensions reported by headless Chrome.

```go
import (
    "github.com/go-rod/rod"
    "github.com/go-rod/stealth"
)

browser := rod.New().MustConnect()
defer browser.MustClose()

page := stealth.MustPage(browser)
page.MustNavigate("https://bot.sannysoft.com")
```

**Verify stealth is working:**

```go
page := stealth.MustPage(browser)
page.MustNavigate("https://bot.sannysoft.com")
page.MustScreenshot("stealth_test.png")
// Expect: WebDriver missing, Plugins Length 3, Languages en-US,en
```

**Inject stealth manually (when you must create the page yourself):**

```go
page := browser.MustPage()
page.MustEvalOnNewDocument(stealth.JS)
page.MustNavigate("https://example.com")
```

## Top Pitfalls

- **Element not found even though it exists** — it may be inside an iframe or shadow DOM. Use `page.MustSearch()` instead of `page.MustElement()`; it searches across all frames.
- **Click does nothing on an animating element** — call `el.MustWaitStable()` before `el.MustClick()`.
- **Still detected despite stealth** — combine `stealth.MustPage()` with randomized viewport sizes, realistic User-Agent, human-like input delays, and residential proxies.
- **Browser zombie processes** — always `defer browser.MustClose()`. Rod uses [leakless](https://github.com/ysmood/leakless) as a backstop, but explicit cleanup is required.
- **Timeouts on slow/AJAX-heavy pages** — use `MustWaitRequestIdle()` instead of `MustWaitLoad()` after actions that trigger fetches.
- **HijackRequests not intercepting** — you must call `go router.Run()` after adding routes, and `defer router.MustStop()` for cleanup.
- **Never use `time.Sleep()`** — use Rod's built-in wait methods (`MustWaitLoad`, `MustWaitStable`, `MustWaitRequestIdle`, `MustWait`).

## Implementation Guides (references/)

Detailed patterns with full code samples are in [`references/implementation-guides.md`](references/implementation-guides.md):

- Launcher configuration & debugging mode (flags, proxy, slow-motion)
- Proxy support & auth handling
- Input simulation (keyboard, mouse)
- Network request interception / hijacking
- Waiting strategies (load, idle, stable, invisible, JS condition, events)
- Race selectors for multi-outcome pages
- Screenshots & PDF generation
- Concurrent page pool (`rod.NewPagePool`)
- Event handling & console capture
- File download
- JavaScript evaluation & direct CDP calls
- Loading Chrome extensions (headless limitations)
- iframe & shadow DOM navigation

## Examples

See the `examples/` directory for complete, runnable Go files:
- `examples/basic_scrape.go` — Minimal scraping example
- `examples/stealth_page.go` — Anti-detection with go-rod/stealth
- `examples/request_hijacking.go` — Intercepting and modifying network requests
- `examples/concurrent_pages.go` — Page pool for concurrent scraping

## Documentation References

- [Official Documentation](https://go-rod.github.io/) — Guides, tutorials, FAQ
- [Go API Reference](https://pkg.go.dev/github.com/go-rod/rod) — Complete type and method documentation
- [go-rod/stealth](https://github.com/go-rod/stealth) — Anti-bot detection plugin
- [Examples (source)](https://github.com/go-rod/rod/blob/main/examples_test.go) — Official example tests
- [Rod vs Chromedp Comparison](https://github.com/go-rod/rod/tree/main/lib/examples/compare-chromedp) — Migration reference
- [Chrome DevTools Protocol Docs](https://chromedevtools.github.io/devtools-protocol/) — Underlying protocol reference
- [Chrome CLI Flags Reference](https://peter.sh/experiments/chromium-command-line-switches) — Launcher flag documentation
- `references/api-reference.md` — Quick-reference cheat sheet
