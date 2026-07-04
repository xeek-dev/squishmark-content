---
title: Changelog
nav_order: 3
description: "Release history for SquishMark, newest first. What changed in each version, in plain English."
---

Release history, newest first. Each entry links to its full notes on GitHub. Want the story behind a release? The [blog](/) has posts on the bigger ones.

## [0.5.0](https://github.com/xeek-dev/squishmark/releases/tag/v0.5.0)

Released 2026-07-04. Pages can live in subdirectories now, so a site can group related pages into sections like `/guides/setup` instead of keeping everything flat.

- Nested pages: any folder structure under `pages/` maps to matching URLs
- Deployment housekeeping for the official site

## [0.4.0](https://github.com/xeek-dev/squishmark/releases/tag/v0.4.0)

Released 2026-07-03. Themes got their own homepages, separate from the posts listing, and the content side filled out with tag pages, an archive, and related posts.

- Tag pages, a full archive, and related posts on every post
- Theme homepages split from the posts listing, so the front page and the feed are two different things
- Terminal theme fix: the home search moved out of the hero so results no longer get clipped

## [0.3.0](https://github.com/xeek-dev/squishmark/releases/tag/v0.3.0)

Released 2026-07-03. Search landed, plus social share buttons on posts.

- Server-side search with a navbar UI on every theme
- Fuzzy matching for typo tolerance
- Social share buttons on posts
- Behind the scenes: services moved to a dependency-injection container, and a pile of theme and reload fixes (Pygments styles now follow config changes without a restart)

## [0.2.0](https://github.com/xeek-dev/squishmark/releases/tag/v0.2.0)

Released 2026-06-11. The big one: multi-theme support with two new themes (blue-tech and terminal), a redesigned default with light and dark modes, and most of the features you'd expect from a blog.

- Multi-theme support, plus the blue-tech and terminal themes and a redesigned default
- SEO essentials: Atom feed, Open Graph tags, canonical URLs, sitemap.xml, and robots.txt
- Content features: featured posts and pages, post series, per-post authors, table of contents, draft filtering
- A dynamic navbar with per-page visibility, admin notes, CSRF protection, and bot-filtered analytics
- Developer niceties: theme hot reload, dev-mode auth bypass, and release-please for automated versioning

## 0.1.0

The starting point: fetch Markdown from a GitHub repo, render it at request time. It predates the automated release notes, so there's no tag to link; it lives at the bottom of the [commit history](https://github.com/xeek-dev/squishmark/commits/main).
