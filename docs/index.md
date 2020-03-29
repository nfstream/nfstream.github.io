---
layout: page
title: Getting Started
permalink: /docs/
nav_order: 1
---

# Getting Started
{: .no_toc }

[**nfstream**][repo] is a Python package providing fast, flexible, and expressive data structures designed to make working with **online** or **offline** network data both easy and intuitive. It aims to be the fundamental high-level building block for
doing practical, **real world** network data analysis in Python. Additionally, it has
the broader goal of becoming **a common network data processing framework for researchers** providing data reproducibility across experiments.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Main Features

* **Performance:** **nfstream** is designed to be fast (x10 faster with pypy3 support) with a small CPU and memory footprint.
* **Layer-7 visibility:** **nfstream** deep packet inspection engine is based on [**nDPI**][ndpi]. It allows nfstream to perform [**reliable**][reliable] encrypted applications identification and metadata extraction (e.g. TLS, QUIC, TOR, HTTP, SSH, DNS, etc.).
* **Flexibility:** add a flow feature in 2 lines as an [**NFPlugin**][nfplugin].
* **Machine Learning oriented:** add your trained model as an [**NFPlugin**][nfplugin]. 

## Usage Examples

### Aggregating packets into flows

Dealing with a big pcap file and just want to aggregate it as network flows? **nfstream** make this path easier in few lines:

```python
from nfstream import NFStreamer
my_awesome_streamer = NFStreamer(source="facebook.pcap") # or network interface (source="eth0")
for flow in my_awesome_streamer:
    print(flow)  # print it, append to pandas Dataframe or whatever you want :)!
```

```python
NFEntry(
    id=0,
    first_seen=1472393122365,
    last_seen=1472393123665,
    version=4,
    src_port=52066,
    dst_port=443,
    protocol=6,
    vlan_id=0,
    src_ip='192.168.43.18',
    dst_ip='66.220.156.68',
    total_packets=19,
    total_bytes=5745,
    duration=1300,
    src2dst_packets=9,
    src2dst_bytes=1345,
    dst2src_packets=10,
    dst2src_bytes=4400,
    expiration_id=0,
    master_protocol=91,
    app_protocol=119,
    application_name='TLS.Facebook',
    category_name='SocialNetwork',
    client_info='facebook.com',
    server_info='*.facebook.com,*.facebook.net,*.fb.com,*.fbcdn.net,*.fbsbx.com',
    j3a_client='bfcc1a3891601edb4f137ab7ab25b840',
    j3a_server='2d1eb5817ece335c24904f516ad5da12'
)
 ```

### Getting flows into pandas

Convert your packet to flow Pandas Dataframe using **nfstream**

```python
my_dataframe = NFStreamer(source='devil.pcap').to_pandas()
my_dataframe.head(5)
```

### Adding a flow feature

Didn't find a specific flow feature? add a plugin to **nfstream** in few lines:

```python
from nfstream import NFPlugin
    
class ack_count(NFPlugin):
    def on_init(self, pkt): # flow creation with the first packet
	    if pkt.tcpflags.ack == 1:
	        return 1
	    else:
	        return 0
	def on_update(self, pkt, flow): # flow update with each packet belonging to the flow
	    if pkt.tcpflags.ack == 1:
	        flow.ack_count += 1
		
streamer_awesome = NFStreamer(source='devil.pcap', plugins=[ack_count()])
for flow in streamer_awesome:
    print(flow.ack_count) # see your dynamically created metric in generated flows
```

### Run your Machine Learning models

In the following, we want to run an early classification of flows based on a trained machine learning model than takes 
as features the 3 first packets size of a flow.

#### Computing required features

```python
from nfstream import NFPlugin

class feat_1(NFPlugin):
    def on_init(self, obs):
        entry.feat_1 == obs.raw_size

class feat_2(NFPlugin):
    def on_update(self, obs, entry):
        if entry.total_packets == 2:
            entry.feat_2 == obs.raw_size

class feat_3(NFPlugin):
    def on_update(self, obs, entry):
        if entry.total_packets == 3:
            entry.feat_3 == obs.raw_size
```

#### Trained model mrediction

```python
class model_prediction(NFPlugin):
    def on_update(self, obs, entry):
        if entry.total_packets == 3:
            entry.model_prediction = self.user_data.predict_proba([entry.feat_1,
                                                                   entry.feat_2,
                                                                   entry.feat_3])
            # optionally we can force NFStreamer to immediately expires the flow
            # entry.expiration_id = -1
```

#### Start your ML powered streamer

```python
my_model = function_to_load_your_model() # or whatever
ml_streamer = NFStreamer(source='devil.pcap',
                         plugins=[feat_1(volatile=True),
                                  feat_2(volatile=True),
                                  feat_3(volatile=True),
                                  model_prediction(user_data=my_model)
                                  ])
for flow in ml_streamer:
     print(flow.model_prediction) # now you will see your trained model prediction.
```

[ndpi]: {{ site.baseurl }}/docs/visibility
[nfplugin]: {{ site.baseurl }}/docs/api#nfplugin
[reliable]: http://people.ac.upc.edu/pbarlet/papers/ground-truth.pam2014.pdf
[repo]: https://github.com/aouinizied/nfstream
[demo]: https://mybinder.org/v2/gh/aouinizied/nfstream-tutorials/master?filepath=demo_notebook.ipynb
