# Notion Vault 🛡️

**Private by Design. Your secrets never leave your screen.**

Notion Vault is a "Zero-Knowledge" security widget. Unlike traditional "cloud" apps, this tool has no database, no login, and no backend. It turns your browser into a local encryption station.



---

## 🔒 The "No-Access" Guarantee

This project is built on the principle that **trust is unnecessary when you have math.**

* **I cannot see your data:** As the developer, I have zero access to your secrets. The application is hosted as a static site; there is no server receiving your input.
* **GitHub cannot see your data:** While GitHub hosts the code, the encryption happens entirely within your browser's temporary memory.
* **Notion cannot see your data:** Notion only sees a URL. Because your secret is stored in the **URL Fragment** (everything after the `#`), it is never sent over the network.

### Why the `#` matters
In web technology, the part of a URL following a `#` is called the "fragment." **Web browsers are programmed to never send this fragment to any server.** 1.  When you load the vault, your browser asks GitHub for the `index.html` file.
2.  The browser **withholds** the part of the link containing your password. 
3.  The encryption/decryption happens locally on your CPU. 

**Result:** Your secret data exists only in two places: **Inside the encrypted link on your Notion page** and **Inside your computer's RAM** for the few seconds it is unlocked.

---

## 🚀 How it Works

1.  **Generate:** Enter a name, your secret (or multiple secrets in vault mode), and a Master Passphrase → click **Generate Link**.
2.  **Embed:** Copy the generated encrypted link and paste it into a Notion `/embed` block.
3.  **Unlock:** When you need it back, open the embed, enter your passphrase → click **Unlock**.

---

## 🛡️ Security Details

We use industry-standard "Military Grade" primitives provided by the native **Web Crypto API**:

* **AES-256-GCM:** Authenticated encryption. This ensures that if even a single bit of your encrypted data is tampered with, the vault will detect it and refuse to open.
* **PBKDF2 Key Derivation:** Your passphrase isn't just a "key." We scramble it with a random salt **$310,000$** times using the SHA-256 algorithm. This makes it virtually impossible for a computer to "guess" your password.
* **Random IV per Encryption:** Every time you click "Generate," we use a new, random Initialization Vector. Even if you encrypt the same word twice, you will get two completely different links.
* **Zero Network Requests:** All crypto runs via the browser's native API. No libraries, no external dependencies, no trackers.



---

## 📝 Usage & Notion Setup

### 1. Host it on GitHub Pages
1.  Go to your repo **Settings** → **Pages**.
2.  Set **Source** to `Deploy from a branch`.
3.  Select the `main` branch and the `/ (root)` folder. Click **Save**.
4.  Your vault is now live at `https://<your-username>.github.io/notion-vault/`.

### 2. Embed in Notion
1.  In any Notion page, type `/embed`.
2.  Paste your GitHub Pages URL (or the specific encrypted link generated).
3.  Resize the block to your liking.

---

## 🛠️ Technical Details

### URL Format
Encrypted vaults are stored in the URL fragment (`#`) as two base64-encoded parts separated by a `.`:
`#<base64(name)>.<base64(salt + IV + ciphertext)>`

### Binary Blob Layout
The binary blob layout within the second part is:
`salt (16 bytes) + IV (12 bytes) + AES-GCM ciphertext (N bytes)`

### Run Locally
No build step needed — just open `index.html` in any modern browser.
```bash
git clone [https://github.com/jonathantiedchen/notion-vault.git](https://github.com/jonathantiedchen/notion-vault.git)
cd notion-vault
open index.html
