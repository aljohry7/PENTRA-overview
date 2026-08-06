# PENTRA

**An all-in-one web application security testing platform — reconnaissance, scanning, exploitation support, and reporting in a single, coherent workspace.**

> This repository is a **public overview** of PENTRA. It explains what the platform does and who it is for.
> The **source code is kept in a private repository** and is not published here.

---

## What is PENTRA?

PENTRA is a unified offensive-security workspace that brings the tools a penetration tester reaches for every day into one clean, modern interface. Instead of juggling a dozen separate command-line tools and copy-pasting output between them, PENTRA orchestrates them behind a single web UI, normalizes their results, and keeps everything tied to the project you are working on.

It wraps and coordinates industry-standard tooling (nmap, hydra, netexec, medusa, mitmproxy, wafw00f, and the SecLists corpus, among others) and adds its own engines for crawling, request interception, credential testing, and adaptive scanning — with results streamed live and stored per project.

---

## Who is it for?

PENTRA is built for people who test systems **they are authorized to test**:

- **Penetration testers & red teams** — one workspace for recon → scan → validate → report.
- **Bug-bounty hunters** — fast recon, an intercepting proxy, and a request editor without leaving the browser.
- **Security students & CTF players** — approachable UI over powerful tooling, plus safe, sandboxed teaching labs.
- **Blue teams & defenders** — understand how these techniques look from the attacker's side to defend against them.

> PENTRA is strictly for **authorized security testing and education**. Every module carries an authorization notice. Using it against systems you do not own or have explicit permission to test is illegal.

---

## Core capabilities

### Reconnaissance & discovery
- **Recon** — subdomain enumeration, DNS/IP resolution, technology fingerprinting, and JavaScript intelligence (endpoint & secret discovery from crawled JS).
- **Crawler** — headless-browser crawling that captures pages, forms, and API endpoints for later testing.
- **Interceptor** — a Playwright-driven browser session that records real traffic and login flows.

### Scanning & detection
- **Server Scan** — an **adaptive multi-phase nmap engine** that classifies firewall/IDS/IPS resistance and escalates evasion techniques automatically. Supports **multiple targets at once** (individual IPs, CIDR networks, ranges, and domains), performs a fast host-discovery sweep, then deep-scans live hosts in parallel — and surfaces the exact winning command for transparency.
- **Web Scanner** — crawler + passive checks + active checks, with an issues board and reporting.
- **WAF detection** — identifies web application firewalls and their vendors.
- **Vulnerability & CVE lookup** — maps detected service versions to known CVEs.
- **BAC (Broken Access Control)** — automated authorization-boundary testing.

### Manual tooling
- **Brute Force** — a professional, protocol-aware credential-testing module supporting **multiple engines (hydra · netexec · medusa)** with automatic recommendation of the right tool per protocol (e.g. netexec for SMB/RDP to avoid legacy-protocol failures and false positives). Includes modern rate-control designed to avoid tripping lockouts and detection: request delay + jitter, concurrency limiting, proxy and User-Agent rotation, header spoofing, exponential backoff on rate limits, and lockout-detection auto-stop. Handles ~20 protocols and integrates the SecLists wordlist catalog.
- **Intercepting Proxy** — a Burp/ZAP-style proxy with history, request inspection, and a request editor.
- **cURL client** — a full-featured HTTP request builder with sessions, templates, and traffic inspection.
- **Shell** — managed post-exploitation shell sessions.

### Teaching labs (sandboxed)
- **HTML Injection Lab** and **CSRF Lab** — deliberately vulnerable, **localhost-gated** modules for learning how these vulnerability classes work in a safe environment. These are excluded from any production build.

### Reporting
- **Report** — consolidates findings across modules into a shareable report per project.

---

## Design

PENTRA uses a calm, professional **light "Cool Steel / Graphite" design system** — a single coordinated theme with a coherent token set, one icon library, keyboard-navigable controls, and WCAG-AA contrast. The goal is a tool that feels like a finished product, not a script with a UI bolted on.

---

## Architecture

| Layer | Technology |
|-------|-----------|
| **Backend** | Python · FastAPI · async orchestration of external security tools |
| **Storage** | SQLite (per-project state, scan results, history) |
| **Frontend** | React 18 · TypeScript · Vite · Zustand · a tokenized design system |
| **Delivery** | Backend serves the built single-page app — one process, one port |
| **Tooling wrapped** | nmap · hydra · netexec · medusa · mitmproxy · wafw00f · SecLists |

The backend is organized into independent routers and services per capability, so modules can evolve without touching each other. Long-running work (scans, brute-force jobs, crawls) runs as background jobs with live progress streamed to the UI.

---

## Project status

- ✅ Active development.
- 🔒 **Source code is private.** This overview exists so the project can be understood without exposing the implementation.
- 🧩 Modular — new capabilities are added as self-contained modules.

---

## Responsible use

PENTRA is a dual-use security tool intended for **authorized penetration testing, security research, and education only**. You are responsible for ensuring you have explicit permission to test any target. The authors do not condone and are not responsible for misuse.

---
---

<div dir="rtl">

## نظرة عامة (بالعربية)

**PENTRA** منصّة متكاملة لاختبار أمن تطبيقات الويب، تجمع الأدوات التي يحتاجها مختبِر الاختراق يوميًا في **واجهة واحدة حديثة ومنسّقة** — بدل التنقّل بين عشرات الأدوات في سطر الأوامر ونسخ النتائج يدويًا.

### ماذا يفعل المشروع؟
ينسّق PENTRA أدوات أمنية قياسية (nmap · hydra · netexec · medusa · mitmproxy · SecLists) خلف واجهة واحدة، ويضيف محرّكاته الخاصة للزحف واعتراض الطلبات واختبار بيانات الاعتماد والفحص التكيّفي — مع عرض النتائج مباشرةً وحفظها لكل مشروع.

### من يخدم؟
- **مختبِرو الاختراق والفِرق الحمراء** — مساحة عمل واحدة: استطلاع ← فحص ← تحقّق ← تقرير.
- **صيّادو المكافآت (Bug Bounty)** — استطلاع سريع وبروكسي اعتراض ومحرّر طلبات داخل المتصفّح.
- **طلاب الأمن ولاعبو CTF** — واجهة سهلة فوق أدوات قوية، مع **مختبرات تعليمية آمنة ومعزولة**.
- **المدافعون (Blue Team)** — فهم كيف تبدو هذه التقنيات من جهة المهاجم للدفاع ضدّها.

### أبرز الوحدات
- **الاستطلاع:** حصر النطاقات الفرعية، بصمة التقنيات، استخراج نقاط النهاية والأسرار من JavaScript.
- **فحص الخوادم:** محرّك nmap تكيّفي متعدّد المراحل، يصنّف مقاومة الجدار الناري/IDS/IPS ويصعّد تقنيات التهرّب تلقائيًا — ويدعم **عدّة أهداف معًا** (IP · شبكات CIDR · نطاقات · دومينات) مع اكتشاف الأجهزة الحيّة أولًا ثم فحصها بالتوازي.
- **Brute Force:** وحدة احترافية متعدّدة الأدوات (hydra · netexec · medusa) تختار الأداة المناسبة لكل بروتوكول تلقائيًا، مع تقنيات حديثة لتفادي الحظر والكشف (تأخير + jitter، تدوير بروكسي وUser-Agent، backoff، إيقاف تلقائي عند رصد القفل).
- **بروكسي اعتراض + عميل cURL + Web Scanner + WAF + BAC + Shell + التقارير.**
- **مختبرات تعليمية معزولة** (HTML Injection / CSRF) تعمل على localhost فقط.

### حالة المشروع
- 🔒 **الكود المصدري خاص.** هذا المستودع للشرح فقط، ليَفهم الآخرون المشروع دون كشف التنفيذ.
- ✅ تطوير نشِط · وحدات مستقلّة قابلة للتوسّع.

### الاستخدام المسؤول
PENTRA أداة أمنية ثنائية الاستخدام، **للاختبار المصرّح به والبحث الأمني والتعليم فقط**. أنت مسؤول عن امتلاك تصريح صريح لاختبار أي هدف.

</div>
