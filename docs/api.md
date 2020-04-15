---
layout: page
title: APIs Documentation
permalink: /docs/api
nav_order: 4
---

# APIs Documentation

<img src="{{ site.baseurl }}/resources/architecture_nfstream.png" alt="drawing" width="730"/>

A step by step walk through each process involved when performing flow monitoring is depicted in the above schema.
Our aim is to provide you with a reminder about how things works in theory. Consequently, an easier understanding of 
nfstream features and implementation is possible.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## NFObserver: The Packet Observation Layer

NFObserver is the implementation of packet observation layer within a flow monitoring framework and performs currently
the following steps:

### Packet capture
This step is performed on the Network Interface Card (NIC) level. After passing various checks such 
as checksum error, packets stored in on-card reception buffers are moved to the hosting device memory. 
Several libraries are available to capture network traffic such as libpcap for UNIX based operating systems Winpcap 
for Windows. nfstream implementation is based on libpcap.

### Truncation
Defining a snapshot length, the process selects precise bytes from the packet. It is performed in some cases to reduce
the amount of data captured and therefore CPU and bus bandwidth load.
 
### Timestamping
As packets may come from several observation points, reordering process is based on packet’s timestamp. 
While hardware timestamping provides a high accuracy up to 100 nanoseconds in case of the IEEE 1588 protocol, 
it’s not supported by most of commodity NIC. nfstream is based on software timestamping which is widely used to outcome
 this lack providing milliseconds accuracy.

### NFPacket
In addition to these steps, NFObserver parses the packet in order to extract a set of informations (IP and 
transport levels mainly) and share it with the upper layer using **NFPacket** object.

| `time` | `int`  | Packet timestamp in milliseconds |
| `raw_size` | `int`  | Packet raw size |
| `ip_size` | `int`  | IP packet size |
| `transport_size` | `int`  | Transport packet size |
| `payload_size` | `int`  | Packet payload size |
| `ip_src` | `int`  | Source IP address numeric value |
| `ip_dst` | `int`  | Destination IP address numeric value |
| `src_port` | `int`  | Transport layer source port |
| `dst_port` | `int`  | Transport layer destination port |
| `protocol` | `int`  | Transport layer protocol |
| `vlan_id` | `int`  | Virtual LAN identifier |
| `version` | `int`  | IP version |
| `tcp_flags` | `namedtuple`  | Packet observed TCP flags |
| `ip_packet` | `bytes`  | Raw content starting from IP Header |
| `direction` | `int`  | Packet direction: 0 for src_to_dst and 1 for dst_to_src |

## NFCache: The Flow Metering Layer

NFCache is the implementation of flow metering layer within a flow monitoring framework and performs currently
the following steps.
- Aggregate packet into flow.
- Compute flow metrics.
- Manage flow expiration. 

### NFEntry Cache
NFEntry Cache consist of a fixed set of LRU caches in which the metering process stores information 
regarding active flows. 
A computed hash (IP source and destination addresses, source and destination ports, protocol and the VLAN identifier) 
determines whether NFPacket is matching an existing NFEntry in the cache or not. 
In the first case, flow metrics are updated. 
In the latter one, a new flow is created. 
We define a flow as bidirectional when we consider a pair of IP adresses/Transport ports and it reverse belongs to 
same flow.

### Expiration Manager
NFCache entries are cached until they are considered as terminated. Termination of a flow is triggered by an 
expiration rule. Currently, the expiration manager consider a flow as expired based on:

- Active timeout: Flow expires after being considered active during a certain period.
- Idle timeout: Flow expires if no packets belonging to it are observed during a specific period.
- Custom expiration: User defined rule.

It is possible to configure our metering process based on expiration policy to reduce the amount of records exported.


### NFPlugin

A flow in nfstream is defined as an **NFEntry**. NFEntry is defined as a set of **NFPlugins** outputs. Each time 
a flow is created, updated or expired, a set of NFPlugins perform the required computations.

```python
class NFPlugin(object):
    def __init__(self, name=None, volatile=False, user_data=None):
        """
        NFPlugin Parameters:
        name [default= class name]: Plugin name. Must be unique as it’s dynamically created 
                                    as a flow attribute.
        volatile [default= False] : Volatile plugin is available only when flow is processed.
                                    At flow expiration level, plugin is automatically removed
                                    (will not appear as flow attribute).
        user_data [default= None] : user_data passed to the plugin. Example: external module,
                                    pickled sklearn model, etc.
        """
    def on_init(self, obs):
        """
        on_init(self, obs): Method called at entry creation. When aggregating packets into 
                            flows, this method is called on NFEntry object creation based on
                            first NFPacket object belonging to it.
        """
        return 0

    def on_update(self, obs, entry):
        """
        on_update(self, obs, entry): Method called to update each entry with its belonging obs. 
                                     When aggregating packets into flows, the entry is an NFEntry
                                     object and the obs is an NFPacket object.
        """
        pass

    def on_expire(self, entry):
        """
        on_expire(self, entry):      Method called at entry expiration. When aggregating packets
                                     into flows, the entry is an NFEntry
        """
        pass

    def cleanup(self):
        """
        cleanup(self):               Method called for plugin cleanup.
        """
        pass
```

### NFEntry

NFEntry is built using a set of core NFPlugins and extensible using user defined NFPlugins. The core 
implemented non volatile NFPlugins output construct the **NFEntry** exposed by NFCache.

| `id` | `int`  | Flow identifier |
| `first_seen` | `int`  | First packet timestamp in milliseconds |
| `last_seen` | `int`  | Last packet timestamp in milliseconds |
| `version` | `int`  | IP version |
| `src_port` | `int`  | Transport layer source port |
| `dst_port` | `int`  | Transport layer destination port |
| `protocol` | `int`  | Transport layer protocol |
| `vlan_id` | `int`  | Virtual LAN identifier |
| `src_ip` | `str`  | Source IP address string representation |
| `dst_ip` | `str`  | Destination IP address string representation |
| `ip_src` | `int`  | Source IP address int value [volatile] |
| `ip_dst` | `int`  | Destination IP address int value [volatile] |
| `total_packets` | `int`  | Flow packets accumulator |
| `total_bytes` | `int`  | Flow bytes (full packet length) accumulator |
| `duration` | `int`  | Flow duration in milliseconds |
| `src2dst_packets` | `int`  | Flow packets accumulator (source->destination) |
| `src2dst_bytes` | `int`  | Flow bytes (full packet length) accumulator (source->destination) |
| `dst2src_packets` | `int`  | Flow packets accumulator (destination->source) |
| `dst2src_bytes` | `int`  | Flow bytes (full packet length) accumulator (destination->source).
| `expiration_id` | `int`  | Identifier of flow expiration trigger. Can be 0 for idle_timeout, 1 for active_timeout or negative for custom expiration |
| `master_protocol` | `int`  | nDPI master protocol identifier |
| `app_protocol` | `int`  | nDPI app protocol identifier |
| `application_name` | `str`  | nDPI application name |
| `category_name` | `str`  | nDPI application category name |
| `client_info` | `str`  | Dissected client informations. Can be http_detected_os for HTTP, client_signature for SSH or client_requested_server_name for SSL |
| `server_info` | `str`  | Dissected server informations. Can be host_server_name for HTTP or DNS, server_signature for SSH or server_names for SSL |
| `j3a_client` | `str`  | J3A client fingerprint |
| `j3a_server` | `str`  | J3A server fingerprint |

## NFStreamer: The Export Layer

NFStreamer is the main class of nfstream framework and implements also the export layer within a flow monitoring schema.
```python
my_capture_streamer = NFStreamer(source="facebook.pcap", # or live interface
                                 snaplen=65535,
                                 idle_timeout=30,
                                 active_timeout=300,
                                 plugins=(),
                                 dissect=True,
                                 max_tcp_dissections=10,
                                 max_udp_dissections=16,
                                 statistics=False,
                                 account_ip_padding_size=False
)

"""
NFStreamer Parameters:
        source [default= None]:                   Source of packets.
                                                  Can be live_interface_name or pcap_file_path.
        snaplen [default= 65535]:                 Packet capture length.
        idle_timeout [default= 30]:               Flows that are inactive for more than this value in 
                                                  seconds will be exported.
        active_timeout [default= 300]:            Flows that are active for more than this value in 
                                                  seconds will be exported.
        plugins [default= ()]:                    Set of user defined NFPlugins.
        dissect [default= True]:                  Enable nDPI deep packet inspection library for 
                                                  Layer 7 visibility.
        max_tcp_dissections [default= 10]:        Maximum per flow TCP packets to dissect
                                                  (ignored when dissect=False).

        max_udp_dissections [default= 16]:        Maximum per flow UDP packets to dissect 
                                                  (ignored when dissect=False).
        statistics [default= False]:              Enable statistical flow features extraction.
        account_ip_padding_size [default= False]: Enable Ethernet padding accounting when reporting IP sizes. 
"""
```

You can iterate over a streamer
```python
for flow in my_capture_streamer:
    print(flow)
```
or convert it to a pandas Dataframe.
```python
df = my_capture_streamer.to_pandas()
df.head(5)
```


