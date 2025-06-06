# PortSwigger Labs – Web Security Academy Solutions

A curated collection of **PortSwigger Web Security Academy** lab write‑ups, PoC scripts, and helper snippets.

> **⚠️  Disclaimer**  
> For **educational and authorized testing only**. Always get permission before testing non‑lab targets.

---

## Repository structure (top‑level categories)

```
.
├── API testing/
├── Path traversial/
├── Prototype polution/
├── Race conditions/
├── Server-Side/
├── Server-side request forgery (SSRF) attacks/
└── txt-files/
```

Each category mirrors the Web Security Academy syllabus and contains sub‑folders for individual topics or labs, for example:

```
API testing/
  ├─ 1 API recon/
  ├─ 2 Mass Assignment/
  ├─ 3 Preventing vulnerabilities in APIs/
  └─ 4 Server-side parameter pollution/
```

---

## How to use

1. **Clone** the repo  
   ```bash
   git clone https://github.com/<your‑handle>/PortSwigger-labs.git
   cd PortSwigger-labs
   ```

2. **Launch** Burp Suite and log in to the  
   [Web Security Academy](https://portswigger.net/web-security).

3. **Open** the relevant lab, note the unique hostname, then browse to the matching directory here.

4. **Follow** `notes.md` or run the automation script in that lab’s folder.

---

## Prerequisites

| Tool | Purpose |
|------|---------|
| **Burp Suite** (Community or Pro) | Intercept / modify requests |
| **Python 3** + `requests` | Run PoC scripts |

---

## Contributing

* Keep the two‑level directory scheme.  
* Redact cookies/hostnames (use placeholders).  
* Explain **why** each payload works, not just the final answer.

---

## License

MIT
