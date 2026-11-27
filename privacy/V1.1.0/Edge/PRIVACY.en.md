# Privacy Policy

**Applies to**: BrickBookmark (a bookmark manager browser extension, hereinafter "the Extension")
**Published date**: November 27, 2026
**Effective date**: November 27, 2026
**Current version**: v1.1.0 (previous version v1.0.0, November 11, 2026)

## Overview

The Extension is a bookmark management tool for various browsers. It **runs locally by default, and its basic features do not collect, store, or upload any personal data**.

Only when the user **actively enables the server features** will the necessary account information and feedback content be sent to a **public server operated by the developer**. Apart from the server features, third-party sign-in, and user-initiated network requests, the Extension performs no other network communication and contains no advertising or tracking components.

The following data handling behaviours are added or changed in this version (v1.1.0) and are disclosed item by item in this policy:

- **Third-party sign-in and account binding** (GitHub / Google / X): the browser opens the identity provider's authorization page, and the Extension only hands the authorization code to the server in exchange for a sign-in state. The `client_secret` is kept only on the server and never shipped in the extension package. Each platform uses its own registered OAuth client and callback URL, and whether the entry is shown depends on the allowlist returned by `GET /auth/oauth/capabilities`. **The Edge build shows no entry until its extension ID and dedicated OAuth client are registered**; once registered, the same mechanism applies and this policy will be updated accordingly.
- **Re-confirmation for sensitive actions**: accounts created through third-party sign-in have no password. Before updating their profile, deleting server data, or deregistering, they must re-authorize once with a bound provider to obtain a **one-time ticket** (valid for 300 seconds by default). The ticket exists only in page memory; it is neither written to local storage nor uploaded. Password accounts confirm with their password and do not trigger this flow.
- **Encrypted folders (local encryption)**: users may migrate folders into a local encrypted container protected with AES-256-GCM and keys derived via PBKDF2-SHA256 (100,000 iterations). The password and recovery code are used in memory only while entered; **their plaintext is never stored or uploaded**, and ciphertext stays on the local device.
- **New Tab dashboard**: **runs entirely locally**, reading only the bookmark data and visit statistics already stored on the device and uploading nothing. When a keyword in its search box matches neither a bookmark nor a URL, the keyword is handed to the **default search engine configured in the user's browser**; the Extension does not receive, record, or upload those keywords. This feature ships with the Chromium builds (Chrome / Edge).
- **"Check for updates" in Settings**: calls the public server endpoint `GET /version` to read version and release notes only; it sends no auth token and no local data.

"Personal data" in this policy means any information that identifies, or could be used to identify, a person. All data handling practices of the Extension are fully disclosed in this policy; there is no undisclosed data processing.

This policy primarily refers to the Microsoft Edge browser. The Extension is also compatible with other Chromium-based browsers, and the same policy applies in principle. The Edge build shows no third-party sign-in entry until its extension ID and dedicated OAuth client are registered (the `identity` permission is invoked only when the entry is shown), and all other data handling behaviour is identical to the Chrome build.

## 1. Data We Process

### 1.1 Local Data (stored only on the user's device; never leaves the browser)

| Data Category | Content | Storage | Purpose |
|---|---|---|---|
| Bookmark data | Bookmarks, folders, and the bookmark tree | Browser Bookmarks API | Core data of the Extension; used solely for display and management |
| Pinned state | Set of pinned bookmark IDs | `chrome.storage.local` | Renders the pinned section |
| Tag data | Tag definitions and bookmark–tag associations | `chrome.storage.local` | Tag management and filtering |
| Notes | User-added notes attached to bookmarks | `chrome.storage.local` | Note display |
| Visit statistics | Visit counts and last-visited timestamps | `chrome.storage.local` | Sorting and retrieval; the New Tab favorites grid and recent list |
| Recycle bin data | Metadata of deleted bookmarks | `chrome.storage.local` | Undo of accidental deletion; scheduled emptying |
| Settings preferences | Theme, font size, language, default sort, New Tab recent-list toggle, hidden items, etc. | `chrome.storage.local` | Personalized interface |
| Auth state | Server access token (only after enabling server features) | `chrome.storage.local` | Keeps the user signed in; removed on sign-out; replaced by the new token issued after a first-time password is set |
| Encrypted folder metadata | Container ID, key-derivation parameters (PBKDF2-SHA256, 100,000 iterations), salt, nonce, recovery-code checksum, item ID mapping | `chrome.storage.local` (prefix `bm_vault_*`) | Local unlock, password change, recovery-code rotation, and container management; **contains no plaintext password or recovery code** |
| Encrypted folder items | Bookmark content and related data encrypted with AES-256-GCM | Browser IndexedDB | Decrypted in memory and displayed after unlock; ciphertext never leaves the device |
| Backup file (user-initiated export) | `.bbmk` snapshot: bookmark tree, tags, notes, folder colours, pinned items, visit statistics, recycle-bin metadata, encrypted container ciphertext and key-derivation parameters | A location chosen by the user on the device | Kept and migrated by the user; exporting **requires no unlock** and the file contains no plaintext password or recovery code |

All of the above data is stored locally in the user's browser. The Extension developer **cannot access it**, and it is never uploaded to any server. The greeting, favorites grid, and recent list shown on the New Tab are all rendered from the **local data above**; the New Tab has no separate data source and does not read browser history.

The password and recovery code of an encrypted folder are used only to derive keys on the local device: their plaintext is never written to extension storage, never written to IndexedDB, never included in an exported `.bbmk` file, and never uploaded. Decrypted keys exist only in page memory and are released when the page closes or the folder is locked manually.

### 1.2 Server Feature Data (requires user opt-in)

| Data Category | Content | Destination | Purpose |
|---|---|---|---|
| Account information | Username, password (encrypted in transit), auth token, email (provided at registration) | Server operated by the developer | Registration / sign-in / authentication |
| Third-party identity data | Provider (GitHub / Google / X), provider user ID, email supplied by the provider (only when verified), display name, binding time | Server operated by the developer (obtained from the provider by the server) | Third-party sign-in, account binding and unbinding; the unique key is "provider + provider user ID", and accounts are **never merged automatically by email** |
| Sensitive-action confirmation | Action purpose (`update` / `deleteData` / `delete`), redirect URI, provider authorization code and `state` | Server operated by the developer | Verifies identity and issues a one-time ticket; the ticket is not a sign-in credential and no auth token is issued |
| Feedback content | Feedback type, text content, submission time | Server operated by the developer | Receiving user opinions and bug reports |

> Note: please do not include sensitive personal information (such as ID card numbers, bank account numbers, or passwords) in feedback content. Feedback attachments are not supported in this version.

> Note: the "third-party identity data" and "sensitive-action confirmation" rows above apply once the Edge extension ID and OAuth client are registered and the entry is shown; before registration the Edge build generates neither category of data. The Chrome build and the registered Firefox build also fall within these rows.

Additional notes on third-party sign-in:

- The Extension requests only the minimum scopes required for sign-in: GitHub `read:user user:email`, Google `openid email profile`, and X `users.read`.
- The authorization page is handled directly between the browser and the provider. The Extension does not read repositories, friends, timelines, direct messages, or any other provider data, and does not collect avatars.
- X's `users.read` scope does not return an email address, so a previously unbound X identity cannot create an account on its own; the user must register with an email first and then bind X in Settings.
- Third-party sign-in entry points are shown based on the platform callback allowlist. The Edge build shows no entry and initiates no third-party authorization request until its extension ID and dedicated OAuth client are registered; once registered, the entry opens the authorization window through the `identity` permission.

### 1.3 What We Explicitly Do NOT Collect

- Browsing history or the content of web pages the user visits
- Personal identity information such as name, phone number, or address (except for the account and email information described in 1.2)
- Location data or search history (keywords typed into the New Tab search box are used only for that single jump; they are not stored or uploaded)
- Friends, repositories, timelines, direct messages, avatars, or any other provider data unrelated to sign-in
- **Highly sensitive information such as health, financial, or payment data** (such data is unrelated to the Extension's functionality and is never collected under any circumstances)
- Any advertising identifiers or behavioral profiling data
- **Personal information of non-users** (the Extension does not collect or share any third party's information)
- Plaintext passwords, recovery codes, or derived keys of encrypted folders (used only in local memory)
- Sensitive-action tickets (held only in page memory and cleared after use)
- IP addresses: after the server features are enabled, server logs may **briefly record** IP addresses **solely for security purposes** (e.g., detecting abnormal logins); such logs are deleted within 30 days. No such records are created when the server features are not in use.

## 2. How Data Is Used

- **Local data**: used only to render the bookmark management interface and implement management features on the user's device; not used for any other purpose.
- **Server data**: used only to complete user-initiated authentication, third-party sign-in and account binding, identity confirmation for sensitive actions, and feedback submission; not used for analytics, marketing, or profiling.
- **Third-party identity data**: used only to confirm the provider identity, create or link an account, and maintain the binding relationship; not used for any other purpose.
- **One-time tickets**: used only to confirm identity for the current sensitive action and invalidated immediately after verification (consumed once on the server).
- **Network requests**: the following requests may involve external communication:
  - Link checking / title refresh: requests are sent to user-specified target websites to detect availability or retrieve titles;
  - Server features: authentication, account-management, or feedback requests are sent to the server operated by the developer;
  - Announcement fetching: the Extension automatically fetches product announcements from the server (no personal data) when the manager page loads;
  - Update check: triggered by the user in Settings to read public version information from the server (no personal data, no auth token);
  - Third-party sign-in / re-authorization for sensitive actions: the browser opens the provider's authorization page with only the minimum scope and the callback URL;
  - Default search engine hand-off: when a keyword in the New Tab search box matches neither a bookmark nor a URL, it is passed to the default search engine configured in the user's browser through the browser's search API. The request is made by the browser and governed by that search engine's privacy policy; the Extension does not receive, store, or upload the keyword.
- Apart from anonymizing or deleting feedback content within the periods in Section 3, the Extension does not perform secondary processing such as **aggregation or profiling**. Should future features require such processing, this policy and applicable laws will be followed, and this policy will be updated accordingly.

## 3. Data Storage and Transmission

- **Local storage**: uses browser extension storage (`chrome.storage.local`) and browser IndexedDB; data exists only on the user's device and is fully deleted when the extension is uninstalled (including encrypted containers).
- **Transmission**: no local bookmark, tag, note, visit-statistics, or encrypted-container data is transmitted without permission; network transmission uses **modern, secure encryption methods such as HTTPS**, and no insecure channels are used to transmit personal data.
- **Retention periods**:
  - Local data persists until the user clears it or uninstalls the extension;
  - **Account data**: after a deregistration request the account is disabled immediately, and account details, third-party binding identifiers, and auth-token records are deleted **within 30 days** of deregistration;
  - **Feedback content**: anonymized or deleted **within 180 days** of creation; retained statistics contain no identifiable fields;
  - **Security logs (including IP records)**: deleted **within 30 days** of being written;
  - **One-time tickets**: expire after **300 seconds** by default and are invalidated after a single use;
  - **Auth tokens**: deleted on sign-out, account deregistration, or a 401 authentication failure; after a first-time password is set, the new token issued by the server replaces the old one.
- **Server location and cross-border transfer**: the server is operated by the developer and deployed in **Hong Kong, China**. Enabling the server features involves cross-border data transfer. Data transferred across borders is limited to account information, feedback content, and security logs (see 1.2 and 1.3), and all transfers are encrypted with HTTPS. The developer applies reasonable security safeguards (such as encrypted storage and least-privilege access controls) to protect such data. **Enabling the server features is deemed as the user's consent to the above transfer and processing.**

## 4. Third-Party Disclosure

Apart from the cloud hosting described below, the Extension **does not share, sell, rent, or disclose user data to any third party** and does not engage in any form of data brokering. It contains no advertising SDKs, analytics SDKs, or third-party tracking code.

### 4.1 Third-Party Identity Providers (GitHub / Google / X)

The Edge build shows no third-party sign-in entry until its extension ID and dedicated OAuth client are registered, so it does not interact with GitHub / Google / X for authorization; once registered and shown, when the user actively clicks third-party sign-in or "re-authorize for verification", the browser opens the corresponding provider's authorization page, where the user confirms the requested scope. The Extension sends no local data — bookmarks, tags, notes, visit statistics, encrypted containers, etc. — to the provider. The server uses only the minimum identity information returned by the provider (provider user ID, verified email, display name) to complete sign-in or binding. The provider's handling of the authorization process and account data is governed by its own privacy policy.

### 4.2 Cloud Service Provider (Data Processor)

The server is hosted as a cloud server. The cloud service provider acts solely as a data processor providing infrastructure services under the developer's instructions; it does not share, sell, or use user data, and its processing is governed by this policy.

Data may be transferred in only the following situations, none of which constitutes "sharing" with a third-party vendor:

1. Interaction between the user and the server operated by the developer (account and feedback data; enabled by the user's opt-in);
2. User-initiated link-check requests to target websites (only the link availability status and title are returned).

## 5. Data Security

- Local data is kept in the browser's isolated extension storage and IndexedDB, readable and writable only by the Extension;
- The auth token is stored locally and automatically cleared on sign-out, account deregistration, or a 401 authentication failure; after a first-time password is set, the old token is revoked by the server;
- Encrypted folders use AES-256-GCM for items and PBKDF2-SHA256 (100,000 iterations) for key derivation, with the password and the recovery code each wrapping the same random data key; plaintext password and recovery code are neither stored nor uploaded, and 3 consecutive failed unlock attempts trigger a 60-second cooldown;
- Sensitive-action tickets are bound to "account + session + purpose", consumed once, expire after 300 seconds by default, and exist only in page memory — never in local storage (the Edge build has password-only accounts and therefore no tickets);
- Server data is protected with encrypted storage and least-privilege access controls, and all transmission is encrypted with HTTPS;
- The `client_secret` for third-party sign-in is kept only in server environment variables; it is never included in the extension package or written to client storage;
- The Extension **will not publicly disclose** any user's authentication information; in the event of a security incident involving authentication information, affected users will be promptly notified.

## 6. User Rights and Controls

> Before OAuth client registration, all Edge accounts are password accounts; after registration the Edge build supports both password and third-party-only accounts, and both paths in the table below apply.

| Action | How | Effect |
|---|---|---|
| Access and review data | Local data can be viewed directly in the Extension's interface (bookmarks, tags, notes, settings, etc.); server data can be accessed on the server side through the user's account | Users can always know what data the Extension processes |
| Correction and copying | Modify account information in the settings page; requests to copy or correct data can be made through the contact channels and will be responded to **within 7 working days** | Protects the rights to correct and copy personal information |
| Withdraw consent | Disable the server features and sign out in the settings page, which stops further transmission and processing; existing server-side data can be deleted via "Delete my server data" | Transmission and processing stop immediately upon withdrawal |
| Delete my server data | "Delete my server data" in the settings page: password accounts confirm with their password; third-party-only accounts confirm by re-authorizing a bound provider to obtain a ticket | Deletes the feedback content and attachments stored on the server; the account profile, sign-in state and third-party bindings are retained (bindings can be removed one by one under Account & Sign-in) |
| Update account information | Modify username / email / password (at least one) in the settings page: password accounts enter the current password; third-party-only accounts use a one-time ticket; submitting "ticket + new password" sets a first password | Takes effect immediately; after setting a first password all old auth tokens are invalidated and the new token must be used |
| Bind / unbind third-party accounts | "Account and sign-in" in the settings page lists bound providers and supports binding or unbinding each one (unbinding requires a second confirmation and at least one sign-in method must remain) | The binding is updated immediately; an unbound provider can no longer be used for sign-in or sensitive-action confirmation |
| Deregister account | Deregistration entry is provided on the sign-in page; password accounts enter their password, third-party-only accounts use a one-time ticket | The account is disabled immediately, and account details and third-party binding identifiers are deleted **within 30 days** |
| Clear all local data | Uninstall the extension | All local data (settings, tags, notes, visit statistics, encrypted containers, auth state, etc.) is deleted |
| Reset configuration | "Reset configuration" in the settings page | Restores default settings |
| Clear visit statistics | "Clear visit counts" in the settings page | Deletes visit frequency data |
| Empty the recycle bin | "Empty recycle bin" in the settings page | Permanently deletes bookmarks in the recycle bin |
| Hide the New Tab recent list | "Show recent visits" toggle in the settings page | The New Tab stops showing the recent list (display only; no data is deleted) |
| Sign out | "Sign out" in the settings page | Removes the local auth token |

## 7. Policy Changes

This policy will be **kept up-to-date** as the Extension evolves (revised when new features are added). When changes are made, this document, the version number, and the effective date will be updated accordingly, and the changes will be noted in the extension's changelog. Material changes to the purpose, scope, or manner of data processing will be highlighted within the Extension.

## 8. Protection of Minors

The Extension is a productivity tool and **contains no adult content**. Because the server features process account information, **users under the age of 14 should not use the account sign-in or feedback features**. If a guardian discovers that a minor's information has been processed, they may request deletion through the contact channels below, and we will respond within 7 working days.

## 9. Contact Us

If you have any questions about this Privacy Policy or data processing, or need to exercise personal data rights under applicable law, please contact us through the following channels, and we will respond in a timely manner in accordance with applicable laws:

- Email: **brick9527@foxmail.com**
- Open-source repository: [brick9527/brick_bookmark_profile](https://github.com/brick9527/brick_bookmark_profile/issues)
