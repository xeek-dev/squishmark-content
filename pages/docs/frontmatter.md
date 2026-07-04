---
title: Frontmatter
visibility: unlisted
description: Every frontmatter field SquishMark reads on posts and pages, what it does, and which content type it applies to.
---

Frontmatter is the YAML block at the top of a Markdown file, fenced by `---` lines. SquishMark reads it for a post's title, date, draft status, and so on. Everything is optional; a file with no frontmatter still renders with the defaults (title `Untitled`, no date, public).

Posts and pages share many fields but not all; the lists below say which apply where. A field that doesn't apply to a type is ignored there, so nothing breaks if you leave one in.

## Posts

A post is a Markdown file in the `posts/` directory. The filename sets the slug and, by convention, the date: name it `YYYY-MM-DD-slug.md`, like `2026-07-04-hello-world.md`, and it serves at `/posts/hello-world`. If you also set `date` in the frontmatter, that wins.

A realistic post:

```markdown
---
title: Three nights on the Wonderland Trail
date: 2026-07-04
tags: [trip-report, rainier]
description: Ninety-three miles, one bear, zero dry socks.
author: Sam Ashby
image: /static/user/covers/wonderland.jpg
featured: true
featured_order: 1
series: trip-reports
series_order: 4
toc: true
---

# Three nights on the Wonderland Trail

The rest of the post goes here.
```

Fields SquishMark reads on posts:

- `title` (default `Untitled`): the post title.
- `date` (default none): the publish date, `YYYY-MM-DD`. Sets ordering in listings. If omitted, the engine falls back to the date in the filename.
- `tags` (default none): a list of tags, either `[a, b]` inline or a YAML list.
- `description` (default empty): a short summary, used for the post's metadata and previews.
- `draft` (default `false`): when true, the post is hidden from listings and only visible to admins. Flip it to false to publish.
- `featured` (default `false`): marks the post as featured so themes can surface it.
- `featured_order` (default none): position among featured posts. Lower comes first.
- `template` (default none): a custom template to render this post with, instead of the theme's default post template.
- `theme` (default none): render this one post with a different theme.
- `author` (default none): overrides the site-wide author for this post.
- `image` (default none): a featured image URL, also used as the `og:image` for social previews.
- `toc` (default `true`): whether to show the auto-generated table of contents. Set `false` to turn it off. Posts only; pages never render a TOC.
- `series` (default none): a series name, to group related posts into a collection.
- `series_order` (default none): position within the series. Lower comes first.

## Pages

A page is a Markdown file in the `pages/` directory. Pages serve at the root, so `pages/about.md` is at `/about`. Subdirectories work too: `pages/docs/setup.md` serves at `/docs/setup`. Pages aren't dated the way posts are, and they don't get tags, drafts, a TOC, or series.

A realistic page:

```markdown
---
title: Setup
description: Getting a SquishMark site running.
nav_order: 2
visibility: public
---

# Setup

The rest of the page goes here.
```

Fields SquishMark reads on pages:

- `title` (default `Untitled`): the page title.
- `description` (default empty): a short summary, used for metadata.
- `template` (default none): a custom template to render this page with.
- `theme` (default none): render this one page with a different theme.
- `image` (default none): a featured image URL, also used as `og:image`.
- `featured` (default `false`) and `featured_order` (default none): rarely needed on pages, but read the same way as on posts.
- `nav_order` (default none): position in the navbar. Pages with a `nav_order` come first, in ascending order; the rest follow alphabetically by title. Pages only.
- `visibility` (default `public`): who can see the page and where it shows up. See below.

### Visibility

`visibility` takes one of three values and controls how a page is exposed. It's a page field; posts use `draft`.

- `public`: the normal case. Reachable at its URL, listed in the navbar, and included in the sitemap.
- `unlisted`: works at its URL for anyone with the link, but stays out of the navbar and sitemap. Good for pages you link to directly but don't want in the nav.
- `hidden`: returns a 404 to everyone. Use it to park a draft in the repo without publishing.
