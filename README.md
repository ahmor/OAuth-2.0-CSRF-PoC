# OAuth Account Takeover (CSRF) Proof-of-Concept

## Description

This repository contains a professional Python implementation for demonstrating an **OAuth 2.0 Account Takeover** vulnerability. The exploit targets implementations where the `state` parameter is either missing or insufficiently validated, allowing for a **Cross-Site Request Forgery (CSRF)** attack during the authentication flow.

By automating the initial phases of the OAuth handshake, this tool generates a "poisoned" callback URL. When a victim interacts with this URL while authenticated to the target application, their account profile is silently linked to the attacker's identity provider credentials.

## Technical Mechanics

The script automates the following sequence:

1. **Identity Provider (IdP) Authentication**: Establishes an authenticated session with the OAuth provider using attacker-controlled credentials.
2. **CSRF Token Handling**: Dynamically extracts security tokens (e.g., `csrfmiddlewaretoken`, `authenticity_token`) to bypass platform-level protections.
3. **Authorization Initiation**: Triggers the OAuth request from the target application to the IdP.
4. **Callback Interception**: Captures the generated authorization `code` within the redirect URL without consuming it, keeping the token valid for the victim's session.

## Prerequisites

* **Python 3.x**
* **Requests**: `pipx install requests`
* **BeautifulSoup4**: `pipx install beautifulsoup4`

## Configuration

Before execution, update the following variables in `exploit.py`:

* `target_base`: The root URL of the vulnerable application.
* `idp_base`: The root URL of the Identity Provider.
* `user` / `password`: Valid credentials for an account on the Identity Provider.

## Usage

1. **Generate Payload**: Execute the script to retrieve the malicious callback URL.
```bash
python3 exploit.py

```

2. **Delivery**: Deliver the resulting URL (containing the `code` parameter) to the target user via a communication channel of choice.
3. **Execution**: Once the victim navigates to the link, the account linking is complete. You may then log in to the target application via the OAuth provider to access the victim's account.

## Remediation

To prevent this attack, developers must:

* Implement a unique, cryptographically secure `state` parameter for every request.
* Verify that the `state` returned in the callback matches the `state` originally sent by the application.
* Ensure that the `redirect_uri` is strictly validated against a pre-registered whitelist.

---

### Disclaimer

This software is provided for educational and authorized security testing purposes only. Unauthorized use of this tool against systems without prior written consent is strictly prohibited and may result in legal consequences. The author assumes no liability for misuse or damage caused by this program.
