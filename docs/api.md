---
layout: page
title: APIs Documentation
permalink: /docs/api
nav_order: 4
---

# APIs Documentation

<img src="{{ site.baseurl }}/resources/architecture_nfstream.png" alt="drawing" width="730"/>

The flow-based aggregation consists of aggregating packets into flows based on a shared set of characteristics 
(flow key, e.g., source IP address, destination IP address, transport protocol, source port, destination port, 
VLAN identifier, tunnel Identifier). A flow cache maintains each flow entry until its termination (e.g., active timeout, inactive timeout).
While the entry is present in the flow cache, basic counters, and several metrics are updated. 
If two pairs generate flows on both directions, the flow cache uses a bidirectional flow definition, adding counters 
and metrics for both directions.

In the above schema, NFStream overall architecture is depicted and could be summarized as follows:
* NFStreamer: the driver process, it is responsible of setting the overall workflow which is mainly an orchestration of 
parallel metering processes.
* Meters: are the core parts of NFStream framework. Raw packets are processed (e.g. timestamped, decoded, truncated) 
and dispatched across meters. Each meter is able to aggregate packet information into flow and compute required features 
until flow expiration is triggered (active timeout, inactive timeout).

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## NFStreamer

```python
from nfstream import NFStreamer
my_streamer = NFStreamer(source="facebook.pcap",
                         decode_tunnels=True,
                         bpf_filter=None,
                         promiscuous_mode=True,
                         snapshot_length=1536,
                         idle_timeout=120,
                         active_timeout=1800,
                         accounting_mode=0,
                         udps=None,
                         n_dissections=20,
                         statistical_analysis=False,
                         splt_analysis=0,
                         n_meters=0,
                         performance_report=0)
```

### NFStreamer attributes

| `source` | `[default=None]` | Packet capture source. Pcap file path or network interface name. |
| `decode_tunnels` | `[default=True]` | Enable/Disable GTP/CAPWAP/TZSP tunnels decoding. |
| `bpf_filter` | `[default=None]` | Specify a [BPF filter][bpf] filter for filtering selected traffic.  |
| `promiscuous_mode` | `[default=True]` | Enable/Disable promiscuous capture mode.  |
| `snapshot_length` | `[default=1536]` | Control packet slicing size (truncation) in bytes.  |
| `idle_timeout` | `[default=120]` | Flows that are idle (no packets received) for more than this value in seconds are expired. |
| `active_timeout` | `[default=1800]` | Flows that are active for more than this value in seconds are expired.  |
| `accounting_mode` | `[default=0]` | Specify the accounting mode that will be used to report bytes related features (0: Link layer, 1: IP layer, 2: Transport layer, 3: Payload).  |
| `udps` | `[default=None]` | Specify user defined NFPlugins used to extend NFStreamer. |
| `n_dissections` | `[default=20]` | Number of per flow packets to dissect for L7 visibility feature. When set to 0, L7 visibility feature is disabled. |
| `statistical_analysis` | `[default=False]` | Enable/Disable post-mortem flow statistical analysis.  |
| `splt_analysis` | `[default=0]` | Specify the sequence of first packets length for early statistical analysis. When set to 0, splt_analysis is disabled. |
| `n_meters` | `[default=0]` | Specify the number of parallel metering processes. When set to 0, NFStreamer will automatically scale metering according to available physical cores on the running host. |
| `performance_report` | `[default=0]` | [**Performance report**](https://github.com/nfstream/nfstream/blob/master/assets/PERFORMANCE_REPORT.md) interval in seconds. Disabled whhen set to 0. Ignored for offline capture. |

### NFStreamer methods

#### Flow interation method

```python
for flow in my_streamer:
   print(flow) # or whatever
```

#### Pandas dataframe conversion

```python
my_dataframe = my_streamer.to_pandas(columns_to_anonymize=[])
my_dataframe.head()
```

| `columns_to_anonymize` | `[default=[]]` | List of columns names to anonymize. Anonymization is based on a random secret key generation at each start of NFStreamer. The generated key is used to anonymize configured values using blake2b algorithm. |

#### CSV file conversion

```python
total_flows_count = my_streamer.to_csv(path=None, columns_to_anonymize=[], flows_per_file=0)
```

| `path` | `[default=None]` | Specify output path of csv resulting file. When Set to None, NFStream uses source as path and add a '.csv' extension to it. |
| `flows_per_file` | `[default=0]` | Specify maximum flows per generated file. Each generated file name will be appended by the chunk index. This limit is disabled when set to 0. |
| `columns_to_anonymize` | `[default=[]]` | List of columns names to anonymize. Anonymization is based on a random secret key generation at each start of NFStreamer. The generated key is used to anonymize configured values using blake2b algorithm. |

## NFlow

NFlow is the flow representation within NFStream. It contains all computed flow features according to NFStreamer configuration.
In the following we detail each implemented feature.

#### NFlow Core Features

| `id` | `int`  | Flow identifier |
| `expiration_id` | `int`  | Identifier of flow expiration trigger. Can be 0 for idle_timeout, 1 for active_timeout or -1 for custom expiration. |
| `src_ip` | `str`  | Source IP address string representation. |
| `src_mac` | `str`  | Source MAC address string representation. |
| `src_oui` | `str`  | Source Organizationally Unique Identifier string representation. |
| `src_port` | `int`  | Transport layer source port. |
| `dst_ip` | `str`  | Destination IP address string representation. |
| `dst_mac` | `str`  | Destination MAC address string representation. |
| `dst_oui` | `str`  | Destination Organizationally Unique Identifier string representation. |
| `dst_port` | `int`  | Transport layer destination port. |
| `protocol` | `int`  | Transport layer protocol. |
| `ip_version` | `int`  | IP version. |
| `vlan_id` | `int`  | Virtual LAN identifier. |
| `bidirectional_first_seen_ms` | `int`  | Timestamp in milliseconds on first flow bidirectional packet. |
| `bidirectional_last_seen_ms` | `int`  | Timestamp in milliseconds on last flow bidirectional packet. |
| `bidirectional_duration_ms` | `int`  | Flow bidirectional duration in milliseconds. |
| `bidirectional_packets` | `int`  | Flow bidirectional packets accumulator. |
| `bidirectional_bytes` | `int`  | Flow bidirectional bytes accumulator (depends on accounting_mode). |
| `src2dst_first_seen_ms` | `int`  | Timestamp in milliseconds on first flow src2dst packet. |
| `src2dst_last_seen_ms` | `int`  | Timestamp in milliseconds on last flow src2dst packet.  |
| `src2dst_duration_ms` | `int`  | Flow src2dst duration in milliseconds. |
| `src2dst_packets` | `int`  | Flow src2dst packets accumulator. |
| `src2dst_bytes` | `int`  | Flow src2dst bytes accumulator (depends on accounting_mode). |
| `dst2src_first_seen_ms` | `int`  | Timestamp in milliseconds on first flow dst2src packet. |
| `dst2src_last_seen_ms` | `int`  | Timestamp in milliseconds on last flow dst2src packet.  |
| `dst2src_duration_ms` | `int`  | Flow dst2src duration in milliseconds. |
| `dst2src_packets` | `int`  | Flow dst2src packets accumulator. |
| `dst2src_bytes` | `int`  | Flow dst2src bytes accumulator (depends on accounting_mode). |

#### Tunnel Decoding Features (decode_tunnels=True)

| `tunnel_id` | `int`  | Tunnel identifier (O: No Tunnel, 1: GTP, 2: CAPWAP, 3: TZSP). |

#### NFlow Layer-7 Visibility Features (n_dissections>0)

| `application_name` | `str`  | nDPI detected application name. |
| `application_category_name` | `str`  | nDPI detected application category name. |
| `application_is_guessed` | `int`  | Indicates if detection result is based on pure dissection or on a port-based guess. |
| `requested_server_name` | `str`  | Requested server name (SSL/TLS, DNS, HTTP). |
| `client_fingerprint` | `str`  | Client fingerprint (DHCP fingerprint for DHCP, [JA3][ja3] for SSL/TLS and [HASSH][hassh] for SSH). |
| `server_fingerprint` | `str`  | Server fingerprint ([JA3][ja3] for SSL/TLS and [HASSH][hassh] for SSH). |
| `user_agent` | `str`  | Extracted user agent for HTTP or User Agent Identifier for QUIC. |
| `content_type` | `str`  | Extracted HTTP content type. |

#### Post-Mortem Statistical Features (statistical_analysis=True)

| `bidirectional_min_ps` | `int`  | Flow bidirectional minimum packet size (depends on accounting_mode).|
| `bidirectional_mean_ps` | `float`  | Flow bidirectional mean packet size (depends on accounting_mode).|
| `bidirectional_stddev_ps` | `float`  | Flow bidirectional packet size sample standard deviation (depends on accounting_mode).|
| `bidirectional_max_ps` | `int`  | Flow bidirectional maximum packet size (depends on accounting_mode).|
| `src2dst_min_ps` | `int`  | Flow src2dst minimum packet size (depends on accounting_mode).|
| `src2dst_mean_ps` | `float`  | Flow src2dst mean packet size (depends on accounting_mode).|
| `src2dst_stddev_ps` | `float`  | Flow src2dst packet size sample standard deviation (depends on accounting_mode).|
| `src2dst_max_ps` | `int`  | Flow src2dst maximum packet size (depends on accounting_mode).|
| `dst2src_min_ps` | `int`  | Flow dst2src minimum packet size (depends on accounting_mode).|
| `dst2src_mean_ps` | `float`  | Flow dst2src mean packet size (depends on accounting_mode).|
| `dst2src_stddev_ps` | `float`  | Flow dst2src packet size sample standard deviation (depends on accounting_mode).|
| `dst2src_max_ps` | `int`  | Flow dst2src maximum packet size (depends on accounting_mode).|
| `bidirectional_min_piat_ms` | `int`  | Flow bidirectional minimum packet inter arrival time.|
| `bidirectional_mean_piat_ms` | `float`  | Flow bidirectional mean packet inter arrival time.|
| `bidirectional_stddev_piat_ms` | `float`  | Flow bidirectional packet inter arrival time sample standard deviation.|
| `bidirectional_max_piat_ms` | `int`  | Flow bidirectional maximum packet inter arrival time.|
| `src2dst_min_piat_ms` | `int`  | Flow src2dst minimum packet inter arrival time.|
| `src2dst_mean_piat_ms` | `float`  | Flow src2dst mean packet inter arrival time.|
| `src2dst_stddev_piat_ms` | `float`  | Flow src2dst packet inter arrival time sample standard deviation.|
| `src2dst_max_piat_ms` | `int`  | Flow src2dst maximum packet inter arrival time.|
| `dst2src_min_piat_ms` | `int`  | Flow dst2src minimum packet inter arrival time.|
| `dst2src_mean_piat_ms` | `float`  | Flow dst2src mean packet inter arrival time.|
| `dst2src_stddev_piat_ms` | `float`  | Flow dst2src packet inter arrival time sample standard deviation.|
| `dst2src_max_piat_ms` | `int`  | Flow dst2src maximum packet inter arrival time.|
| `bidirectional_syn_packets` | `int`  | Flow bidirectional syn packet accumulators.|
| `bidirectional_cwr_packets` | `int`  | Flow bidirectional cwr packet accumulators.|
| `bidirectional_ece_packets` | `int`  | Flow bidirectional ece packet accumulators.|
| `bidirectional_urg_packets` | `int`  | Flow bidirectional urg packet accumulators.|
| `bidirectional_ack_packets` | `int`  | Flow bidirectional ack packet accumulators.|
| `bidirectional_psh_packets` | `int`  | Flow bidirectional psh packet accumulators.|
| `bidirectional_rst_packets` | `int`  | Flow bidirectional rst packet accumulators.|
| `bidirectional_fin_packets` | `int`  | Flow bidirectional fin packet accumulators.|
| `src2dst_syn_packets` | `int`  | Flow src2dst syn packet accumulators.|
| `src2dst_cwr_packets` | `int`  | Flow src2dst cwr packet accumulators.|
| `src2dst_ece_packets` | `int`  | Flow src2dst ece packet accumulators.|
| `src2dst_urg_packets` | `int`  | Flow src2dst urg packet accumulators.|
| `src2dst_ack_packets` | `int`  | Flow src2dst ack packet accumulators.|
| `src2dst_psh_packets` | `int`  | Flow src2dst psh packet accumulators.|
| `src2dst_rst_packets` | `int`  | Flow src2dst rst packet accumulators.|
| `src2dst_fin_packets` | `int`  | Flow src2dst fin packet accumulators.|
| `dst2src_syn_packets` | `int`  | Flow dst2src syn packet accumulators.|
| `dst2src_cwr_packets` | `int`  | Flow dst2src cwr packet accumulators.|
| `dst2src_ece_packets` | `int`  | Flow dst2src ece packet accumulators.|
| `dst2src_urg_packets` | `int`  | Flow dst2src urg packet accumulators.|
| `dst2src_ack_packets` | `int`  | Flow dst2src ack packet accumulators.|
| `dst2src_psh_packets` | `int`  | Flow dst2src psh packet accumulators.|
| `dst2src_rst_packets` | `int`  | Flow dst2src rst packet accumulators.|
| `dst2src_fin_packets` | `int`  | Flow dst2src fin packet accumulators.|

#### Early Statistical Features (splt_analysis>0)

| `splt_direction` | `list`  | List of N (splt_analysis=N) first flow packet directions (0: src2dst, 1: dst2src, -1:no packet).|
| `splt_ps` | `list`  | List of N (splt_analysis=N) first flow packet sizes (depends on accounting_mode, -1 when there is no packet).|
| `splt_piat_ms` | `list`  | List of N (splt_analysis=N) first flow packet inter arrival times (always 0 for first packet, -1 when there is no packet).|

## NFPlugin

### NFPlugin prototype

NFPlugin is the main class for extending NFStream. It can be used for the following use cases:
* Changing the expiration logic of NFStream with custom expiration.
* Create a set of new NFlow features.
* Deploy Machine Learning Models.
* Combination of the above possibilities.

In the following, we provide the prototype of NFPlugin class that a user defined plugin must inherit from.

```python
class NFPlugin(object):
    """ NFPlugin class: Main entry point to extend NFStream """
    def __init__(self, **kwargs):
        """
        NFPlugin Parameters:
        kwargs : user defined named arguments that will be stored as Plugin attributes
        """
        for key, value in kwargs.items():
            setattr(self, key, value)

    def on_init(self, packet, flow):
        """
        on_init(self, packet, flow): Method called at flow creation.
        You must initiate your udps values if you plan to compute ones.
        Example: -------------------------------------------------------
                 flow.udps.magic_message = "NO"
                 if packet.raw_size == 40:
                    flow.udps.packet_40_count = 1
                 else:
                    flow.udps.packet_40_count = 0
        ----------------------------------------------------------------
        """

    def on_update(self, packet, flow):
        """
        on_update(self, packet, flow): Method called to update each flow 
                                       with its belonging packet.
        Example: -------------------------------------------------------
                 if packet.raw_size == 40:
                    flow.udps.packet_40_count += 1
        ----------------------------------------------------------------
        """

    def on_expire(self, flow):
        """
        on_expire(self, flow):      Method called at flow expiration.
        Example: -------------------------------------------------------
                 if flow.udps.packet_40_count >= 10:
                    flow.udps.magic_message = "YES"
        ----------------------------------------------------------------
        """
        pass

    def cleanup(self):
        """
        cleanup(self):               Method called for plugin cleanup.
        Example: -------------------------------------------------------
                 del self.large_dict_passed_as_plugin_attribute
        ----------------------------------------------------------------
        """
```

### NFPacket object

As you may noticed, NFPlugin on_init and on_update are called to initiate/update flows with its belonging set of packets.
When calling these entry points NFStream already matched the packet to its flow and preprocessed its information. Packet
information are exposed in an NFPacket (Network Flow Packet) which contains the following attributes:

| `time` | `int`  | Packet timestamp in milliseconds. |
| `delta_ime` | `int`  | Delta time in milliseconds with previous flow packet. |
| `raw_size` | `int`  | Link layer packet size. |
| `ip_size` | `int`  | IP packet size. |
| `transport_size` | `int`  | Transport packet size. |
| `payload_size` | `int`  | Packet payload size. |
| `src_ip` | `str`  | Source IP address string representation. |
| `src_mac` | `str`  | Source MAC address string representation. |
| `src_oui` | `str`  | Source Organizationally Unique Identifier string representation. |
| `dst_ip` | `str`  | Destination IP address string representation. |
| `dst_mac` | `str`  | Destination MAC address string representation. |
| `dst_oui` | `str`  | Destination Organizationally Unique Identifier string representation. |
| `src_port` | `int`  | Transport layer source port. |
| `dst_port` | `int`  | Transport layer destination port. |
| `protocol` | `int`  | Transport layer protocol. |
| `vlan_id` | `int`  | Virtual LAN identifier. |
| `ip_version` | `int`  | IP version. |
| `ip_packet` | `bytes`  | Raw content starting from IP Header. |
| `direction` | `int`  | Packet direction: 0 for src_to_dst and 1 for dst_to_src. |
| `syn` | `bool`  | TCP SYN Flag present. |
| `cwr` | `bool`  | TCP CWR Flag present. |
| `ece` | `bool`  | TCP ECE Flag present. |
| `urg` | `bool`  | TCP URG Flag present. |
| `ack` | `bool`  | TCP ACK Flag present. |
| `psh` | `bool`  | TCP PSH Flag present. |
| `rst` | `bool`  | TCP RST Flag present. |
| `fin` | `bool`  | TCP FIN Flag present. |
| `tunnel_id` | `int`  | Tunnel identifier (O: No Tunnel, 1: GTP, 2: CAPWAP, 3: TZSP). |

### NFPlugins Examples

#### Flow Slicer

In the following, we implement a flow slicer NFPlugin, which will force NFStream to expire each flow that reaches a 
packet count limit.

```python
from nfstream import NFStreamer, NFPlugin

class FlowSlicer(NFPlugin):
    def on_init(self, packet, flow):
        if self.limit == 1:
           flow.expiration_id = -1 

    def on_update(self, packet, flow):
        if self.limit == flow.bidirectional_packets:
           flow.expiration_id = -1 # -1 value force expiration


streamer = NFStreamer(source="eth0", udps=FlowSlicer(limit=7))
# Iterate, convert it to pandas or csv.
```

#### SPLT reimplementation

In the following, we reimplement splt analysis that is provided within NFStream as an NFPlugin (for demo purpose).

```python
from nfstream import NFStreamer, NFPlugin

class SPLT(NFPlugin):
    """
    Reimplementation of SPLT native analysis as NFPlugin: For demo purposes.
    SPLT: Sequence of packet length and time analyzer.
    This plugin will take 2 arguments:
        - sequence_length: determines the maximum sequence length (number of packets to analyze)
        - accounting_mode: Set how packet size will be reported (0: raw_size,
                                                                 1: ip_size,
                                                                 2: transport_size,
                                                                 3: payload_size)
    Plugin will generate 3 new metrics as follows:
    - splt_directions: Array with direction of each packet (0: src_to_dst, 1:dst_to_src)
    - splt_ps: Array with packet size in bytes according to accounting_mode value.
    - splt_ipt: Array with inter packet arrival time in milliseconds.
    Note: Tail will be set with default value -1.
    """
    @staticmethod
    def _get_packet_size(packet, accounting_mode):
        if accounting_mode == 0:
            return packet.raw_size
        elif accounting_mode == 1:
            return packet.ip_size
        elif accounting_mode == 2:
            return packet.transport_size
        else:
            return packet.payload_size

    def on_init(self, packet, flow):
        flow.udps.splt_direction = [-1] * self.sequence_length
        flow.udps.splt_direction[0] = 0  # First packet so  src->dst
        flow.udps.splt_ps = [-1] * self.sequence_length
        flow.udps.splt_ps[0] = self._get_packet_size(packet, self.accounting_mode)
        flow.udps.splt_piat_ms = [-1] * self.sequence_length
        flow.udps.splt_piat_ms[0] = packet.delta_time

    def on_update(self, packet, flow):
        if flow.bidirectional_packets <= self.sequence_length:
            packet_index = flow.bidirectional_packets - 1
            flow.udps.splt_direction[packet_index] = packet.direction
            flow.udps.splt_ps[packet_index] = self._get_packet_size(packet, self.accounting_mode)
            flow.udps.splt_piat_ms[packet_index] = packet.delta_time


streamer = NFStreamer(source="facebook.pcap", udps=SPLT(sequence_length=7, accouncting_mode=1))
for flow in streamer: # Work also with to_pandas, to_csv
   print(flow.udps.splt_direction)
```

Other examples could be found and imported in NFStream [plugins][plg] submodule.

#### Machine Learning Model: Train and Deploy

In the the following, we demonstrate a simplistic machine learning approach training and deployment.
We suppose that we want to run a classification of Social Network category flows based on bidirectional_packets and 
bidirectional_bytes as input features. For the sake of brevity, we decide to predict only at flow expiration stage.

##### Training the model

```python
from nfstream import NFPlugin, NFStreamer
from sklearn.ensemble import RandomForestClassifier

df = NFStreamer(source="training_traffic.pcap").to_pandas()
X = df[["bidirectional_packets", "bidirectional_bytes"]]
y = df["application_category_name"].apply(lambda x: 1 if 'SocialNetwork' in x else 0)
model = RandomForestClassifier()
model.fit(X, y)
```

##### ML powered streamer on live traffic

```python
import numpy

class ModelPrediction(NFPlugin):
    def on_init(self, packet, flow):
        flow.udps.model_prediction = 0
    def on_expire(self, flow):
        # You can do the same in on_update entrypoint and force expiration with custom id. 
        to_predict = numpy.array([flow.bidirectional_packets,
                                  flow.bidirectional_bytes]).reshape((1,-1))
        flow.udps.model_prediction = self.my_model.predict(to_predict)

ml_streamer = NFStreamer(source="eth0", udps=ModelPrediction(my_model=model))
for flow in ml_streamer:
    print(flow.udps.model_prediction)
```

[bpf]: https://biot.com/capstats/bpf.html
[ja3]: https://github.com/salesforce/ja3
[hassh]: https://github.com/salesforce/hassh
[plg]: https://github.com/nfstream/nfstream/tree/master/nfstream/plugins
