# PENTRA

**An all-in-one web application & network security testing platform — reconnaissance, scanning, exploitation, and reporting in a single, coherent workspace.**

> This repository is a **public overview** of PENTRA. It explains what the platform does and who it is for.
> The **source code is kept in a private repository** and is not published here.

---

## What is PENTRA?

PENTRA is a unified offensive-security workspace that brings the tools a penetration tester reaches for every day into one clean, modern interface. Instead of juggling a dozen separate command-line tools and copy-pasting output between them, PENTRA orchestrates them behind a single web UI, normalizes their results, and keeps everything tied to the project you are working on.

It wraps and coordinates industry-standard tooling (nmap, hydra, netexec, medusa, sqlmap, mitmproxy, wafw00f, Playwright, whatweb, nikto, and the SecLists corpus, among others) and adds its own engines for crawling, request interception, credential testing, adaptive scanning, and exploit chains — with results streamed live and stored per project.

At its current size, PENTRA spans **42 backend service modules** and **26 frontend workspaces**, covering reconnaissance through exploitation across web apps, APIs, GraphQL, common off-the-shelf applications, and network services.

---

## Who is it for?

PENTRA is built for people who test systems **they are authorized to test**:

- **Penetration testers & red teams** — one workspace for recon → scan → exploit → report.
- **Bug-bounty hunters** — fast recon, an intercepting proxy, a request editor, and OOB/XSS callback infrastructure without leaving the browser.
- **Security students & CTF players** — approachable UI over powerful tooling, guided attack workflows, and safe, sandboxed teaching labs.
- **Blue teams & defenders** — understand how these techniques look from the attacker's side to defend against them.

> PENTRA is strictly for **authorized security testing and education**. Every module carries an authorization notice. Using it against systems you do not own or have explicit permission to test is illegal.

---

## Core capabilities

### Reconnaissance & discovery
- **Recon** — subdomain enumeration, vhost fuzzing, spidering, directory & parameter fuzzing, DNS records & toolkit, robots.txt/WHOIS, TLS certificate inspection, technology fingerprinting, cloud asset discovery, Google dorking, and JS/PHP intelligence (endpoint & secret discovery from crawled code, with a bilingual secret-type knowledge base).
- **Interceptor** — a Playwright-driven browser session that records real traffic, login flows, and follows multi-step form chains (password-reset / registration wizards) that nothing links to directly — every discovered endpoint is tagged by how it was found.
- **WAF detection** — identifies web application firewalls, their vendors, and the real origin IP behind them.

### Scanning & detection
- **Server Scan** — an **adaptive multi-phase nmap engine** that classifies firewall/IDS/IPS resistance and escalates evasion techniques automatically. Supports **multiple targets at once** (IPs, CIDR ranges, domains), runs a fast host-discovery sweep, then deep-scans live hosts in parallel — surfacing the exact winning command for transparency.
- **Scanner** — crawler + passive checks + active checks (SQLi, XSS, command injection, path traversal, open redirect) with a per-parameter issues dashboard, a shared sqlmap console, and one-click handoff into the deeper attack tabs.
- **Vulnerability & CVE lookup** — maps detected service versions to known CVEs and exploit paths.
- **BAC (Broken Access Control)** — automated authorization-boundary testing across roles.

### Web application attacks
A 13-tab attack suite, each with an authorization gate, a "how to use" guide, and Manual Proof (raw HTTP + copy-paste curl) attached to every confirmed finding:
- **SQL Injection** — detection, an interactive SQL/MariaDB DB console, guided HTTP UNION-injection extraction, an automated login-bypass engine (grounded on vendored sqlmap error/boundary signatures, 150+ WAF-evasion payload variants), and a full sqlmap console (dump/tables/passwords, WAF bypass, advanced tuning: tamper scripts, prefix/suffix, traffic capture routed through PENTRA's own proxy).
- **XSS** — a built-in callback collector, payload generator, and a Playwright + XSStrike-powered DOM XSS engine.
- **CSRF** and **HTML Injection** — dedicated, sandboxed, localhost-only teaching labs.
- **Command Injection**, **File Inclusion (LFI)**, **File Upload** (form-field auto-discovery + real outcome classification, not just HTTP status).
- **SSRF** — expanded into SSRF / SSTI / SSI / XSLT sub-engines, each with an automatic fingerprint-and-exploit decision tree.
- **Broken Auth** — enumeration/brute-force, default credentials, raw request-chain replay, and a session-token analyzer/forger.
- **HTTP Verb Tampering**, **IDOR**, **XXE** (raw XML builder, auto file-read battery, config-secret scanning).
- **API Attacks** — full OWASP API Security Top 10 across 11 sub-tabs (BOLA, Broken Auth, BFLA, BOPLA, Resource, SSRF, Business Flow, Misconfig, Inventory, Unsafe Consumption), driven by real JWT + curl engines.

### GraphQL
A dedicated module (7 sub-tabs: Setup, Console, Info Disclosure, IDOR, Injection, Mutations, DoS) — introspection & fingerprinting, batching/DoS surface detection, SQLi through GraphQL, and automated exploit engines for each class.

### Common applications
Fingerprinting and exploitation for widely-deployed software: **WordPress, Joomla, Drupal, Tomcat, Jenkins, Splunk, PRTG, GitLab, osTicket**, plus Shellshock, ColdFusion, IIS short-name (tilde) enumeration, LDAP, and mass-assignment — each with its own enumerate → brute-force → RCE chain, plus a headless-browser environment recon (screenshot gallery + service fingerprinting across virtual hosts).

### Manual tooling
- **Brute Force** — protocol-aware credential testing across ~20 protocols via hydra/netexec/medusa and a custom HTTP engine, with automatic per-protocol tool recommendation, rate-control (delay/jitter, proxy & UA rotation, lockout auto-stop), a wordlist generator (Username Anarchy + CUPP), and SSH pivoting.
- **Proxy** — a Burp/ZAP-style intercepting proxy with history and a field inspector.
- **Repeater** — multi-tab raw HTTP request editor (Pretty/Raw/Hex/Render views), fed directly from the Interceptor.
- **cURL client** — a full-featured HTTP request builder with sessions, templates, and traffic inspection.
- **Connection / Shell / Flag Command** — live SSH & service session management, managed post-exploitation shells, and post-compromise flag hunting.

### Cross-cutting infrastructure
- **Agent hand-off** — when an automated engine comes up empty, every attack tab can generate a ready-to-paste task for an external autonomous pentesting agent.
- **Background job pattern** — every long-running operation (scans, brute-force, crawls) runs as a resumable, polling-based job that survives page navigation, with live progress in the UI.
- **Manual Proof** everywhere, per-tab usage guides, and a shared Decoder utility (base64/URL/hex/JWT + hashing).

### Reporting
- **Report** — consolidates findings across every module into a shareable, per-project report, with AI-assisted write-up support.

---

## Design

PENTRA uses a calm, professional **light "Cool Steel" design system** — a single coordinated theme with a tokenized palette, one icon library, keyboard-navigable controls, and WCAG-AA contrast. The goal is a tool that feels like a finished product, not a script with a UI bolted on.

---

## Architecture

| Layer | Technology |
|-------|-----------|
| **Backend** | Python · FastAPI · 42 async service routers, one per capability |
| **Storage** | SQLite (per-project state, scan results, history) |
| **Frontend** | React 18 · TypeScript · Vite · Zustand · a tokenized design system · 26 workspaces |
| **Delivery** | Backend serves the built single-page app — one process, one port |
| **Long-running work** | Job-queue + polling pattern — scans, brute-force, and crawls run in the background and resume across navigation |
| **Tooling wrapped** | nmap · hydra · netexec · medusa · sqlmap · mitmproxy · wafw00f · Playwright/XSStrike · headless Chromium · whatweb · nikto · SecLists |

Scale, at the time of this update: **~39,000 lines of backend Python** and **~54,000 lines of frontend TypeScript**.

The backend is organized into independent routers and services per capability, so modules can evolve without touching each other. Long-running work streams live progress to the UI instead of blocking behind a single request.

---

## Project status

- ✅ Active development.
- 🔒 **Source code is private.** This overview exists so the project can be understood without exposing the implementation.
- 🧩 Modular — new capabilities are added as self-contained modules with their own authorization gate and usage guide.

---

## Responsible use

PENTRA is a dual-use security tool intended for **authorized penetration testing, security research, and education only**. You are responsible for ensuring you have explicit permission to test any target. The authors do not condone and are not responsible for misuse.

---
---

<div dir="rtl">


</div>
