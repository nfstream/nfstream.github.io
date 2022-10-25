---
layout: page
title: Releases
permalink: /docs/releases
nav_order: 9
---

# Releases

A complete release history for NFStream is available [on GitHub](https://github.com/aouinizied/nfstream/releases).

This page contains NFStream release history

## Latest Unofficial Release (`master` branch)

- GitHub page: <https://github.com/nfstream/nfstream>{:target="_blank"}

## Latest Official Release - v6.5.3
Release date: 2022-10-26 {% include new-release.html %}

* nDPI update.
* Implement max_nflows parameter.
* Minor example fixes.

## Latest Official Release - v6.5.2
Release date: 2022-09-27

* nDPI update.
* Windows fixes.
* Multiple pcap files support.
* Wheels generation for Alpine, Linux 32bits, aarch64, Apple Silicon.

## v6.5.1

Release date: 2022-04-27

* Fix for broken pypi Linux wheels.

## v6.5.0

Release date: 2022-04-27

* Performances improvements (x3 faster): CFFI bindings from ABI to API mod.
* nDPI integration several improvements.

## v6.4.3

Release date: 2022-03-16

* Add CSV rotating files feature.
* Add pypi wheels for aarch64 and armhf.
* nDPI maintenance update.
* Fix RAW datalink handling on Windows.

## v6.4.2

Release date: 2022-02-11

* Fix for pypi missing wheels.

## v6.4.1

Release date: 2022-02-10

* nDPI maintenance update.

## v6.4.0

Release date: 2021-12-10

* Introduce Windows platform official support.
* Introduce System Visibility feature.
* Introduce Python 3.10 official support.
* nDPI maintenance update.
* Patch for RPI platforms cores detection.

## v6.3.5

Release date: 2021-10-07

* Improve IPv6 handling.
* nDPI maintenance update (memory consumption reduction).
* Fix macOS multiprocessing context.
* Fix macOS dependencies handling (NumPy and pandas)
* Improve README (ToC and related publications).


## v6.3.4

Release date: 2021-09-03

* Fix transport_size value.
* Fix requirements on MacOS and PyPy.
* Update dependencies (libgcrypt, libpcap).
* Maintainance update of nDPI.

## v6.3.3

Release date: 2021-07-02

* nDPI Performance improvements.

## v6.3.2

Release date: 2021-06-10

* Fix multiple interface capture support.
* nDPI update and minor fixes.

## v6.3.1

Release date: 2021-04-22

* Update package requirements.

## v6.3.0

Release date: 2021-04-21

* Implement tunnel_id extraction.

## v6.2.6

Release date: 2021-04-14

* Add support for pcapng format.
* Add pypy3.7 support.
* Improve errors handling.
* nDPI update and minor fixes.

## v6.2.5

Release date: 2020-11-27

* Patch for minimal truncated UDP raw pcap handling.

## v6.2.4

Release date: 2020-11-23

* Minor fixes (csv and pandas interfaces).
* nDPI maintenance update.

## v6.2.3

Release date: 2020-11-15

* Patch fixing BPF filtering on live interface.

## v6.2.2

Release date: 2020-11-11

* Patch fixing anonymization on user defined plugins.

## v6.2.1

Release date: 2020-11-10

* Add raw IPv4/IPv6 datalink type support.
* Make plugins importable.
* Improve libpcap integration.
* nDPI maintenance update.
* Fix IPv6 extension headers handling.
* Fix autoscale.
* Fix DHCP plugin.
* Reduce startup drops.


## v6.2.0

Release date: 2020-10-21

* Complete rework of multi cpus scaling.
* Add src_mac, src_oui, dst_mac, dst_oui flow features.
* Add MDNS and DHCP plugins.
* Add configurable anonymization.
* Add Python3.9 support.
* Fix overflow in performance report.
* Fix CAPWAP tunnels decoding.
* nDPI maintenance update.

## v6.1.3

Release date: 2020-09-21

* Add user_agent extraction on QUIC.


## v6.1.2

Release date: 2020-09-17

* Limit contention by excluding hyperthreading from scaling heuristic.
* Change idle and active timeouts default values. 
* Minor fixes and optimizations.
* Fix Travis CI timeouts issues.

## v6.1.1

Release date: 2020-09-14

* Optimize PACKET_FANOUT on Linux (cpu affinity assigning strategy).
* Fix keybord interupt handling issue.
* Implement Json periodic performance report for live capture.

## v6.0.5

Release date: 2020-09-13

* Enable PACKET_FANOUT on Linux (Kernel load balancing).
* Improve Keyboard Interrupt handling.

## v6.0.4

Release date: 2020-09-12

* Live capture performances optimization, Fix for AF_PACKETv3.
* CPU usage optimization (Remove active polling mechanism).
* Add support of libgcrypt for QUIC improved disssection.

## v6.0.3

Release date: 2020-09-10

* Upgrade libpcap to 1.10.0.

## v6.0.2

Release date: 2020-09-09

* Fix performance summary on live interface.

## v6.0.0

Release date: 2020-09-09

* Introduce parallelized metering architecture.
* Rework feature computation engine (performance boosted).
* Introduce SPLT analysis.
* Add configurable accounting mode.
* Add performance reporting feature.
* Improve Pandas and CSV interfaces.
* nDPI maintenance update.
* Several bug fixes and improvements.
* Major refactoring.

## v5.2.0

Release date: 2020-07-18

* Switch to pure header based packet sizes computation.
* Fix some minor issue with to_pandas() method.
* Drop account_ip_padding_size option.

## v5.1.6

Release date: 2020-07-11

* nDPI maintenance update (12abcd516b468f6e0070308fa57052b93aa3a3ca).
* Fix payload size computation.
* Add length check for MPLS.
* ZMQ sockets graceful close.

## v5.1.5

Release date: 2020-05-23

* Fix broken wheels.

## v5.1.4

Release date: 2020-05-23

* nDPI maintenance update (e5c2c400efa7a9b239be36ca95dd296a779ae363)
* Fix VLAN_ID issue.

## v5.1.3

Release date: 2020-05-21

* Add ip anonymization option.
* Add ip_src_type and ip_dst_type features.
* Add support for arm64 architecture.

## v5.1.2

Release date: 2020-05-19

* Improve NULL values handling.
* Improve pandas dataframe types handling.

## v5.1.1

Release date: 2020-05-18

* Fix custom expiration handling..
* Path for server_names handling.

## v5.1.0

Release date: 2020-05-07

* Add to_csv export feature.
* Rework to_pandas export.
* Rework libpcap setup.
* Patch for IPv6 support.

## v5.0.0

Release date: 2020-05-06

* Rework packet observation module.
* Add tunnel decoding feature.
* Add BPF Filtering feature.
* Add promisc option.
* Add wheels for py36, py38, pypy3 on MacOS.
* Maintenance update of nDPI (commit: 4148c5e065d32128eea17c0e228e372ad72eef82).

## v4.0.1

Release date: 2020-04-23

* Add to_json method.
* Add enable_guess parameter.
* Fix NFPlugins expiration handling.
* Improve IPv6 extensions handling.
* Maintenance update of nDPI.

## v4.0.0 

Release date: 2020-04-15

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
