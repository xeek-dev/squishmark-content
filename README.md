# squishmark-content

Content for [squishmark.dev](https://squishmark.dev), the official SquishMark site. The site runs on [SquishMark](https://github.com/xeek-dev/squishmark) itself and renders this repository at request time.

## Structure

```
squishmark-content/
├── posts/       # Blog posts (release notes, design posts, updates)
├── pages/       # Static pages (about, docs, changelog)
├── theme/       # Custom theme templates (optional overrides)
├── static/      # Favicon and other assets
└── config.yml   # Site configuration
```

Pushing to `main` refreshes the live site via webhook; no redeploy needed.

## Writing your own SquishMark site

Don't fork this repo; use the [squishmark-starter](https://github.com/xeek-dev/squishmark-starter) template instead.

## License

MIT License, see [LICENSE](LICENSE) for details.
