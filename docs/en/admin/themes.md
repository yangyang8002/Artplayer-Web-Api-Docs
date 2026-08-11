# Theme System

## Player Themes

The player page theme is controlled by the server config `theme` (applied after saving) and can be overridden per-URL with `?theme=`.

10 built-in themes: `bilibili` (deep blue & pink), `sakura` (cherry pink & white), `ocean` (deep sea blue), `sunset` (sunset orange), `forest` (forest green), `mono` (minimal black & white), `cyber` (neon cyberpunk), `shoujo` (shojo manga), `jrpg` (JRPG), `neon` (neon samurai).

## Admin Themes

The admin theme is controlled by the `adminTheme` config, applies **instantly** and is saved locally (localStorage). Also 10 built-in themes.

## Theme Structure

Theme files live in the `theme/` directory:

```
theme/
├── admin.css            # admin base styles (generated)
├── player.css           # player base styles (generated)
├── build.js             # build script (merges theme.json + style.css)
├── admin/<theme>/       # admin themes (theme.json variables + style.css component styles)
└── player/<theme>/      # player themes (theme.json variables)
```

## Custom Themes

1. Copy `theme/admin/<theme>/` to a custom name
2. Edit the CSS variables in `theme.json` (colors, radius, etc.) and component styles in `style.css`
3. Run `node theme/build.js` to regenerate `admin.css` / `player.css`

See `public/CUSTOM_THEME.md` in the repository for details.
