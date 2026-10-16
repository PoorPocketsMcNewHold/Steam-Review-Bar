# Review Bar (Millennium plugin)

Replaces Steam's text review labels ("Mixed", "Very Positive", etc.) on store
pages with a simple two-color bar (blue = positive, red = negative).

Webkit-only plugin — no Lua backend needed. Runs as a preload script inside
Steam's store-page browser view, reads the review percentage Steam already
embeds in the tooltip of its own summary element, and swaps in a bar.

## Build and install

Requirements: [Millennium](https://github.com/SteamClientHomebrew/Millennium) and [Bun](https://bun.sh/).

From this repository's root:

```sh
bun install
bun run build
```

Starlight builds the plugin using the checked-in `millennium.toml`. Enable **Review Bar** in Millennium's plugin settings and restart Steam if needed.

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

## Scope and limitations

- Works on Steam Store app pages, search results, and wishlist entries where Steam exposes review-percentage metadata.
- Does not modify the Library UI or fetch review data from Steam's API; it uses information already present in the page.
- Steam can change its DOM and tooltip markup, which may require selector updates.

## AI assistance and maintenance

AI tools assisted with parts of development and troubleshooting. The maintainer is responsible for reviewing, testing, and maintaining the code; AI assistance is disclosed here for submission transparency.

## License

MIT. See [LICENSE](LICENSE).
