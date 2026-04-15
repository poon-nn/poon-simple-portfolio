# Git Repository Management: SSH vs. Tokens

The primary difference lies in the **authentication protocol** (SSH vs. HTTPS) and how **access levels** are managed. Here is a breakdown of the two methods for managing GitHub repositories:

### 1. SSH (Secure Shell)
SSH uses a **key-pair system**: a private key (stored on your computer) and a public key (uploaded to GitHub).

*   **How it works:** When you connect via `git@github.com:...`, your computer uses the private key to sign a challenge from GitHub. If the signature matches the public key on file, you are granted access.
*   **Best for:** Local development on a trusted machine.
*   **Pros:**
    *   **No password prompts:** Once set up, you never have to enter credentials.
    *   **Secure:** Private keys never leave your machine.
    *   **Long-lived:** Keys don't expire unless you manually rotate or delete them.
*   **Cons:**
    *   **Binary Access:** Usually, an SSH key gives access to *everything* your account can do. It is harder to limit an SSH key to a single repository (unless using "Deploy Keys").
    *   **Setup:** Requires generating keys and managing the SSH agent.

### 2. Tokens (Personal Access Tokens / PATs)
Tokens are used with **HTTPS** URLs (`https://github.com/...`). They act as "scoped" passwords.

*   **How it works:** You generate a string (the token) in GitHub settings. When Git asks for a password over HTTPS, you provide the token instead of your account password.
*   **Best for:** CI/CD pipelines, scripts, or when you need restricted access.
*   **Pros:**
    *   **Fine-grained control:** You can create "Fine-grained tokens" that only have access to specific repositories or specific actions (e.g., only "Read" access to code).
    *   **Expiration:** You can set tokens to expire automatically (e.g., after 30 days).
    *   **Ease of use:** No need to manage local key files; just treat it like a password.
*   **Cons:**
    *   **Credential Management:** You often have to use a "Credential Manager" (like Keychain on macOS) to avoid typing it every time.
    *   **Security Risk:** If a token is leaked, it is plain text.

### Summary Comparison

| Feature | SSH | Tokens (HTTPS) |
| :--- | :--- | :--- |
| **Primary Use** | Daily development on your own machine. | Automation, CI/CD, or temporary access. |
| **Security** | Key-based (Hardware/File). | Password-based (String). |
| **Expiration** | None (Manually managed). | Configurable (Mandatory for new tokens). |
| **Scope** | Account-wide (usually). | Highly granular (per-repo/per-action). |
| **Convenience** | "Set it and forget it." | Requires a credential manager. |

**Recommendation:** For your **local machine**, use **SSH** for a seamless "set it and forget it" experience. For **servers, scripts, or shared environments**, use **Fine-grained Tokens** to limit the damage if the credentials are ever compromised.
