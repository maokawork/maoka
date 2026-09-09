# dsh-client-ui-aqua (optimized for dsh 0.1.2-rc.1)

> **Optimized fork** of [WYH66666666/DSH-Transparent-UI-Plugin](https://github.com/WYH66666666/DSH-Transparent-UI-Plugin) (original author: **John Wu**). This repository is published on the basis of the original author's source code, with compatibility fixes applied so the Aqua glassmorphism theme works on the current DeepSeek Harness release.

English | [中文](README.zh.md)

Aqua is a highly customizable glassmorphism theme for the DeepSeek Harness web UI. The header, sidebar, composer, stats line, and trajectory view all become panes of frosted glass. You can put a video as wallpaper, and switch it off to restore the stock UI exactly — with no source changes to DSH itself.

## Why this fork

The upstream npm package `dsh-client-ui-aqua@1.3.1` (released 2026-08-17) was built for the DSH `0.1.0-rc.x` architecture, in which the client runtime (`@deepseek-ai/dsh-client-runtime`) provided the `sessions` / `workspaces` client services. DSH `0.1.2-rc.1` (2026-09-03) refactored those services into the official controllers, and the upstream plugin's author explicitly states the plugin has not been updated to the new API yet. Installing the upstream package on `0.1.2-rc.1` therefore fails with:

- `Failed to load plugins: loader fibers failed` — `@deepseek-ai/dsh-api-session-controller` / `@deepseek-ai/dsh-api-workspace-controller` (duplicate client service registration)
- `keyed slot "settings.plugin.item" requires options.key`

## Changes vs. upstream 1.3.1

| File | Change |
| --- | --- |
| `lib/client.js` | `require("@deepseek-ai/dsh-client-runtime/client")` → `require("@deepseek-ai/dsh-client-store")` — dsh 0.1.2-rc.1 ships the store module built-in, and it exports the same `defineStore` API Aqua uses; the runtime (and its colliding service registrations) is no longer needed |
| `lib/client.js` | `settings.plugin.item` registration now passes `key: "aqua"` (the slot is `keyed` in 0.1.2-rc.1) |
| `package.json` | Removed `@deepseek-ai/dsh-client-runtime` from `dsh.client.inject` and `peerDependencies`; version bumped to `1.3.1-optimized.1` |

**Do NOT install the runtime together with this fork.** `dsh-client-runtime` registers the same client `sessions` / `workspaces` services as the 0.1.2-rc.1 controllers, which re-introduces the `loader fibers failed` conflict. No `insert` entry for `dsh-client-runtime` should exist in the profile patch.

### Known limitation

The master on/off card (Settings → Plugins → Glass theme) is not dispatched on 0.1.2-rc.1, because that keyed slot only renders cards for host-served setting namespaces and Aqua is a pure client plugin. The theme is **enabled by default**; the appearance controls are available at **Settings → General → Appearance** (the `settings.general.item` list slot renders all registrants).

## Installation (GitHub / local path)

`dsh plugin add` pulls the upstream npm package and would overwrite these fixes, so install from this repository directly:

1. Clone or download this repository.
2. Copy the `dsh-client-ui-aqua` directory into your web profile's node_modules:

   ```
   %DSH_HOME%\profiles\web\node_modules\dsh-client-ui-aqua
   ```

3. Register the plugin row in `%DSH_HOME%\profiles\web\cordis.patch.yml`:

   ```yaml
   - insert:
       - id: ui-aqua
         name: 'dsh-client-ui-aqua'
   ```

4. Restart `dsh web` (or the desktop wrapper).

The theme applies immediately. Toggle it off / tweak it in **Settings → General → Appearance**.

## License

[MIT](LICENSE) — Copyright (c) 2026 **John Wu** (original author). Modified and redistributed under the same license with the original copyright notice preserved.
