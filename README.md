[nfstream](https://nfstream.github.io) is a Python package providing fast, flexible, and expressive data structures 
designed to make working with **online** or **offline** network data both easy and intuitive. It aims to be the 
fundamental high-level building block for doing practical, **real world** network data analysis in Python. 
Additionally, it has the broader goal of becoming **a common network data processing framework for researchers** 
providing data reproducibility across experiments.

This repo contains the content for [nfstream web-site](https://nfstream.github.io).

The project itself can be found here: <https://github.com/aouinizied/nfstream>

## Getting Started

### Prerequisites

1. Git
2. [Jekyll](https://jekyllrb.com/docs/installation/)

### Running locally

1. Clone this repo: `git clone https://github.com/nfstream/nfstream.github.io.git`
2. Run the web-site using Bundler: `bundle exec jekyll serve`
3. Browse to `http://localhost:4000`

## Overview

This web-site is built using [Jekyll](https://jekyllrb.com/) and uses [Just The Docs](https://pmarsceill.github.io/just-the-docs/) 
theme for most of its content.

Most of the content is organized in Markdown (.md) files which are easy to generate and maintain. 
The only exceptions is the landing Page (`index.html`).

## Contribute

You are more than welcome to contribute to this web-site. 
You can do that by either opening [issues on GitHub](https://github.com/nfstream/nfstream.github.io/issues) or by adding/modifying content and create a PR.

If you decide to create a PR please make sure you:

- Test the changes on different browsers
- Test the changes on both desktop and mobile. You don't really have to run it on a mobile device, 
each desktop browser offers a mobile view of the page
