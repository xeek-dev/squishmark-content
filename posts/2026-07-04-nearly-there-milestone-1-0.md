---
title: "Nearly there, Milestone 1.0"
date: 2026-07-04
tags: [release]
description: "Why I built a Markdown blog engine in Python, what it can do now, and the short list left before I call it 1.0."
featured: true
featured_order: 2
---

I didn't set out to build a blog engine. I set out to have a website: a home for my projects as they evolve, and a place to write about tech when the mood hits. What I wanted from it was simple. Write Markdown, push it, see it online.

Finding that turned out to be the hard part. The hosted builders fought me on the one thing I cared most about: code. I could not get syntax highlighting working correctly out of the box in anything from WordPress to Squarespace, and for someone who writes about software, a blog that mangles code blocks is a dealbreaker. The static site generators handle code fine, but they bring build pipelines and toolchains I didn't want to babysit. And I wanted Python underneath: not because it's the only thing I write, but because it's the language I actually enjoy maintaining. Nothing out there was simple enough to love. So, SquishMark.

## What it is now

Milestone 1.0 means fully functional, and it's essentially there:

- Your posts live as Markdown in a GitHub repo; pushing publishes them
- Syntax highlighting that just works, server-side, over 500 languages, pick your style in one config line
- Three themes, light and dark, and you can reskin or override any of them from your own repo
- Search with typo tolerance, tags, an archive, drafts, series, featured posts
- The boring essentials done right: feeds, sitemaps, Open Graph tags, reading time

## Proving it

Before calling anything 1.0, I put this site on it. squishmark.dev is a SquishMark site; the post you're reading is a Markdown file, fetched and rendered when you loaded the page.

Running it for real found problems no test suite did. The docs needed folders, and it turned out pages couldn't live in subdirectories, so now they can. Two caching bugs are still open: one can briefly show stale content right after a push, and one lets a browser hang onto old styles longer than it should. That's the point of the exercise. I'd rather meet these bugs than have you meet them.

## The short list

What's left on the [1.0 milestone](https://github.com/xeek-dev/squishmark/milestone/1): finish this site, ship real docs, one more polish pass on the blue-tech theme, and a couple of caching bugs. It's the difference between "works for me" and "I'd hand this to someone else." That's the bar for 1.0.

The [changelog](/changelog) tracks what ships. Nearly there.
