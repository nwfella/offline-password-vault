# 🔐 Offline Password Vault + Generator

A paranoid's password manager that **never touches a network** — except for the privacy-preserving HaveIBeenPwned k-anonymity API for breach checks.

**[🌐 Live Demo](https://nwfella.github.io/offline-password-vault/)**

## Features

| Feature | Description |
|---------|-------------|
| **Password Generation** | XKCD-style word phrases (from 1000-word dictionary), random alphanumeric, PINs, pronounceable passwords |
| **Strength Meter** | Real-time entropy calculation (bits) with color-coded bar — Very Weak → Very Strong |
| **Ephemeral Vault** | Store site/password combos in `sessionStorage` only — vanishes the moment you close the tab |
| **Encrypted Export** | AES-256-GCM via the Web Crypto API — export your vault as a portable encrypted blob |
| **Encrypted Import** | Decrypt and restore an exported blob with your passphrase |
| **Breach Check** | Privacy-preserving k-anonymity lookup against HaveIBeenPwned — only the first 5 hex chars of your password's SHA-1 hash leave your browser |
| **Copy to Clipboard** | One-click copy for any password |

## Why It's Cool

- **Web Crypto API** — demonstrates real-world AES-GCM, PBKDF2, and SHA-1 usage
- **Entropy math** — real-time Shannon entropy calculation
- **Privacy-preserving API design** — the HIBP k-anonymity model in practice
- **Zero data on disk** — nothing is ever stored unless you explicitly export

## Tech Stack

- **Zero dependencies** — single HTML file, vanilla JS
- **Web Crypto API** — `crypto.subtle.encrypt/decrypt`, `crypto.getRandomValues`
- **sessionStorage** — temporary vault storage
- **HIBP API v3** — k-anonymity range search (`api.pwnedpasswords.com/range/`)

## Security Model

- No servers, no backends, no cookies, no tracking
- No data ever leaves your browser except:
  - The first 5 hex chars of your password hash (HIBP breach check)
  - When you explicitly copy/paste an export blob
- The export blob is encrypted with **AES-256-GCM** using a **PBKDF2-derived key** (600,000 iterations, SHA-256, random 16-byte salt)
- All randomness comes from `crypto.getRandomValues()`

## Usage

1. **Generate** — pick a password type, tweak settings, click Generate
2. **Check strength** — entropy is calculated in real-time
3. **Save to vault** — click "Save to Vault" to store site/password combos
4. **Breach check** — click the 🔍 icon on any generated password
5. **Export** — set a passphrase and export your vault as an encrypted blob
6. **Import** — paste the blob + passphrase on another device/incognito session

## License

MIT
