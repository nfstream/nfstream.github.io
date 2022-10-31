---
layout: page
title: Getting Started
permalink: /docs/
nav_order: 1
---

# Getting Started
{: .no_toc }

**NFStream** is a multiplatform Python framework providing fast, flexible, and expressive data structures designed to make 
working with **online** or **offline** network data easy and intuitive. It aims to be Python's fundamental high-level 
building block for doing practical, **real-world** network flow data analysis. Additionally, it has the broader 
goal of becoming **a unifying network data analytics framework for researchers** providing data reproducibility 
across experiments.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Main Features

* **Performance:** NFStream is designed to be fast: [**AF_PACKET_V3/FANOUT**][packet] on Linux, multiprocessing, native
[**CFFI based**][cffi] computation engine, and [**PyPy**][pypy] full support.
* **Encrypted layer-7 visibility:** NFStream deep packet inspection is based on [**nDPI**][ndpi]. 
It allows NFStream to perform [**reliable**][reliable] encrypted applications identification and metadata 
fingerprinting (e.g. TLS, SSH, DHCP, HTTP).
* **System visibility:** NFStream probes the monitored system's kernel to obtain information on open Internet sockets 
and collects guaranteed ground-truth (process name, PID, etc.) at the application level.
* **Statistical features extraction:** NFStream provides state of the art of flow-based statistical feature extraction. 
It includes post-mortem statistical features (e.g., minimum, mean, standard deviation, and maximum of packet size and 
inter-arrival time) and early flow features (e.g. sequence of first n packets sizes, inter-arrival times, and directions).
* **Flexibility:** NFStream is easily extensible using [**NFPlugins**][nfplugin]. It allows the creation of a new flow 
feature within a few lines of Python.
* **Machine Learning oriented:** NFStream aims to make Machine Learning Approaches for network traffic management 
reproducible and deployable. By using NFStream as a common framework, researchers ensure that models are trained using 
the same feature computation logic, and thus, a fair comparison is possible. Moreover, trained models can be deployed 
and evaluated on live networks using [**NFPlugins**][nfplugin]. 

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
sudo apt-get install python3-dev autoconf automake libtool pkg-config flex bison gettext libjson-c-dev
sudo apt-get install libusb-1.0-0-dev libdbus-glib-1-dev libbluetooth-dev libnl-genl-3-dev
```

#### MacOS Prerequisites

```bash
brew install autoconf automake libtool pkg-config gettext json-c
```

### Windows Prerequisites

On Windows, NFStream build system is based on MSYS2. Please follow [**msys2 installation guide**][msys2] before moving to 
the next steps.

```bash
pacman -S git unzip mingw-w64-x86_64-toolchain automake1.16 automake-wrapper autoconf libtool make mingw-w64-x86_64-json-c mingw-w64-x86_64-crt-git
```

Note that you will also need to have npcap installed according to [**these instructions**][npcap].

### Build NFStream

```bash
git clone --recurse-submodules https://github.com/nfstream/nfstream.git
cd nfstream
python3 -m pip install --upgrade pip
python3 -m pip install -r dev_requirements.txt
python3 -m pip install .
```

[ndpi]: https://github.com/ntop/nDPI
[nfplugin]: https://www.nfstream.org/docs/api#nfplugin
[reliable]: http://people.ac.upc.edu/pbarlet/papers/ground-truth.pam2014.pdf
[pypy]: https://www.pypy.org/
[cffi]: https://cffi.readthedocs.io/en/latest/index.html
[msys2]: https://www.msys2.org/
[npcap]: https://npcap.com/guide/npcap-users-guide.html
[packet]: https://manned.org/packet.7
