# Antigravity + Gemini CLI OAuth Plugin for Opencode

[![npm version](https://img.shields.io/npm/v/@devriturajkumar/opencode-antigravity-auth.svg)](https://www.npmjs.com/package/@devriturajkumar/opencode-antigravity-auth)
[![npm downloads](https://img.shields.io/npm/dw/@devriturajkumar/opencode-antigravity-auth.svg)](https://www.npmjs.com/package/@devriturajkumar/opencode-antigravity-auth)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![X (Twitter)](https://img.shields.io/badge/X-@dopesalmon-000000?style=flat&logo=x)](https://x.com/dopesalmon)

Enable Opencode to authenticate against **Antigravity** (Google's IDE) via OAuth so you can use Antigravity rate limits and access models like `gemini-3.1-pro` and `claude-opus-4-6-thinking` with your Google credentials.

Unofficial plugin. Google may restrict accounts using third-party clients.

> **Fork release 2.1.3** — this package is `@devriturajkumar/opencode-antigravity-auth`, a fork of [NoeFabris/opencode-antigravity-auth](https://github.com/NoeFabris/opencode-antigravity-auth) 2.1.2 with two fixes awaiting upstream review: strict-JSON output for the account list RPC ([#604](https://github.com/NoeFabris/opencode-antigravity-auth/issues/604), [PR #612](https://github.com/NoeFabris/opencode-antigravity-auth/pull/612)) and request forwarding across plugin reloads ([#610](https://github.com/NoeFabris/opencode-antigravity-auth/issues/610), [PR #613](https://github.com/NoeFabris/opencode-antigravity-auth/pull/613)). Install it **instead of** the upstream package — the plugin id is unchanged, so existing accounts and credentials carry over as-is. This fork can be retired once either fix is released upstream.

## Setup

Requires OpenCode **2.1.2+** and Node.js **20+**.

Add to `~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugins": ["@devriturajkumar/opencode-antigravity-auth@2.1.3"]
}
```

Then run:

```sh
opencode auth login google
```

Choose **Google OAuth (Antigravity)**. Existing accounts are reused.

To use a local checkout, run `npm ci && npm run build` and replace the package name above with the checkout's absolute path.

## Use

```sh
opencode models
opencode run --model=google/antigravity-gemini-3.8-flash-high "Hello"
opencode run --model=google/antigravity-claude-opus-4-6-thinking#max "Hello"
```

Models are discovered from your enabled accounts and cached for an hour. Availability depends on your Google plan. Select an `antigravity-*` model for Google OAuth. Native Google API-key models are hidden while Antigravity OAuth is selected and return when you switch to an API-key connection.

Inside OpenCode, open **`/antigravity`** (or **`/ag`**) to:

- See logged-in accounts, the current selection, and cached quota percentages and reset times.
- Switch to an enabled account by selecting its row.
- Check Antigravity and Gemini CLI quotas and reset times.
- Add an account, or use **Manage account…** to verify, enable, disable, re-authenticate, or delete one.
- Refresh the model list.

The account list shows the lowest cached quota across Claude, Gemini Pro, and Gemini Flash. Choose **Check quotas / verify enabled accounts** to refresh the saved quota data. Refresh quotas if an older cache shows a percentage without a reset time.

Accounts are stored in `~/.config/opencode/antigravity-accounts.json`. Back up this file before moving machines; it contains credentials. Disabling an account excludes it from rotation and routine quota checks.

## Development

```sh
npm ci
npm run typecheck
npm test
npm run build
OPENCODE_BIN=/path/to/opencode npm run test:e2e:v2
```

The runtime test uses synthetic credentials. Authenticated model and menu checks require a logged-in Google account.

[V2 migration and verification](docs/OPENCODE-V2.md) · [Configuration](docs/CONFIGURATION.md) · [Troubleshooting](docs/TROUBLESHOOTING.md)

OpenCode 1.18.29+ remains supported through the legacy entrypoint; its account menu opens from `opencode auth login`.
