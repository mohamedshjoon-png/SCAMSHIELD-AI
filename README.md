# SCAMSHIELD AI

**Bilingual prototype for preliminary analysis of suspicious URLs and messages.**

## 1. Problem

Phishing and online scams often use urgent language, misleading links, and requests for credentials or verification codes. People may struggle to recognize warning signs before interacting with a suspicious message or URL.

## 2. Proposed solution

SCAMSHIELD AI is a browser-based prototype that provides an initial, explainable review of:
- A URL entered directly by the user.
- A suspicious message pasted by the user.

It combines local heuristic checks with optional Google Gemini analysis. The interface supports Arabic and English.

## 3. Current MVP capabilities

- Separate URL-scanning and message-analysis modes.
- URL parsing and local checks for selected indicators such as HTTP, IP-address hosts, URL shorteners, selected risky TLDs, punycode, excessive subdomains, suspicious path/query terms, and encoded characters.
- Message checks for selected urgency, credential, payment, and verification language.
- Optional Gemini analysis when the user supplies a valid API key.
- Local-only fallback when Gemini is not configured or unavailable.
- Preliminary risk indicators and explanatory findings.
- Bilingual Arabic/English interface.

## 4. Important limitations

This is a prototype, not a production security product. The current version:
- Does not query a trusted URL/domain reputation database.
- Does not fetch or inspect the contents of a remote website.
- Does not crawl or scan search-engine results or map listings.
- Does not perform DNS, certificate, redirect-chain, or live brand-impersonation verification.
- Has not been validated through a measured detection-accuracy study.
- May produce false positives and false negatives. A low score does not prove that a URL or message is safe.

The hackathon challenge includes identifying fraudulent links and websites appearing in Search and Maps. Those integrations are not implemented in this MVP and are listed as future work rather than claimed as completed functionality.

## 5. Technical choices and trade-offs

- **Single-file HTML/CSS/JavaScript:** simple to run and easy to review during a prototype event; less suitable for long-term maintenance than a modular application.
- **Client-side heuristics:** immediate and available without an external service; limited by static rules and can be bypassed by attackers.
- **Optional Gemini API:** adds semantic analysis of text and URLs; depends on network access and a valid API key, and submitted content is sent to Google for processing.
- **Session storage for the API key:** limits persistence to the browser session, but the key remains accessible to scripts running on the page. This is not an appropriate production secret-management design.
- **No automatic URL fetching:** reduces the risk of the browser directly visiting potentially harmful links, but also means the app cannot inspect page content or redirects.

## 6. Run locally

1. Download or clone this repository.
2. Open `index.html` in a modern browser.
3. Choose **Scan URL** or **Analyze Message**.
4. Paste the URL or message and start the scan.
5. For optional Gemini analysis, open Gemini settings and enter a valid API key. Do not use a sensitive or production key.

No build step or package installation is required for this single-file prototype.

## 7. Repository structure

```text
SCAMSHIELD_AI/
├── index.html
├── README.md
└── TECHNICAL_DOCUMENTATION.md
```

## 8. Suggested next steps

1. Add a secure backend so API credentials are not exposed in the browser.
2. Integrate reputable threat-intelligence sources with clear licensing and rate-limit handling.
3. Add safe redirect, DNS, certificate, and domain-age analysis through a controlled backend.
4. Explore authorized Search and Maps data integrations that comply with platform terms.
5. Build a labeled test set of benign and malicious examples; report precision, recall, false-positive rate, and false-negative rate.
6. Add automated tests, accessibility checks, and a privacy/data-retention review.

## 9. Team and attribution

Add the registered team members and any third-party libraries or assets used before final submission. Ensure all team members have reviewed the source code and agree with the submission.
