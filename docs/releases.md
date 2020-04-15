---
layout: page
title: Releases
permalink: /docs/releases
nav_order: 9
---

# Releases

A complete release history for nfstream is available [on GitHub](https://github.com/aouinizied/nfstream/releases).

This page contains nfstream release history

## Latest Unofficial Release (`master` branch)

- GitHub page: <https://github.com/aouinizied/nfstream>{:target="_blank"}

## Latest Official Release - v4.0.0 

Release date: 2020-04-15 {% include new-release.html %}

* Documentation on: https://nfstream.github.io/
* 90+ flow statistical features extraction.
* nDPI version update.
* ZMQ improvements.

## v3.2.2 

Release date: 2020-02-29

* Add to_pandas method.
* Live demo using binder
* Fix previous broken package.

## v3.2.1 

Release date: 2020-02-29

* Add libpcap to builded libs.
* Packet parsing improvements (expose several packet size levels).
* Observer code refactoring.
* Improve nDPI structures handling.

## v3.2.0 

Release date: 2020-02-02

* nDPI 3.2 support.
* Fix metadata extraction issues.

## v3.1.2 

Release date: 2020-01-07

* Fix tests workflow.
* Update nDPI (commit: 73c7ccdb65a1e13e3fb1726af7882dd34534906f).

## v3.1.1 

Release date: 2019-12-29

* Fix generated wheels. (drop sdist)

## v3.1.0 

Release date: 2019-12-29

* Initial support for nDPI3.1 (commit: 73c7ccdb65a1e13e3fb1726af7882dd34534906f).
* Add wrapping for pandas.
* pypy7.2 support.
* Add py36, py38 for macOS wheels.
* Move continous integration to GitHub Actions.

## v3.0.4 

Release date: 2019-12-18

* Fix pypi description rendering.

## v3.0.3 

Release date: 2019-12-18

* MacOS Catalina support.
* Implement random port selection for zmq.

## v3.0.2 

Release date: 2019-12-06

* ether type double stacking implementation.
* Minor fixes.

## v3.0.1 

Release date: 2019-12-04

* Fix macOS wheels 10.14

## v3.0.0 

Release date: 2019-12-04

* Sync with nDPI major.minor versions.
* New NFPlugin API definition.
* Fix macOS wheels for 10.13 and 10.14

## v2.0.1 

Release date: 2019-11-29

* Fix pypy3 wheel.

## v2.0.0 

Release date: 2019-11-28

* Pypy support.
* Major performances improvements.
* NFPlugin as main extension API.
* nDPI memory usage improved.
* nDPI implemented using cffi.
* tcp_max_dissections, udp_max_dissections options.
* NFFlow dynamic attributes creation.
* HTTP, SSH, DNS client and server informations extraction.
* FlowCache management implemented in pure Python.

## v1.2.1 

Release date: 2019-11-15

* Fix ndpi padding and alignement issues.
* nDPI3.1 compatibility.

## v1.2.0 

Release date: 2019-11-14

* Fix ndpi bindings.
* Add TLS dissection features (server sni, client sni, version, organization, expiration dates)
* Improve documentation.

## v1.1.8 

Release date: 2019-11-07

* Fix ndpi wrap missing fields.
* Add host_server_name metric.
* Update doc.

## v1.1.7 

Release date: 2019-11-07

* Fix minor bugs.

## v1.1.6 

Release date: 2019-11-03

* TCP flags extraction.
* Minor bug fixes.

## v1.1.5 

Release date: 2019-11-02

* Add BPF filtering feature.
* Fix radiotap parsing.

## v1.1.2-3-4 

Release date: 2019-11-01

* Fix broken macos wheels on pypi.

## v1.1.1 

Release date: 2019-11-01

* Fix broken linux wheels on pypi.
* Py38 compatibility.

## v1.1.0 

Release date: 2019-11-01

* Add OSX support.

## v1.0.1-2-3 

Release date: 2019-10-31

* Fix deployment CI


## v1.0.0 

Release date: 2019-10-30

* cffi based packet capture.
* fast parsing mechanism.
* Minor bug fixes.
* auto-generate binaries.

## v0.5.0 

Release date: 2019-10-21

* Classifier mechanism introduced.
* Custom export_reason.
* Fix minor bugs.
* Improve documentation.

## v0.4.0 

Release date: 2019-10-20

* Pypi package description readable.

## v0.3.1 

Release date: 2019-10-20

* Add category_name as flow feature.

## v0.3.0 

Release date: 2019-10-20

* Add user defined callbacks feature.
* Fix live capture handling.
* Fix library loading path.
* Json support for flow printing.
* Add examples.

## v0.2.0 

Release date: 2019-10-19

* Add nDPI bindings as part of the released package
* Documentation improvement

## v0.1.0 

Release date: 2019-10-19

* First release on PyPI.
