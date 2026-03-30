# 🦉 OInject

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-ORecon%20%2F%20OWebSec-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-2.0-cyan?style=flat-square"/>
</p>

> **OInject** is an XSS vulnerability scanner — it crawls a target web application, injects a curated low false-positive payload set into forms and GET parameters, and detects reflected XSS using strict executable-context matching.

---

> ⚠️ **AUTHORISED SECURITY TESTING AND BUG BOUNTY USE ONLY** — Do not scan systems you do not own or have written permission to test. Authorization is required before each scan.

---

## 📌 Overview

OInject discovers pages by crawling within the target domain, then tests every form field and GET parameter against 8 XSS payloads. Detection is strict — a payload must be reflected *and* appear inside an executable HTML context — reducing false positives significantly.

---

## 🖥️ Modules

| # | Module | Description |
|---|--------|-------------|
| **[1] Full Scan** | Crawl the target domain (configurable depth) then test all discovered forms and GET parameters |
| **[2] URL Scan** | Test a single URL — no crawl — forms and GET params only |
| **[3] Form Scan** | Test all forms found on a specific URL |
| **[4] GET Param Scan** | Test all GET parameters in a URL (must include `?param=value`) |
| **[5] Last Results** | View all findings from the current session |
| **[6] Export Results** | Save JSONL + text report to `oxss_output/` |

---

## 💉 Payload Set — 8 Strict Low-FP Payloads

```
<script>alert(document.domain)</script>
"><script>alert(1)</script>
'><script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg/onload=alert(1)>
javascript:alert(document.cookie)
<details open ontoggle=alert(1)>
';alert(1);//
```

---

## 🔍 Detection Logic

A finding is only raised when the payload is:

1. **Reflected** in the response body, AND
2. **Appears in an executable context** matching one of these patterns:

| Pattern | Context |
|---------|---------|
| `<script …>…payload` | Script tag execution |
| `on*="…payload` | Inline event handler |
| `javascript:…payload` | JavaScript URI |
| `<img onerror=…payload` | Image error handler |
| `<svg onload=…payload` | SVG load handler |

---

## 🛡️ Safety & Politeness

| Feature | Detail |
|---------|--------|
| **Polite delays** | 0.5–1.3s random delay between crawl requests, 150ms between injections |
| **Internal-domain only** | Crawler never leaves the target domain |
| **Safe params skipped** | Parameters like `page`, `id`, `sort`, `order`, `cat` are not injected |
| **Known FP endpoints skipped** | `search.php`, `search`, `find`, `query` actions excluded |
| **Authorization gate** | Explicit confirmation required before every scan |
| **Graceful stop** | Ctrl+C safely stops the scan and shows partial results |

---

## 📤 Export

All output is saved to `oxss_output/`:

| File | Contents |
|------|----------|
| `oxss_findings_YYYYMMDD_HHMMSS.jsonl` | One JSON object per finding — type, page, payload, method, action, param, HTTP status, response URL, timestamp |
| `oxss_report_YYYYMMDD_HHMMSS.txt` | Human-readable report with target, total findings, and full detail per finding |

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable

---

## 🚀 Usage

```bash
./OInject
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
