# Permission Usage

**Applies to**: BrickBookmark (a bookmark manager browser extension, hereinafter "the Extension")
**Corresponding privacy policy version**: v1.1.0 (November 27, 2026)
**Note**: This document is an appendix to the Privacy Policy, independently describing the browser permissions requested by the Extension and their purposes.

## Chromium-based Browsers (Chrome / Edge)

| Permission | Purpose |
|---|---|
| `bookmarks` | Reads and writes the user's bookmarks; core functionality of bookmark management |
| `storage` | Stores extension data locally (pinned state, tags, notes, settings, encrypted-folder metadata, etc.) |
| `sidePanel` | Provides the sidebar interface so bookmarks can be accessed without leaving the current page |
| `tabs` | Opens bookmark links clicked by the user (foreground / background) |
| `favicon` | Displays website icons in the bookmark list |
| `search` | Used only by the New Tab search box: when a keyword is neither a bookmark nor a URL, it is handed to the default search engine configured in the browser. The Extension does not receive, record, or upload that keyword |
| `identity` | Used only for third-party sign-in and re-authorization for sensitive actions: opens the provider's authorization page (GitHub / Google / X) in a browser authorization window. The Extension only receives the callback URL and hands the authorization code to the server in exchange for a sign-in state or a one-time ticket. The `client_secret` stays on the server and is never included in the extension package. **The Chrome build and the registered Edge build both show the entry and actually call this permission based on the server allowlist**; before Edge registers its extension ID and dedicated client, no entry is shown and no `identity` call is made |
| Communication with the server (brick9527.qd.je) | Enables account sign-in and registration, feedback submission, announcement fetching, update checks, and the server-side exchange for third-party sign-in / sensitive-action confirmation (the server address is configured by the developer at build time) |
| On-demand website access (optional permission) | Used only for user-initiated link checking and title refresh; as an optional permission, it is granted per bookmark domain that the user actually checks or refreshes, when the user first uses the corresponding feature. No website is accessed before the grant, and the extension never accesses websites automatically in the background. Users can view and revoke any granted site at any time in the "Site access" section of the browser extension details page; the Link Check page shows the number of authorized sites |

The Extension requests only the permissions essential for the functionality described above, and no permissions beyond its functional requirements.

> The Chromium build also replaces the default New Tab with the dashboard through the manifest key `chrome_url_overrides.newtab`. That key is a page-override declaration and grants no additional permission; everything shown on the dashboard comes from bookmark and visit data already stored on the device.

## Firefox

| Permission | Purpose |
|---|---|
| `bookmarks` | Reads and writes the user's bookmarks; core functionality of bookmark management |
| `storage` | Stores extension data locally (pinned state, tags, notes, settings, encrypted-folder metadata, etc.) |
| `tabs` | Opens bookmark links clicked by the user (foreground / background) |
| `identity` | Used for third-party sign-in and re-authorization for sensitive actions: opens the provider's authorization page (GitHub / Google / X) in a Firefox authorization window. The Extension only receives the callback URL and hands the authorization code to the server in exchange for a sign-in state or a one-time ticket; the `client_secret` stays on the server. The Firefox callback host is derived from the add-on ID (`brickbookmark@brick9527.com`) via SHA1 and is registered on the server |
| Communication with the server (brick9527.qd.je) | Enables account sign-in and registration, feedback submission, announcement fetching, and update checks (the server address is configured by the developer at build time) |
| On-demand website access (optional permission) | Used only for user-initiated link checking and title refresh; as an optional permission, it is granted per bookmark domain that the user actually checks or refreshes, when the user first uses the corresponding feature. No website is accessed before the grant, and the extension never accesses websites automatically in the background. Users can view and revoke any granted site at any time in the "Site access" section of the browser extension details page; the Link Check page shows the number of authorized sites |

The Extension requests only the permissions essential for the functionality described above, and no permissions beyond its functional requirements.

> Firefox provides the sidebar through `sidebar_action`, which needs no `sidePanel` permission; Firefox has no `favicon` permission, so site icons are rendered locally from page data.

### Firefox Data Collection Consent

Firefox provides a built-in optional data collection permission mechanism (`browser_specific_settings.gecko.data_collection_permissions.optional`, declaring `authenticationInfo` / `personallyIdentifyingInfo` / `technicalAndInteraction`). Before **sign-in, registration, third-party sign-in, re-authorization for sensitive actions, and feedback submission**, the Firefox build requests consent via `browser.permissions.request({ data_collection: [...] })`:

- If the user declines, the action stops immediately and **no network request is sent**;
- Consent covers only the account information, authentication information, and technical interaction information necessary for those actions;
- The user may revoke this optional permission at any time in the browser's extension permission settings; the next action will ask for consent again.

The Firefox build does not package the New Tab dashboard, so it **does not declare the `search` permission** and does not declare a New Tab page override. Third-party sign-in is available through the `identity` permission once the Firefox callback is registered (see above).
