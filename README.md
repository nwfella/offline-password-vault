# 🔐 Offline Password Vault + Generator

A paranoid's password manager. Core features work fully offline. Only the breach check makes an optional network call (privacy-preserving HIBP k-anonymity API).

**[🌐 Live Demo](https://nwfella.github.io/offline-password-vault/)**

## Features

| Feature | Description |
|---------|-------------|
| **Password Generation** | XKCD-style word phrases (1000-word dictionary), random alphanumeric, PINs, pronounceable passwords |
| **Strength Meter** | Real-time entropy calculation (bits) with color-coded bar — Very Weak → Very Strong |
| **Ephemeral Vault** | Store site/password combos in `sessionStorage` only — vanishes the moment you close the tab |
| **Encrypted Export** | AES-256-GCM via the Web Crypto API — export your vault as a portable encrypted blob (`PWVAULT2` format) |
| **Encrypted Import** | Decrypt and restore an exported blob with your passphrase. Backward-compatible with `PWVAULT1` |
| **Breach Check** | Privacy-preserving k-anonymity lookup against HaveIBeenPwned — only the first 5 hex chars of your password's SHA-1 hash leave your browser |
| **Copy to Clipboard** | One-click copy for any password |

## Why It's Cool

- **Web Crypto API** — demonstrates real-world AES-256-GCM, PBKDF2-SHA256 (600K iterations), and SHA-1 usage
- **Entropy math** — real-time Shannon entropy calculation with color-coded strength meter
- **Privacy-preserving API design** — the HIBP k-anonymity model in practice
- **Zero data on disk** — nothing is ever stored unless you explicitly export
- **Cryptographically sound random** — rejection-sampling CSPRNG via `crypto.getRandomValues()`, no modulo bias

## Security Model

- No servers, no backends, no cookies, no tracking
- No data ever leaves your browser except:
  - The first 5 hex chars of your password hash (HIBP breach check, optional)
  - When you explicitly copy/paste an export blob
- All random values generated via `crypto.getRandomValues()` with **rejection sampling** — eliminates modulo bias introduced by naive `% range`
- The export blob is encrypted with **AES-256-GCM** using a **PBKDF2-derived key** (600,000 iterations, SHA-256, random 16-byte salt)
- Zero external dependencies — no NPM, no CDN, no supply chain risk

## v2 Improvements

| Fix | Before | After |
|-----|--------|-------|
| **Modulo bias** | `randInt` used `% range` on 32-bit CSPRNG — slight bias for non-power-of-2 ranges | Rejection sampling: loops until value is within `0xFFFFFFFF - (0xFFFFFFFF % range)` |
| **Alphanumeric entropy** | Post-hoc last-character swap if a type was missing (reduced true entropy) | Type guarantees built into generation loop, then shuffled |
| **XSS surface** | Inline `onclick` handlers with string-interpolated password data — missing single-quote escaping | DOM API (`createElement`, `dataset`, `textContent`) + event delegation. Zero inline handlers |
| **Tagline** | "Zero-server. Zero-tracking. Zero-compromise." (contradicted by optional breach check) | "Core features work offline. Zero tracking. Zero compromise." |
| **Transparency** | No warning about browser extensions | About tab includes a red advisory card about `sessionStorage` scraping risk and memory remanence |
| **Export format** | `PWVAULT1:` format | `PWVAULT2:` format (new exports). `PWVAULT1:` imports still supported for backward compatibility |

## Tech Stack

- **Zero dependencies** — single HTML file, vanilla JS
- **Web Crypto API** — `crypto.subtle.encrypt/decrypt`, `crypto.subtle.digest`, `crypto.getRandomValues`
- **sessionStorage** — temporary vault storage
- **HIBP API v3** — k-anonymity range search (`api.pwnedpasswords.com/range/`)

## Usage

1. **Generate** — pick a password type, tweak settings, click Generate
2. **Check strength** — entropy is calculated in real-time
3. **Save to vault** — click "Save to Vault" to store site/password combos
4. **Breach check** — click the 🔍 icon on any generated password
5. **Export** — set a passphrase and export your vault as an encrypted blob
6. **Import** — paste the blob + passphrase on another device/incognito session

### Security Recommendations

- Run via `file:///` (save the HTML locally) rather than from a hosted URL
- Use a private/incognito window with **all extensions disabled** to prevent `sessionStorage` scraping
- The About tab has a full security advisory with more detail

## License

MIT
