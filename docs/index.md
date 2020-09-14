---
layout: page
title: Getting Started
permalink: /docs/
nav_order: 1
---

# Getting Started
{: .no_toc }

**NFStream** is a Python framework providing fast, flexible, and expressive data structures designed to make 
working with **online** or **offline** network data both easy and intuitive. It aims to be the fundamental high-level 
building block for doing practical, **real world** network data analysis in Python. Additionally, it has the broader 
goal of becoming **a common network data analytics framework for researchers** providing data reproducibility 
across experiments.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Main Features

* **Performance:** NFStream is designed to be fast: AF_PACKETV3/FANOUT on Linux, parallel processing, native C 
(using [**CFFI**][cffi]) for critical computation and [**PyPy**][pypy] support.
* **Encrypted layer-7 visibility:** NFStream deep packet inspection is based on [**nDPI**][ndpi]. 
It allows NFStream to perform [**reliable**][reliable] encrypted applications identification and metadata 
fingerprinting (e.g. TLS, SSH, DHCP, HTTP).
* **Statistical features extraction:** NFStream provides state of the art of flow-based statistical feature extraction. 
It includes both post-mortem statistical features (e.g. min, mean, stddev and max of packet size and inter arrival time) 
and early flow features (e.g. sequence of first n packets sizes, inter arrival times and
directions).
* **Flexibility:** NFStream is easily extensible using [**NFPlugins**][nfplugin]. It allows to create a new 
feature within a few lines of Python.
* **Machine Learning oriented:** NFStream aims to make Machine Learning Approaches for network traffic management 
reproducible and deployable. By using NFStream as a common framework, researchers ensure that models are trained using 
the same feature computation logic and thus, a fair comparison is possible. Moreover, trained models can be deployed 
and evaluated on live network using [**NFPlugins**][nfplugin]. 

## Installation Guide

### Python packages manager

Binary installers for the latest released version are available on Pypi.

```bash
pip install nfstream
```

### Building NFStream from sources

#### Linux Prerequisites

```bash
sudo apt-get update
sudo apt-get install autoconf automake libtool pkg-config flex bison gettext
sudo apt-get install libusb-1.0-0-dev libdbus-glib-1-dev libbluetooth-dev libnl-genl-3-dev
```

#### MacOS Prerequisites

```bash
brew install autoconf automake libtool pkg-config gettext
```

### Build Dependencies

* [**libgpg-error**](https://github.com/gpg/libgpg-error)

```bash
git clone --branch libgpg-error-1.39 https://github.com/gpg/libgpg-error
cd libgpg-error
./autogen.sh
./configure -enable-maintainer-mode --enable-static --enable-shared --with-pic --disable-doc --disable-nls
make
sudo make install
cd ..
rm -rf libgpg-error
```

* [**libgcrypt**](https://github.com/gpg/libgcrypt)

```bash
git clone --branch libgcrypt-1.8.6 https://github.com/gpg/libgcrypt
cd libgcrypt
./autogen.sh
./configure -enable-maintainer-mode --enable-static --enable-shared --with-pic --disable-doc
make
sudo make install
cd ..
rm -rf libgcrypt
```

* [**libpcap**](https://github.com/the-tcpdump-group/libpcap)

```bash
git clone --branch fanout https://github.com/tsnoam/libpcap
cd libpcap
./configure --enable-ipv6 --disable-universal --enable-dbus=no --without-libnl
make
sudo make install
cd ..
rm -rf libpcap
```

* [**nDPI**](https://github.com/ntop/nDPI)

```bash
git clone --branch dev https://github.com/ntop/nDPI.git
cd nDPI
./autogen.sh
./configure
make
sudo make install
cd ..
rm -rf nDPI
```

### Build NFStream

```bash
git clone https://github.com/nfstream/nfstream.git
cd nfstream
python3 -m pip install -r requirements.txt
python3 setup.py bdist_wheel
```

[ndpi]: https://github.com/ntop/nDPI
[nfplugin]: https://www.nfstream.org/docs/api#nfplugin
[reliable]: http://people.ac.upc.edu/pbarlet/papers/ground-truth.pam2014.pdf
[pypy]: https://www.pypy.org/
[cffi]: https://cffi.readthedocs.io/en/latest/index.html
