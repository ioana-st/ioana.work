---
title: "Meta: Creating this site"
---

## Introduction

> This article describes how I created this website. Very meta.

This site is two things:

1. A place to host some of the things I've written. Some serve purely as portfolio pieces, some are more general articles that I felt should exist outside my company's wiki.

2. A way for me to play with static site generators, since my day job has always involved more traditional help authoring tools.

## Choosing the SSG

* My previous portfolio site was done in Jekyll, but I didn't particularly enjoy working with it (and the site definitely looked dated).

* I had previously done POCs with Hugo and Docusaurus, but I wanted to try something completely new.

* MkDocs + Material seem very popular nowadays (2025).

The decision was obvious.

## Setting up

The initial setup was done with the help of Gemini. The MkDocs and Material documentation sets are very good, so I was able to get unstuck quickly when Gemini got confused.

I built out the `mkdocs.yml` file iteratively, as I added pages to the site and needed new features.

``` yaml
site_name: ioana @ work - a technical writing portfolio
site_url: https://ioana.work/
repo_url: https://github.com/ioana-st/ioana.work
use_directory_urls: true
theme:
  name: material
  palette:
    primary: deep purple
    accent: deep orange
  font:
    text: Inter
  logo: assets/logo.png
  favicon: assets/favicon.png
  features:
    - content.code.copy  # (1)!
    - content.code.wrap  # (2)!
    - content.code.annotate   # (3)!
extra_css:
  - assets/styles.css  # (4)!
nav:
  - Home: index.md
  - About Me: about.md
  - Portfolio: portfolio.md
  - Articles: articles.md
markdown_extensions:
  - admonition  # (5)!
  - md_in_html
  - attr_list
  - toc:
      permalink: true
      toc_depth: 3
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - pymdownx.superfences  # (6)!
  - pymdownx.highlight:
      use_pygments: true  # (7)!
```

1.  Adds the "copy" button to code blocks.
2.  Wraps code samples.
3.  Enables these annotations! Requires `use_pygments: true`.
4.  The default heading colors were too boring, so I changed them through a little custom CSS.
5.  Enables professional-looking callouts.
6.  Used for code blocks like this one.
7.  Use Pygments for syntax highlighting (required for these annotations).

## Migration to Python Markdown

The materials I wanted to include on this site were in different formats:

* Jekyll-flavored Markdown

* HTML with Bootstrap

* Messy Word exported out of Confluence

* Google Docs

I decided to convert everything to Python Markdown. The bulk of the conversion was done with Gemini, with some manual fixes at the end.

The old site had a blog section, but I do not expect to post regularly, so I just converted the blog posts into standalone articles and I replaced the blog structure with a simple table with dates.
## GitHub actions

The site is automatically built and pushed to the `gh-pages` branch at every commit through a GitHub action that runs `mkdocs gh-deploy`. GitHub then runs its default action to publish the `gh-pages` branch.

No site is complete without a link checker, so I added a GitHub action that runs [Lychee](https://github.com/lycheeverse/lychee) at every commit.


## Lessons learned

* MkDocs and Material are easy and downright _fun_ to use. My first portfolio was much less pleasant to set up, mainly because the Jekyll documentation was not as good and there was no GenAI back in 2020.

* Half my problems were caused by wrong indentation and browser cache. (But Gemini could at least figure them out.)

* Gemini really struggled to convert files that contained code blocks. It could either give me a good, but alredy-rendered (thus useless) output, or it failed when trying to put code blocks inside other code blocks. I ended up telling it to ignore the code blocks and adding the formatting manually after the conversion.

* Sometimes AI is not enough and you really have to RTFM. I missed an important theme prerequisite and Gemini was trying to troubleshoot the wrong problem.

* You don't need a complicated setup to deploy a basic static site generator - I interacted with Gemini in the browser, I did all the editing in Notepad++ and I interacted with Git through a handful of terminal commands.

* Even if I don't use a "pure" docs-as-code workflow in my day job, some of the things I learned are applicable to my Flare/GitHub environment.