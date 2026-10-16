# Review Bar

Get a quick read on a game's Steam reviews without squinting at the numbers. Review Bar turns the positive and negative review percentages into a simple bar, so you can see the split at a glance.

It shows up on Steam Store app pages, search results, and wishlist entries whenever Steam provides review percentage data. It uses information that's already on the page, so it doesn't make separate API requests. The plugin runs as a WebKit preload and doesn't need a Lua backend.

## Build and install

Requirements: [Millennium](https://github.com/SteamClientHomebrew/Millennium) and [Bun](https://bun.sh/).

From this repository's root:

```sh
bun install
bun run build
```

Starlight builds the plugin using the checked-in `millennium.toml`. Enable **Review Bar** in Millennium's plugin settings and restart Steam if needed.

## If it stops matching

Steam's store-page class names (`.user_reviews_summary_row` and `data-tooltip-html`) have been stable for a long time, but Valve can change them without notice. If the bar does not appear:

1. Open a store page in Steam.
2. Use Millennium's remote debugger or devtools access to inspect the DOM around the review score.
3. Update `ROW_SELECTOR` and the regular expressions in `parseScore()` in `webview/preload.ts` to match Steam's current markup.

## Customizing the look

The colors are CSS variables set at the top of `injectStyles()` in `webview/preload.ts`: `--reviewbar-positive` and `--reviewbar-negative`. Change their hex values to customize the colors. Bar width, height, and label format are defined nearby.

## Scope and limitations

- Works on Steam Store app pages, search results, and wishlist entries where review percentage metadata is available.
- Does not change the Library interface.
- Does not fetch review data from Steam's API. It uses information already present in the page.
- Steam may change its page structure or tooltip markup, which could require updates to the selectors.

## AI assistance and maintenance

AI tools helped with parts of development and troubleshooting. The maintainer is responsible for reviewing, testing, and maintaining the code. This disclosure is included for submission transparency.

## License

MIT. See [LICENSE](LICENSE).
