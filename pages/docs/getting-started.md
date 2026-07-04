---
title: Getting started
visibility: unlisted
toc: true
description: Go from an empty content repo to a live SquishMark blog, then wire up the webhook so pushes go live in seconds.
---

<div class="notice"><strong>Heads up:</strong> this guide is brand new and still being tested end to end. Steps may shift, and a rough edge or two is likely. If something doesn't match what you see, check back soon.</div>

This walks you from nothing to a running blog: create a content repo, write a post, deploy on Fly.io, and set up a webhook so pushes go live without a redeploy. You don't need to be a developer, and every command you'll run is written out for you to copy and paste.

## Before you start

You need three things, all free to start:

- **A GitHub account.** Sign up at [github.com](https://github.com) if you don't have one. GitHub is where your writing lives.
- **A repo.** A repo (short for repository) is just a folder of files that GitHub stores and keeps a history of. You'll make one for your posts in the next step.
- **A terminal.** The deploy step means pasting a few commands into a terminal window: the Terminal app on a Mac, or PowerShell on Windows. Where commands differ between the two, both versions are given.

## Create your content repo

Your posts and pages live in a separate GitHub repo, not the engine, and SquishMark fetches them at request time. The fastest start is the [squishmark-starter](https://github.com/xeek-dev/squishmark-starter) template: open it on GitHub, click "Use this template", and make a new repo under your account. Call it whatever you like; `my-blog-content` works fine.

The starter comes with a `config.yml`, a sample post, and the directory layout SquishMark expects:

```
my-blog-content/
├── posts/       # your blog posts
├── pages/       # standalone pages like About or Docs
├── static/      # images, favicon, anything served as-is
└── config.yml   # site title, theme, and other settings
```

## Write your first post

Posts are Markdown files in the `posts/` directory. Name them `YYYY-MM-DD-slug.md`. The date prefix sets the post's date and the rest becomes the URL slug, so `2026-01-25-hello-world.md` publishes at `/posts/hello-world` dated January 25th.

Each post starts with a frontmatter block (the part between the `---` lines) followed by your content:

~~~markdown
---
title: Hello World
tags: [hello, world]
---

# Hello World

This is my first post, written in **Markdown**.

```python
print("Hello from SquishMark!")
```
~~~

You can skip the date prefix and set `date:` in the frontmatter instead, but the filename prefix is the simplest way to keep posts sorted.

## Deploy on Fly.io

Fly.io runs your blog and bills by usage; a personal blog on the smallest VM costs a few dollars a month at most.

Start by installing the Fly CLI (the `fly` command). Pick your system:

**macOS** with Homebrew:

```bash
brew install flyctl
```

No Homebrew? Use the curl installer instead:

```bash
curl -L https://fly.io/install.sh | sh
```

**Windows** in PowerShell:

```powershell
pwsh -Command "iwr https://fly.io/install.ps1 -useb | iex"
```

The [Fly install docs](https://fly.io/docs/flyctl/install/) cover both if either one gives you trouble.

Now clone the engine repo (it ships with a `fly.toml` config file) and launch. This uses `git`; if your system says it isn't installed, grab it from [git-scm.com](https://git-scm.com/downloads):

```bash
git clone https://github.com/xeek-dev/squishmark.git
cd squishmark
fly launch
```

`fly launch` walks you through setup and asks a couple of questions, like a name for your app and which region to run it in. Accepting the defaults is fine; you can change them later. It reads the included `fly.toml` and may adjust or add settings as it goes. If you're not signed in yet, it'll prompt you to create a Fly account or log in first.

Where does the actual app come from? You don't build it by hand. The engine's code lives in the repo you just cloned, next to a `Dockerfile` (the recipe for packaging it as a container image). When you deploy, Fly builds that container on its own servers and runs it, so you never need Docker installed for this path.

SquishMark keeps analytics and admin data in a SQLite database, so create a volume for it. The bundled `fly.toml` mounts a volume named `squishmark_data` at `/data`:

```bash
fly volumes create squishmark_data --size 1
```

Now set your secrets. `GITHUB_CONTENT_REPO` tells the engine which repo to read (same command on any system):

```bash
fly secrets set GITHUB_CONTENT_REPO=your-username/my-blog-content
```

`SECRET_KEY` signs sessions, so it needs to be a long random string. On macOS or Linux, `openssl` generates one:

```bash
fly secrets set SECRET_KEY=$(openssl rand -hex 32)
```

Windows PowerShell doesn't have `openssl`, so generate the random value with PowerShell instead:

```powershell
fly secrets set SECRET_KEY=$(-join (1..32 | ForEach-Object { '{0:x2}' -f (Get-Random -Maximum 256) }))
```

`GITHUB_TOKEN` is optional for a public repo but worth adding: without it, GitHub caps unauthenticated requests at a low rate, and a token lifts that ceiling (it's required for a private repo):

```bash
fly secrets set GITHUB_TOKEN=ghp_your_token_here
```

Then deploy:

```bash
fly deploy
```

## Set up the push webhook

Out of the box, SquishMark caches content and refreshes on a timer (five minutes by default). A webhook makes pushes go live in seconds instead: when GitHub sends one, the engine clears its cache and re-fetches your content, no redeploy needed.

First pick a secret and set it on the app. On macOS or Linux:

```bash
fly secrets set GITHUB_WEBHOOK_SECRET=$(openssl rand -hex 32)
```

On Windows PowerShell:

```powershell
fly secrets set GITHUB_WEBHOOK_SECRET=$(-join (1..32 | ForEach-Object { '{0:x2}' -f (Get-Random -Maximum 256) }))
```

Then add the webhook in your content repo on GitHub, under Settings, Webhooks, Add webhook:

- Payload URL: `https://your-app.fly.dev/webhooks/github`
- Content type: `application/json`
- Secret: the same value you set for `GITHUB_WEBHOOK_SECRET`
- Events: just the push event

SquishMark verifies the signature against your secret and only acts on push events, so a bad or unsigned request gets rejected. Push a new post and it shows up a moment later.

## Optional: preview locally with Docker

Comfortable with Docker? You can run the engine against your repo before (or instead of) deploying. From the engine repo you cloned in the deploy step, build the image once, then point it at your content repo:

```bash
docker build -t squishmark .
docker run -p 8000:8000 \
  -e GITHUB_CONTENT_REPO=your-username/my-blog-content \
  squishmark
```

Open http://localhost:8000 and you should see your post. If your content repo is private, add a GitHub token so the engine can read it:

```bash
docker run -p 8000:8000 \
  -e GITHUB_CONTENT_REPO=your-username/my-blog-content \
  -e GITHUB_TOKEN=ghp_your_token_here \
  squishmark
```

You've got a live blog that updates every time you push. From here, [Configuration](/docs/configuration) tunes `config.yml`, [Theming](/docs/theming) changes how it looks, and [Frontmatter](/docs/frontmatter) lists every per-post field.
