# 🔓 OAuth Account Takeover PoC

> **Automated Proof-of-Concept for OAuth 2.0 CSRF Vulnerabilities**

## 📖 Description
**OAuth Account Takeover** is a Python automation utility designed to demonstrate and exploit **OAuth 2.0 Cross-Site Request Forgery (CSRF)** vulnerabilities.

The vulnerability occurs when an application fails to validate the `state` parameter during the OAuth exchange. This allows an attacker to generate a valid authorization code using their own identity and trick a victim into "finishing" the link to their own profile.

### The Attack Flow:
1.  **Authenticate**: The script logs into the Identity Provider (IdP) using attacker credentials.
2.  **Authorize**: It initiates the OAuth flow to the target application.
3.  **Intercept**: It captures the `callback` URL containing the `?code=` before it is used.
4.  **Takeover**: The "poisoned" link is delivered to the victim. When clicked, the victim's account is linked to the attacker's IdP account.

---

## 🛠️ Prerequisites

* **Python 3.x**
* **Requests** (HTTP Library)
* **BeautifulSoup4** (HTML Parsing)

### Installation
```bash
git clone [https://github.com/yourusername/oauth-takeover-poc.git](https://github.com/yourusername/oauth-takeover-poc.git)
cd oauth-takeover-poc
pip install requests beautifulsoup4
```

---

## ⚙️ Configuration

Modify the `config` dictionary in `xpl.py` to match your target environment:

| Variable | Description |
| --- | --- |
| `TARGET_APP` | The URL of the application to be taken over. |
| `IDP_URL` | The URL of the OAuth Identity Provider. |
| `USERNAME` | Attacker account username on the IdP. |
| `PASSWORD` | Attacker account password on the IdP. |

---

## 🚀 Usage

1. **Generate the Payload:**
Run the script to produce the malicious callback link.
```bash
python3 xpl.py

```


2. **The Delivery:**
Send the resulting URL to the target user (the victim).
```text
[+] Payload Generated: 
[http://target-app.com/oauth/callback?code=AUTH_CODE_HERE&state=EXPLOIT](http://target-app.com/oauth/callback?code=AUTH_CODE_HERE&state=EXPLOIT)

```


3. **Profit:**
Once the victim clicks the link, go to the target application and select **"Login with [Provider]"**. You will be logged into the victim's account.

---

## 🔍 Technical Details

This script handles the following complexities:

* **CSRF Token Extraction**: Automatically scrapes `csrfmiddlewaretoken` or hidden input tokens required for login.
* **Session Persistence**: Maintains cookies across the IdP and the Target App.
* **Redirect Management**: Prevents the `code` from being consumed by the attacker session so it remains valid for the victim.

---

## ⚠️ Disclaimer

**Educational Purposes Only.** This repository is intended for security researchers and CTF participants. Unauthorized access to computer systems is illegal. The author is not responsible for any misuse of this information.
