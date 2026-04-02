# notion-vault

A lightweight, in-browser encryption tool designed to be embedded in Notion. Store sensitive text (passwords, keys, notes) safely — encrypted with AES-256-GCM directly in your browser, no data ever leaves your device.

## how it works

1. Enter a name, your secret (or multiple secrets in vault mode), and a passphrase → click **Generate Link**
2. Copy the generated encrypted link and paste it into a Notion `/embed` block
3. When you need it back, open the embed, enter your passphrase → click **Unlock**

## security

- **AES-256-GCM** — authenticated encryption, detects any tampering with the ciphertext
- **PBKDF2** key derivation with 310,000 iterations (SHA-256) and a random salt per encryption
- **Random IV** per encryption — the same input always produces a different ciphertext
- **Zero network requests** — all crypto runs via the browser's native [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API), no libraries, no external dependencies
- The hosted page is a single `index.html` — fully auditable in this repo

> ⚠️ This tool is designed for low-to-medium sensitivity data (e.g. secondary passwords, API keys, personal notes). For highly sensitive credentials use a dedicated password manager like [Bitwarden](https://bitwarden.com) or [1Password](https://1password.com).

## embed in Notion

1. Enable GitHub Pages for this repo (Settings → Pages → Source: main branch, / root)
2. In any Notion page, type `/embed` and paste your GitHub Pages URL:

https://<your-username>.github.io/notion-vault/

3. Resize the embed block to your liking

## run locally

No build step needed — just open `index.html` in any modern browser.

git clone https://github.com/<your-username>/notion-vault.git
cd notion-vault
open index.html

url format
Encrypted vaults are stored in the URL fragment (#) as two base64-encoded parts separated by .:

#<base64(name)>.<base64(salt + IV + ciphertext)>

The binary blob layout:

salt (16 bytes) + IV (12 bytes) + AES-GCM ciphertext (N bytes)

license
MIT



