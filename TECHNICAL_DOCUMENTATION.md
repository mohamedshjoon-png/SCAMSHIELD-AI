# SCAMSHIELD AI — Technical Documentation

## 1. System overview

SCAMSHIELD AI is a bilingual, browser-based prototype for preliminary inspection of user-supplied URLs and suspicious message text. The implementation is intentionally client-side and contained in one HTML file.

### High-level flow

```text
User input
   |
   +--> URL mode --------> URL parser + local URL heuristics --+
   |                                                           |
   +--> Message mode ----> Message keyword heuristics ----------+--> Preliminary result
                                                               |
                                                   Optional Gemini request
                                                               |
                                                Weighted local/AI score
```

If Gemini is unavailable or not configured, the application retains the local result.

## 2. Main components

### User interface
- Arabic and English language toggle.
- Separate URL and message input modes.
- Scan result display with risk band and findings.
- Optional Gemini API settings modal.
- Privacy and limitation notices.

### Local URL analysis
The URL is normalized and parsed using the browser URL parser. The heuristic layer checks selected structural indicators:
- Plain HTTP rather than HTTPS.
- IP-address host.
- Embedded username/password information.
- Known URL-shortener hostnames in the local list.
- Selected high-risk top-level domains.
- Punycode host labels.
- Large numbers of subdomains.
- Long URLs.
- Credential or account-related terms in the path/query.
- Selected encoded or non-ASCII host characteristics.

These are indicators only. They are not proof that a site is malicious or safe.

### Local message analysis
The message mode checks for selected words and phrases associated with urgency, account suspension, credentials, verification codes, banking, and payments. The message rules are static and language coverage is limited to the phrases explicitly included in the code.

### Optional Gemini integration
When a user supplies a key, the browser sends the user-provided URL or message and a prompt to the Gemini `generateContent` endpoint. The application asks for a structured risk score and explanation. The result is combined with the local score using a 40% local / 60% AI weighting.

This weighting is a prototype design choice, not a statistically calibrated model. It must not be represented as validated detection accuracy.

## 3. Model choice

Gemini is used as an optional semantic analysis layer because it can interpret contextual language beyond exact keyword matches. It is not the sole decision-maker: local rules remain available as a fallback.

The model's output can be incomplete, inaccurate, or inconsistent. The prototype clamps the returned score to the supported range but does not establish ground-truth correctness.

## 4. Privacy and security design

- URL scanning does not automatically open or fetch the target website.
- Local heuristics execute in the browser.
- If Gemini is enabled, the entered URL or message is transmitted to Google for processing.
- The API key is stored in `sessionStorage` for the current browser session.
- `sessionStorage` is not secure secret storage. Page scripts can access it, so a production system should move API calls to a protected backend and keep credentials server-side.
- Users should not submit passwords, OTPs, financial details, or other sensitive personal information.
- A production release would require a documented retention policy, consent flow, secure transport, access controls, abuse monitoring, and a privacy/legal review.

## 5. Reliability and error handling

The scan flow uses a busy-state guard and a `finally` block to restore the scan button after completion. Gemini requests use an abort timeout. When the AI request fails, the local result is retained.

The API key verification flow reports request failures and clears its timeout. Browser behavior, cross-origin availability, and live Gemini connectivity still require testing in the intended deployment environment.

## 6. Evaluation status

### Verified by source inspection
- A standalone HTML prototype exists.
- The code includes URL and message modes.
- Local heuristic functions are present.
- Optional Gemini API request logic is present.
- Arabic/English UI strings and language switching are present.
- A local fallback path is present.

### Not established yet
- Successful runtime behavior in all target browsers.
- Live Gemini connectivity with a valid key.
- Detection precision, recall, or accuracy.
- Performance against a representative phishing dataset.
- Search-engine or Maps coverage.
- Production-grade security or privacy compliance.

No quantitative accuracy claim should be made until a documented evaluation has been conducted.

## 7. Known gaps against the hackathon challenge

The challenge asks for early identification of fraudulent links and websites, including those appearing in Search Engine results and Maps. The current MVP only analyzes inputs manually supplied by the user. It does not automatically ingest Search or Maps listings and does not query threat-intelligence feeds.

Recommended implementation sequence:
1. Secure backend and key management.
2. Reputation-source integration.
3. Safe URL enrichment and redirect analysis.
4. Authorized Search/Maps ingestion.
5. Dataset-based evaluation and monitoring.

## 8. Build and run

No compilation or dependency installation is needed. Open `index.html` in a modern browser. Gemini functionality requires network access and a valid API key.

## 9. Technical trade-offs

| Choice | Benefit | Limitation |
|---|---|---|
| Single HTML file | Easy to share and run | Harder to maintain and test |
| Local heuristics | Fast, offline-capable | Static and easy to evade |
| Optional Gemini | Contextual interpretation | Network/API dependency; user content is transmitted |
| Session storage | Session-scoped persistence | Not secure secret storage |
| No URL fetching | Avoids automatic visits | Cannot inspect live page or redirect behavior |

## 10. Submission integrity

This document distinguishes implemented functionality from future work. Before submission, add team names, confirm third-party asset/library licenses, test the application, and ensure the GitHub repository contains the exact submitted source.
