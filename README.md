
## Introduction

This introduction to Dr. Henry will explain what it is, where it comes from, and what it aims to be. Much inspiration (and code) comes from other projects, credited in the features and credits section below.

## Goals

The ultimate goal of Dr. Henry is to have a nice way to make use of [GitHub Pages](https://pages.github.com), and depending on nothing but [git](http://git-scm.com/), and your favourite text editor while working with it. This does bring certain limitations relative to other methods of publishing to GitHub Pages, but it was the work flow I wanted, letting GitHub generate the pages after a check-in.

I also wanted it to be themable, with the ability to fallback w/ override in a modular way. Also, try to be as agnostic as possible to differing frameworks.

For now, a basic blog and home page is the main goal. There's others, but they depend on upgrades happening at GitHub Pages.

## Features

[Dr. Henry](https://github.com/jhohertz/dr-henry) has:

- [Jekyll](http://jekyllrb.com/) boilerplate *strictly* targeting the [current GitHub Pages](https://pages.github.com/versions/) environment
- self hosting on [GitHub Pages](https://pages.github.com/), publish with just git and an editor
- themeable with layered themes (IE: local, child, parent, core; with fallback from left to right)
- several themes included, meant to illustrate using base themes like "bootstrap" and "foundation", with local overrides to implement styling
- pluggable social, comments, analytics, converted from the projects Dr. Henry takes from.
- uses files in the _data folder for configuring much of this
- uses the plugins github pages supports, sitemap, emoji :smile: and more.

## Themes 

These themes are included with Dr. Henry:

### Core theme

The core theme is the foundation of Dr. Henry, and offers default implementations of various fragments, and the conventions that other themes should usually follow so everything works. It's format is a header/body/footer sandwich with an optional sidebar with the body.

HTML5 is assumed. Semantic tags are used.

### Base Themes

- bootstrap2: adapted from "twitter" theme from [Jekyll Bootstrap](http://jekyllbootstrap.com/)
- bootstrap3: adapted from "bootstrap3" theme from [Jekyll Bootstrap](http://jekyllbootstrap.com/) (default bootstrap3 styling)
- foundation5: a minimally implemented foundation5 base theme

## Top level themes

- jekb: child theme of bootstrap3, implements look of "bootstrap3" theme from Jekyll Bootstrap
- octo: a conversion of the [Octopress 2.0](http://octopress.org) "classic" default theme.
- octolost: a conversion of [OctoFound](https://github.com/annejohnson/octofound), as a child theme of foundation5

## Status

At this stage, Dr. Henry is a proof of concept that seems to be working fairly well, and a lot more that can be done to refine it over time. There are some notable gaps that are deferred as future versions of GitHub Pages may introduce features that will greatly enable implementation.

- no category landing pages (resulting in a blog-only format)
- lots of cleanups yet to do, things we can factor out
- some things carried over from other projects but not verified
- no real work on formalizing front-matter data yet
- it lacks documentation at the moment. 
  - for now, look at the [core theme](https://github.com/jhohertz/dr-henry/tree/develop/_includes/themes/core) items, the [_data files](https://github.com/jhohertz/dr-henry/tree/develop/_data) files, and the [config.yml](https://github.com/jhohertz/dr-henry/blob/develop/_config.yml) file.
  - then override if needed in top level themes like [jekb](https://github.com/jhohertz/dr-henry/tree/develop/_includes/themes/jekb), [octo](https://github.com/jhohertz/dr-henry/tree/develop/_includes/themes/octo), or [octolost](https://github.com/jhohertz/dr-henry/tree/develop/_includes/themes/octolost), or make your own.
- the tooling is lacking yet.
  - a command-line helper is planned for creating pages/posts, w/ useful, commented front matter.
- the template rendering generates a LOT of vertical white space. Browsers mostly ignore this, so we accept it most places in the name of readability of the templates, but it in some places we do things on one line where we need tight control of spacing. 

## The Future

Besides general cleanups and factoring improvements, the main thing to look forward to are collections, which itself will enable a lot of other possibilities. We can have types other than the general pages or posts, custom, with their own structure and rendering. This is a feature of Jekyll 2.0+, which is not yet supported on GitHub Pages (at 1.5.2 as of this writing).

That will let you make category metadata and page rendering quite easily. (if permalinks can be set as /:category, which probably should be fine, but not yet proven.)

The newer Jekyll version also allows for [sass/scss](http://sass-lang.com/) processing, and both upstream Octopress and Octofound make use of it. (Dr. Henry takes a snapshot of their outputs currently.)

## Credits

This work is based first, on the projects from which Dr. Henry was created.

The efforts of these folks, who probably have no idea about this project, taken from the URL on their GitHub user pages:

- [Jade Dominguez](http://plusjade.com/) for Jekyll Bootstrap, which Dr. Henry, started as a fork of, and is the original "just an editor" minded scaffold for jekyll targeting GitHub Pages.
- [Anne K. Johnson](http://annekjohnson.com/) for Octofound, a nicely done theme for Octopress based on the latest version of Foundation, which Dr. Henry's "octolost" theme is derived from.
- [Brandon Mathis](http://brandonmathis.com/) for Octopress 2.0, which in some ways, was sort of ported into Jekyll Bootstrap once the theme loader was working.
- And of course the people contributing to those projects.

And I guess me. Joe Hohertz. No web page yet, but... once I get this thing tagged, here I come....

## Licence

This project is licenced under the [MIT Licence](https://github.com/jhohertz/dr-henry/blob/develop/LICENCE.md), like the projects it derives from.

