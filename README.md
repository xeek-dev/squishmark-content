# SquishMark Starter

A starter template for creating a blog with [SquishMark](https://github.com/xeek-dev/squishmark).

## Getting Started

### 1. Use this template

Click the "Use this template" button above to create your own content repository.

### 2. Write your content

Add posts to the `posts/` directory:

```markdown
---
title: My First Post
date: 2026-01-25
tags: [hello, world]
draft: false
---

Your post content in Markdown...
```

Add pages to the `pages/` directory:

```markdown
---
title: About Me
---

This is a static page.
```

### 3. Deploy SquishMark

Follow the [SquishMark deployment guide](https://github.com/xeek-dev/squishmark#quick-start) to deploy your blog.

## Repository Structure

```
your-content-repo/
├── posts/                    # Blog posts
│   ├── 2026-01-01-welcome.md
│   └── 2026-01-15-getting-started.md
├── pages/                    # Static pages
│   └── about.md
├── theme/                    # Custom theme (optional)
│   └── ...
└── config.yml                # Site configuration
```

## Configuration

Edit `config.yml` to customize your site:

```yaml
site:
  title: "My Blog"
  description: "A blog about things"
  author: "Your Name"
  url: "https://yourdomain.com"

theme:
  name: default
  pygments_style: monokai

posts:
  per_page: 10
```

## Frontmatter Reference

### Posts

```yaml
---
title: Post Title          # Required
date: 2026-01-25           # Required (YYYY-MM-DD)
tags: [tag1, tag2]         # Optional
draft: false               # Optional (default: false)
description: "A summary"   # Optional (for SEO/previews)
---
```

### Pages

```yaml
---
title: Page Title          # Required
description: "A summary"   # Optional
---
```

## Custom Themes

To customize the look of your blog, create a `theme/` directory with Jinja2 templates:

```
theme/
├── base.html      # Base layout
├── index.html     # Post listing
├── post.html      # Single post
├── page.html      # Static page
└── static/
    └── style.css  # Your styles
```

See the [Theming Guide](https://github.com/xeek-dev/squishmark/docs/theming.md) for details.

## License

MIT License - see [LICENSE](LICENSE) for details.
