---
layout: page
title: Getting Started
permalink: /docs/
nav_order: 1
---

# Getting Started
{: .no_toc }

[**NFStream**][repo] is a Python package providing fast, flexible, and expressive data structures designed to make working with **online** or **offline** network data both easy and intuitive. It aims to be the fundamental high-level building block for
doing practical, **real world** network data analysis in Python. Additionally, it has
the broader goal of becoming **a common network data processing framework for researchers** providing data reproducibility across experiments.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Main Features

* **Performance:** **NFStream** is designed to be fast (x10 faster with pypy3 support) with a small CPU and memory footprint.
* **Layer-7 visibility:** **NFStream** deep packet inspection engine is based on [**nDPI**][ndpi]. It allows NFStream to perform [**reliable**][reliable] encrypted applications identification and metadata extraction (e.g. TLS, QUIC, TOR, HTTP, SSH, DNS, etc.).
* **Flexibility:** add a flow feature in 2 lines as an [**NFPlugin**][nfplugin].
* **Machine Learning oriented:** add your trained model as an [**NFPlugin**][nfplugin]. 

## Usage Examples

### Aggregating packets into flows

* Dealing with a big pcap file and just want to aggregate it as network flows? **NFStream** make this path easier in few lines:

```python
from nfstream import NFStreamer
my_awesome_streamer = NFStreamer(source="facebook.pcap", # or network interface (source="eth0")
                                 snaplen=65535,
                                 idle_timeout=30,
                                 active_timeout=300,
                                 plugins=(),
                                 dissect=True,
                                 max_tcp_dissections=10,
                                 max_udp_dissections=16,
                                 statistics=False,
                                 enable_guess=True,
                                 decode_tunnels=True,
                                 bpf_filter=None,
                                 promisc=True
)

for flow in my_awesome_streamer:
    print(flow)  # print it.
    print(flow.to_namedtuple()) # convert it to a namedtuple.
    print(flow.to_json()) # convert it to json.
    print(flow.keys()) # get flow keys.
    print(flow.values()) # get flow values.
```

```python
NFEntry(id=0,
        bidirectional_first_seen_ms=1472393122365,
        bidirectional_last_seen_ms=1472393123665,
        src2dst_first_seen_ms=1472393122365,
        src2dst_last_seen_ms=1472393123408,
        dst2src_first_seen_ms=1472393122668,
        dst2src_last_seen_ms=1472393123665,
        src_ip='192.168.43.18',
        src_ip_type=1,
        dst_ip='66.220.156.68',
        dst_ip_type=0,
        version=4,
        src_port=52066,
        dst_port=443,
        protocol=6,
        vlan_id=4,
        bidirectional_packets=19,
        bidirectional_raw_bytes=5745,
        bidirectional_ip_bytes=5479,
        bidirectional_duration_ms=1300,
        src2dst_packets=9,
        src2dst_raw_bytes=1345,
        src2dst_ip_bytes=1219,
        src2dst_duration_ms=1300,
        dst2src_packets=10,
        dst2src_raw_bytes=4400,
        dst2src_ip_bytes=4260,
        dst2src_duration_ms=997,
        expiration_id=0,
        master_protocol=91,
        app_protocol=119,
        application_name='TLS.Facebook',
        category_name='SocialNetwork',
        client_info='facebook.com',
        server_info='*.facebook.com,*.facebook.net,*.fb.com,\
                     *.fbcdn.net,*.fbsbx.com,*.m.facebook.com,\
                     *.messenger.com,*.xx.fbcdn.net,*.xy.fbcdn.net,\
                     *.xz.fbcdn.net,facebook.com,fb.com,messenger.com',
        j3a_client='bfcc1a3891601edb4f137ab7ab25b840',
        j3a_server='2d1eb5817ece335c24904f516ad5da12')

 ```
* NFStream also extracts [**60+ flow statistical features**][stat_feat]

```python
from nfstream import NFStreamer
my_awesome_streamer = NFStreamer(source="facebook.pcap", statistics=True)
for flow in my_awesome_streamer:
    print(flow)
```

```python
NFEntry(id=0,      
        bidirectional_first_seen_ms=1472393122365,
        bidirectional_last_seen_ms=1472393123665,
        src2dst_first_seen_ms=1472393122365,
        src2dst_last_seen_ms=1472393123408,
        dst2src_first_seen_ms=1472393122668,
        dst2src_last_seen_ms=1472393123665,
        src_ip='192.168.43.18',
        src_ip_type=1,
        dst_ip='66.220.156.68',
        dst_ip_type=0,
        version=4,
        src_port=52066,
        dst_port=443,
        protocol=6,
        vlan_id=4,
        bidirectional_packets=19,
        bidirectional_raw_bytes=5745,
        bidirectional_ip_bytes=5479,
        bidirectional_duration_ms=1300,
        src2dst_packets=9,
        src2dst_raw_bytes=1345,
        src2dst_ip_bytes=1219,
        src2dst_duration_ms=1300,
        dst2src_packets=10,
        dst2src_raw_bytes=4400,
        dst2src_ip_bytes=4260,
        dst2src_duration_ms=997,
        expiration_id=0,
        bidirectional_min_raw_ps=66,
        bidirectional_mean_raw_ps=302.36842105263156,
        bidirectional_stdev_raw_ps=425.53315715259754,
        bidirectional_max_raw_ps=1454,
        src2dst_min_raw_ps=66,
        src2dst_mean_raw_ps=149.44444444444446,
        src2dst_stdev_raw_ps=132.20354676701294,
        src2dst_max_raw_ps=449,
        dst2src_min_raw_ps=66,
        dst2src_mean_raw_ps=440.0,
        dst2src_stdev_raw_ps=549.7164925870628,
        dst2src_max_raw_ps=1454,
        bidirectional_min_ip_ps=52,
        bidirectional_mean_ip_ps=288.36842105263156,
        bidirectional_stdev_ip_ps=425.53315715259754,
        bidirectional_max_ip_ps=1440,
        src2dst_min_ip_ps=52,
        src2dst_mean_ip_ps=135.44444444444446,
        src2dst_stdev_ip_ps=132.20354676701294,
        src2dst_max_ip_ps=435,
        dst2src_min_ip_ps=52,
        dst2src_mean_ip_ps=426.0,
        dst2src_stdev_ip_ps=549.7164925870628,
        dst2src_max_ip_ps=1440,
        bidirectional_min_piat_ms=0,
        bidirectional_mean_piat_ms=72.22222222222223,
        bidirectional_stdev_piat_ms=137.34994188549086,
        bidirectional_max_piat_ms=398,
        src2dst_min_piat_ms=0,
        src2dst_mean_piat_ms=130.375,
        src2dst_stdev_piat_ms=179.72036811192467,
        src2dst_max_piat_ms=415,
        dst2src_min_piat_ms=0,
        dst2src_mean_piat_ms=110.77777777777777,
        dst2src_stdev_piat_ms=169.51458475436397,
        dst2src_max_piat_ms=1,
        bidirectional_syn_packets=2,
        bidirectional_cwr_packets=0,
        bidirectional_ece_packets=0,
        bidirectional_urg_packets=0,
        bidirectional_ack_packets=18,
        bidirectional_psh_packets=9,
        bidirectional_rst_packets=0,
        bidirectional_fin_packets=0,
        src2dst_syn_packets=1,
        src2dst_cwr_packets=0,
        src2dst_ece_packets=0,
        src2dst_urg_packets=0,
        src2dst_ack_packets=8,
        src2dst_psh_packets=4,
        src2dst_rst_packets=0,
        src2dst_fin_packets=0,
        dst2src_syn_packets=1,
        dst2src_cwr_packets=0,
        dst2src_ece_packets=0,
        dst2src_urg_packets=0,
        dst2src_ack_packets=10,
        dst2src_psh_packets=5,
        dst2src_rst_packets=0,
        dst2src_fin_packets=0,
        master_protocol=91,
        app_protocol=119,
        application_name='TLS.Facebook',
        category_name='SocialNetwork',
        client_info='facebook.com',
        server_info='*.facebook.com,*.facebook.net,*.fb.com,\
                     *.fbcdn.net,*.fbsbx.com,*.m.facebook.com,\
                     *.messenger.com,*.xx.fbcdn.net,*.xy.fbcdn.net,\
                     *.xz.fbcdn.net,facebook.com,fb.com,messenger.com',
        j3a_client='bfcc1a3891601edb4f137ab7ab25b840',
        j3a_server='2d1eb5817ece335c24904f516ad5da12')
```

* From pcap to Pandas DataFrame?

```python
flows_count = NFStreamer(source='devil.pcap').to_pandas(ip_anonymization=False)
my_dataframe.head(5)
```

* From pcap to csv file?

```python
flows_rows_count = NFStreamer(source='devil.pcap').to_csv(path="devil.pcap.csv",
                                                          sep="|",
                                                          ip_anonymization=False)
```
* Didn't find a specific flow feature? add a plugin to **NFStream** in few lines:

```python
from nfstream import NFPlugin
    
class packet_with_666_size(NFPlugin):
    def on_init(self, pkt): # flow creation with the first packet
        if pkt.raw_size == 666:
            return 1
        else:
            return 0
	
    def on_update(self, pkt, flow): # flow update with each packet belonging to the flow
        if pkt.raw_size == 666:
            flow.packet_with_666_size += 1
		
streamer_awesome = NFStreamer(source='devil.pcap', plugins=[packet_with_666_size()])
for flow in streamer_awesome:
    print(flow.packet_with_666_size) # see your dynamically created metric in generated flows
```

### Run your Machine Learning models

In the following, we want to run an early classification of flows based on a trained machine learning model than takes 
as features the 3 first packets size of a flow.

#### Computing required features

```python
from nfstream import NFPlugin

class feat_1(NFPlugin):
    def on_init(self, obs):
        return obs.raw_size

class feat_2(NFPlugin):
    def on_update(self, obs, entry):
        if entry.bidirectional_packets == 2:
            entry.feat_2 = obs.raw_size

class feat_3(NFPlugin):
    def on_update(self, obs, entry):
        if entry.bidirectional_packets == 3:
            entry.feat_3 = obs.raw_size
```

#### Trained model prediction

```python
class model_prediction(NFPlugin):
    def on_update(self, obs, entry):
        if entry.bidirectional_packets == 3:
            entry.model_prediction = self.user_data.predict_proba([entry.feat_1,
                                                                   entry.feat_2,
                                                                   entry.feat_3])
            # optionally we can trigger NFStreamer to immediately expires the flow
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
