# Permission Usage

**Applies to**: BrickBookmark (a bookmark manager browser extension, hereinafter "the Extension")
**Corresponding privacy policy version**: v0.14.1.3 (October 15, 2026)
**Note**: This document is an appendix to the Privacy Policy, independently describing the browser permissions requested by the Extension and their purposes.

## Chromium-based Browsers (Chrome / Edge)

| Permission | Purpose |
|---|---|
| `bookmarks` | Reads and writes the user's bookmarks; core functionality of bookmark management |
| `storage` | Stores extension data locally (pinned state, tags, notes, settings, etc.) |
| `sidePanel` | Provides the sidebar interface so bookmarks can be accessed without leaving the current page |
| `tabs` | Opens bookmark links clicked by the user (foreground / background) |
| `favicon` | Displays website icons in the bookmark list |
| Communication with the server (brick9527.qd.je) | Enables online features such as account login and feedback submission (the server address is configured by the developer at build time) |
| On-demand website access (optional permission) | Used only for user-initiated link checking and title refresh; as an optional permission, it is granted per bookmark domain that the user actually checks or refreshes, when the user first uses the corresponding feature. No website is accessed before the grant, and the extension never accesses websites automatically in the background. Users can view and revoke any granted site at any time in the "Site access" section of the browser extension details page; the Link Check page shows the number of authorized sites |

The Extension requests only the permissions essential for the functionality described above, and no permissions beyond its functional requirements.

## Firefox

| Permission | Purpose |
|---|---|
| `bookmarks` | Reads and writes the user's bookmarks; core functionality of bookmark management |
| `storage` | Stores extension data locally (pinned state, tags, notes, settings, etc.) |
| `sidePanel` / Sidebar | Provides the sidebar interface so bookmarks can be accessed without leaving the current page |
| `tabs` | Opens bookmark links clicked by the user (foreground / background) |
| `favicon` | Displays website icons in the bookmark list |
| Communication with the server (brick9527.qd.je) | Enables online features such as account login and feedback submission (the server address is configured by the developer at build time) |
| On-demand website access (optional permission) | Used only for user-initiated link checking and title refresh; as an optional permission, it is granted per bookmark domain that the user actually checks or refreshes, when the user first uses the corresponding feature. No website is accessed before the grant, and the extension never accesses websites automatically in the background. Users can view and revoke any granted site at any time in the "Site access" section of the browser extension details page; the Link Check page shows the number of authorized sites |

The Extension requests only the permissions essential for the functionality described above, and no permissions beyond its functional requirements. In Firefox, the Extension follows the Firefox built-in data collection consent mechanism and transmits optional data (account, feedback, and technical data) only after obtaining the user's consent.
