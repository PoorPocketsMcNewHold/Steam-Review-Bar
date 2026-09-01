# Review Bar (Millennium plugin)

Replaces Steam's text review labels ("Mixed", "Very Positive", etc.) on store
pages with a simple two-color bar (blue = positive, red = negative).

Webkit-only plugin — no Lua backend needed. Runs as a preload script inside
Steam's store-page browser view, reads the review percentage Steam already
embeds in the tooltip of its own summary element, and swaps in a bar.

## Setup (assumes Millennium is already installed)

1. Clone the official template — it already has working `package.json` /
   `tsconfig.json` build tooling, don't hand-roll these:
   ```sh
   git clone https://github.com/SteamClientHomebrew/PluginTemplate reviewbar
   cd reviewbar
   ```
2. Overwrite the template's `millennium.toml` with the one in this folder.
3. Overwrite `webview/preload.ts` in the template with the one in this
   folder (same path, so it drops straight in).
4. Build:
   ```sh
   bun install
   bun run build
   ```
   `output_path = "auto"` in `millennium.toml` should detect your Millennium
   install and place the built plugin there. If it doesn't, locate your
   Millennium `plugins/` folder manually and copy the build output in.
5. Steam > Settings > Millennium > enable "Review Bar" in the plugin list.
6. Fully restart Steam, open any store page.

## If it stops matching

Steam's store-page classnames (`.user_reviews_summary_row`,
`data-tooltip-html`) have been stable for a long time but Valve can change
them without notice. If the bar doesn't appear:

1. Open a store page in Steam.
2. Use Millennium's remote-debugger / devtools access to inspect the actual
   DOM around the review score.
3. Update `ROW_SELECTOR` and the regexes in `parseScore()` in
   `webview/preload.ts` to match whatever Steam's new markup looks like.

## Customizing the look

Colors are CSS variables injected at the top of `injectStyles()` in
`webview/preload.ts` (`--reviewbar-positive`, `--reviewbar-negative`) — edit
those two hex values. Bar width/height/label format are right below.

## What this doesn't do (yet)

- Only touches the app store page rows — not library, not search results.
- Uses Steam's own blended percentage as-is; doesn't hit Steam's review API
  directly or recompute anything.

## Publishing to steambrew.app

Once it works locally: push this to its own GitHub repo, then open a PR
against [PluginDatabase](https://github.com/SteamClientHomebrew/PluginDatabase)
adding your repo as a git submodule under `plugins/`. Merged PRs get pinned
at a reviewed commit and show up in the community plugin browser.
