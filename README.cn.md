# ArtPlayer Web API Docs

Official documentation site for [ArtPlayer Web API](https://github.com/yangyang8002/Artplayer-Web-Api) — built with [VitePress](https://vitepress.dev), bilingual (zh/en).

English | [中文](README.cn.md)

- Live docs: <https://yangyang8002.github.io/Artplayer-Web-Api-Docs/>
- Main repo: <https://github.com/yangyang8002/Artplayer-Web-Api>

## Local Development

```bash
npm install
npm run dev      # http://localhost:5173
npm run build    # build to docs/.vitepress/dist
npm run preview  # preview production build
```

## Structure

```
docs/
├── index.md            # Home (Chinese)
├── guide/              # Guide: quickstart / player / docker / update / faq
├── admin/              # Admin: overview / console / plugins / deps / database / backup / security ...
├── api/                # API reference
├── plugins/            # Plugins: dev guide / ctx API / schema / marketplace
└── en/                 # English version
```

## Deploy

Pushing to `main` triggers a GitHub Actions workflow that builds and deploys to GitHub Pages.

## License

MIT
