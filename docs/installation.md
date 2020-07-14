---
layout: page
title: Installation Guide
permalink: /docs/installation
nav_order: 2
---

# Installation Guide
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Using Package Installer for Python

Binary installers for the latest released version are available.

```shell
python3 -m pip install nfstream
```

## Build From Source Code

If you want to build NFStream from sources on your local machine:

### Linux Instructions

```shell
sudo apt-get install autoconf automake libtool pkg-config libpcap-dev
git clone https://github.com/aouinizied/nfstream.git
cd nfstream
python3 -m pip install -r requirements.txt
python3 setup.py bdist_wheel
```
### MacOS Instructions

```shell
brew install autoconf automake libtool pkg-config
git clone https://github.com/aouinizied/nfstream.git
cd nfstream
python3 -m pip install -r requirements.txt
python3 setup.py bdist_wheel
```