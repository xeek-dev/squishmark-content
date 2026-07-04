---
title: Configuration
visibility: unlisted
description: How to configure a SquishMark site, from server environment variables to the config.yml in your content repo.
---

SquishMark reads configuration from two places. The server (the running engine) is configured with environment variables: where your content lives, how to talk to GitHub, who the admins are. Your content repo carries a `config.yml` at its root that sets the things a writer cares about: the site title, the theme, how many posts per page.

The split is deliberate: secrets and deployment details stay on the server, while everything about how the site looks lives next to the content, in Git, where you can review it in a pull request.

## Server environment variables

These are read once at startup. Set them in your environment, in a `.env` file, or through whatever your host uses for secrets. Anything unset falls back to the default in the table.

| Variable | Default | What it does |
|----------|---------|--------------|
| `GITHUB_CONTENT_REPO` | (empty) | The content source. Usually `owner/repo` (e.g. `sam/trailnotes-content`), or a `file://` URL pointing at a local directory, which switches the engine into local mode (see below). |
| `GITHUB_TOKEN` | (none) | Token for fetching content. Needed for private repos and to raise your API rate limit. Skip it for a public repo if you don't mind the lower unauthenticated limit. |
| `GITHUB_CLIENT_ID` | (none) | OAuth app client ID, for admin login via GitHub. |
| `GITHUB_CLIENT_SECRET` | (none) | OAuth app client secret, paired with the client ID. |
| `GITHUB_WEBHOOK_SECRET` | (none) | Shared secret for verifying incoming GitHub webhooks. Set it if you want pushes to invalidate the cache automatically. |
| `ADMIN_USERS` | (empty) | Comma-separated GitHub usernames who get admin access, for example `sam,alexj`. Anyone not on this list is anonymous. |
| `SECRET_KEY` | `change-me-in-production` | Signs session cookies. Set a long random value in production; the default is fine for local work only. |
| `DATABASE_URL` | `sqlite+aiosqlite:////data/squishmark.db` | Async SQLAlchemy connection string. The database holds things like admin notes, not your posts. Defaults to a SQLite file at `/data/squishmark.db`. |
| `CACHE_TTL_SECONDS` | `300` | How long (seconds) fetched content stays cached before the engine rechecks GitHub. Webhooks bust the cache immediately, so this is the fallback when one doesn't fire. |
| `THEMES_PATH` | (auto) | Absolute path to the themes directory. Left unset, the engine finds the bundled themes on its own (source tree in dev, `/app/themes` in Docker). Set it only for a custom location. |
| `DEBUG` | `false` | Turns on debug mode. Keep it off in production. |
| `DEV_SKIP_AUTH` | `false` | Bypasses login and treats you as an admin, so you can preview admin-only views locally without OAuth. Only takes effect when `DEBUG` is also true. Never set it in production. |

### Local content mode

Point `GITHUB_CONTENT_REPO` at a `file://` URL and the engine reads content straight off disk instead of GitHub:

```bash
GITHUB_CONTENT_REPO=file:///Users/you/my-blog-content
```

This is the fastest way to write and preview: it skips GitHub entirely, and edits show up on the next request.

## The content repo: config.yml

Every content repo has a `config.yml` at its root with three sections: `site`, `theme`, and `posts`. All three are optional, and every field has a default, so a minimal config is valid.

A complete example:

```yaml
site:
  title: "Trail Notes"
  tagline: "hiking logs and gear takes"
  logo: "/static/user/logo.svg"
  description: "Hiking logs and trail maps from the Cascades"
  author: "Sam Ashby"
  url: "https://trailnotes.example"
  favicon: "/static/user/favicon.png"
  featured_max: 5

theme:
  name: terminal
  pygments_style: github-dark
  background: "/static/user/bg.png"
  nav_image: "/static/user/nav.png"
  hero_image: "/static/user/hero.png"
  nav_max_pages: 6

posts:
  per_page: 10
```

### site

Site-wide identity and metadata.

- `title` (default `My Blog`): the site name, shown in the nav and used in page titles.
- `tagline` (default none): a short subtitle shown inline next to the title in the nav, with a separator between them.
- `logo` (default none): a logo image URL. If set, it replaces the text title in the nav.
- `description` (default empty): the site description, used for metadata and by some themes.
- `author` (default empty): the default author for the site. Individual posts can override this in their frontmatter.
- `url` (default empty): the canonical site URL, for example `https://trailnotes.example`. Used to build absolute links in the sitemap and metadata, so set it in production.
- `favicon` (default none): a custom favicon URL, for example `/static/user/favicon.png`.
- `featured_max` (default `5`): the maximum number of featured posts to show. Must be zero or more.

### theme

Which theme to use and how to tune it.

- `name` (default `default`): the theme to render with. Bundled themes are `default`, `blue-tech`, and `terminal`.
- `pygments_style` (default `github-dark`): the Pygments style for syntax-highlighted code blocks.
- `background` (default none): a background image URL, if the theme supports one.
- `nav_image` (default none): a nav image URL, if the theme supports one.
- `hero_image` (default none): a hero image URL, if the theme supports one.
- `nav_max_pages` (default none, meaning no limit): the maximum number of pages to list in the navbar.

The `theme` section accepts extra fields beyond these, so a theme can define its own settings and read them from here. Check the theme's docs for what it supports.

### posts

- `per_page` (default `10`): how many posts to show per page in the post listing.

Values you don't set fall back to their defaults, and unknown fields in `site` and `posts` are ignored, so you can start with a two-line config and grow it.
