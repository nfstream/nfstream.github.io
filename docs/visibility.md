---
layout: page
title: nDPI Support
permalink: /docs/visibility
nav_order: 5
---

# nDPI Support

**nfstream** deep packet inspection engine is based on **nDPI**. It allows nfstream to 
perform reliable encrypted applications identification and metadata extraction 
(e.g. TLS, QUIC, TOR, HTTP, SSH, DNS, etc.).

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Layer-7 visibility

Layer 7 visibility is enabled by default using `dissect` option and results in the following NFEntry fields.

| `master_protocol` | `int`  | nDPI master protocol identifier |
| `app_protocol` | `int`  | nDPI app protocol identifier |
| `application_name` | `str`  | nDPI application name |
| `category_name` | `str`  | nDPI application category name |
| `client_info` | `str`  | Dissected client informations. Can be `http_detected_os` for HTTP, `client_signature` for SSH or `client_requested_server_name` for SSL |
| `server_info` | `str`  | Dissected server informations. Can be `host_server_name` for HTTP or DNS, `server_signature` for SSH or `server_names` for SSL |
| `j3a_client` | `str`  | J3A client fingerprint |
| `j3a_server` | `str`  | J3A server fingerprint |


## Supported Protocols

 | 0 | Unknown | TCP | Unrated | Unspecified | 
 | 1 | FTP_CONTROL | TCP | Unsafe | Download-FileTransfer-FileSharing | 
 | 2 | POP3 | TCP | Unsafe | Email | 
 | 3 | SMTP | TCP | Acceptable | Email | 
 | 4 | IMAP | TCP | Unsafe | Email | 
 | 5 | DNS | TCP/UDP | Acceptable | Network | 
 | 6 | IPP | TCP/UDP | Acceptable | System | 
 | 7 | HTTP | TCP | Acceptable | Web | 
 | 8 | MDNS | UDP | Acceptable | Network | 
 | 9 | NTP | UDP | Acceptable | System | 
 | 10 | NetBIOS | TCP/UDP | Acceptable | System | 
 | 11 | NFS | TCP/UDP | Acceptable | DataTransfer | 
 | 12 | SSDP | UDP | Acceptable | System | 
 | 13 | BGP | TCP | Acceptable | Network | 
 | 14 | SNMP | UDP | Acceptable | Network | 
 | 15 | XDMCP | TCP/UDP | Acceptable | RemoteAccess | 
 | 16 | SMBv1 | TCP | Dangerous | System | 
 | 17 | Syslog | TCP/UDP | Acceptable | System | 
 | 18 | DHCP | UDP | Acceptable | Network | 
 | 19 | PostgreSQL | TCP | Acceptable | Database | 
 | 20 | MySQL | TCP | Acceptable | Database | 
 | 21 | Hotmail | TCP | Acceptable | Email | 
 | 22 | Direct_Download_Link | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 23 | POPS | TCP | Safe | Email | 
 | 24 | AppleJuice | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 25 | DirectConnect | TCP/UDP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 26 | ntop | TCP | Safe | Network | 
 | 27 | COAP | UDP | Safe | RPC | 
 | 28 | VMware | UDP | Acceptable | RemoteAccess | 
 | 29 | SMTPS | TCP | Safe | Email | 
 | 30 | FacebookZero | TCP | Safe | SocialNetwork | 
 | 31 | UBNTAC2 | UDP | Safe | Network | 
 | 32 | Kontiki | UDP | Potentially Dangerous | Media | 
 | 33 | OpenFT | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 34 | FastTrack | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 35 | Gnutella | TCP/UDP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 36 | eDonkey | TCP/UDP | Unsafe | Download-FileTransfer-FileSharing | 
 | 37 | BitTorrent | TCP/UDP | Unsafe | Download-FileTransfer-FileSharing | 
 | 38 | SkypeCall | TCP | Acceptable | VoIP | 
 | 39 | Signal | TCP | Fun | Chat | 
 | 40 | Memcached | TCP/UDP | Acceptable | Network | 
 | 41 | SMBv23 | TCP | Acceptable | System | 
 | 42 | Mining | UDP | Unsafe | Mining | 
 | 43 | NestLogSink | TCP | Acceptable | Cloud | 
 | 44 | Modbus | TCP | Acceptable | Network | 
 | 45 | WhatsAppCall | TCP | Acceptable | VoIP | 
 | 46 | DataSaver | TCP | Fun | Web | 
 | 47 | Xbox | UDP | Fun | Game | 
 | 48 | QQ | UDP | Fun | Chat | 
 | 49 | TikTok | TCP | Fun | SocialNetwork | 
 | 50 | RTSP | TCP/UDP | Fun | Media | 
 | 51 | IMAPS | TCP | Safe | Email | 
 | 52 | IceCast | TCP | Fun | Media | 
 | 53 | PPLive | UDP | Fun | Media | 
 | 54 | PPStream | TCP/UDP | Fun | Video | 
 | 55 | Zattoo | TCP/UDP | Fun | Video | 
 | 56 | ShoutCast | TCP | Fun | Music | 
 | 57 | Sopcast | TCP/UDP | Fun | Video | 
 | 58 | Tvants | TCP/UDP | Fun | Video | 
 | 59 | TVUplayer | TCP/UDP | Fun | Video | 
 | 60 | HTTP_Download | TCP | Acceptable | Download-FileTransfer-FileSharing | 
 | 61 | QQLive | TCP | Fun | Video | 
 | 62 | Thunder | TCP/UDP | Fun | Download-FileTransfer-FileSharing | 
 | 63 | Soulseek | TCP | Fun | Download-FileTransfer-FileSharing | 
 | 64 | PS_VUE | TCP | Acceptable | Video | 
 | 65 | IRC | TCP | Unsafe | Chat | 
 | 66 | Ayiya | UDP | Acceptable | Network | 
 | 67 | Unencrypted_Jabber | TCP/UDP | Acceptable | Web | 
 | 68 | MSN | TCP/UDP | Safe | Web | 
 | 69 | Oscar | TCP/UDP | Acceptable | Chat | 
 | 70 | Yahoo | TCP/UDP | Safe | Web | 
 | 71 | BattleField | UDP | Fun | Game | 
 | 72 | GooglePlus | TCP | Fun | SocialNetwork | 
 | 73 | VRRP | TCP | Acceptable | Network | 
 | 74 | Steam | TCP/UDP | Fun | Game | 
 | 75 | HalfLife2 | UDP | Fun | Game | 
 | 76 | WorldOfWarcraft | TCP | Fun | Game | 
 | 77 | Telnet | TCP | Unsafe | RemoteAccess | 
 | 78 | STUN | TCP/UDP | Acceptable | Network | 
 | 79 | IPsec |  | Safe | VPN | 
 | 80 | GRE |  | Acceptable | Network | 
 | 81 | ICMP |  | Acceptable | Network | 
 | 82 | IGMP |  | Acceptable | Network | 
 | 83 | EGP |  | Acceptable | Network | 
 | 84 | SCTP |  | Acceptable | Network | 
 | 85 | OSPF |  | Acceptable | Network | 
 | 86 | IP_in_IP |  | Acceptable | Network | 
 | 87 | RTP | UDP | Acceptable | Media | 
 | 88 | RDP | TCP | Acceptable | RemoteAccess | 
 | 89 | VNC | TCP | Acceptable | RemoteAccess | 
 | 90 | PcAnywhere | TCP/UDP | Acceptable | RemoteAccess | 
 | 91 | TLS | UDP | Safe | Web | 
 | 92 | SSH | TCP | Acceptable | RemoteAccess | 
 | 93 | Usenet | TCP | Acceptable | Web | 
 | 94 | MGCP | UDP | Acceptable | VoIP | 
 | 95 | IAX | UDP | Acceptable | VoIP | 
 | 96 | TFTP | UDP | Acceptable | DataTransfer | 
 | 97 | AFP | TCP | Acceptable | DataTransfer | 
 | 98 | Stealthnet | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 99 | Aimini | TCP/UDP | Fun | Download-FileTransfer-FileSharing | 
 | 100 | SIP | TCP/UDP | Acceptable | VoIP | 
 | 101 | TruPhone | TCP | Acceptable | VoIP | 
 | 102 | ICMPV6 | Acceptable | Network | 
 | 103 | DHCPV6 | UDP | Acceptable | Network | 
 | 104 | Armagetron | UDP | Fun | Game | 
 | 105 | Crossfire | TCP/UDP | Fun | RPC | 
 | 106 | Dofus | TCP | Fun | Game | 
 | 107 | Fiesta | TCP | Fun | Game | 
 | 108 | Florensia | TCP/UDP | Fun | Game | 
 | 109 | Guildwars | TCP | Fun | Game | 
 | 110 | HTTP_ActiveSync | TCP | Acceptable | Cloud | 
 | 111 | Kerberos | TCP/UDP | Acceptable | Network | 
 | 112 | LDAP | TCP/UDP | Acceptable | System | 
 | 113 | MapleStory | TCP | Fun | Game | 
 | 114 | MsSQL-TDS | TCP | Acceptable | Database | 
 | 115 | PPTP | TCP | Acceptable | VPN | 
 | 116 | Warcraft3 | TCP/UDP | Fun | Game | 
 | 117 | WorldOfKungFu | TCP | Fun | Game | 
 | 118 | Slack | TCP | Acceptable | Collaborative | 
 | 119 | Facebook | TCP | Fun | SocialNetwork | 
 | 120 | Twitter | TCP | Fun | SocialNetwork | 
 | 121 | Dropbox | UDP | Acceptable | Cloud | 
 | 122 | GMail | TCP | Acceptable | Email | 
 | 123 | GoogleMaps | TCP | Safe | Web | 
 | 124 | YouTube | TCP | Fun | Media | 
 | 125 | Skype | TCP/UDP | Acceptable | VoIP | 
 | 126 | Google | TCP | Unrated | Web | 
 | 127 | DCE_RPC | TCP | Acceptable | RPC | 
 | 128 | NetFlow | UDP | Acceptable | Network | 
 | 129 | sFlow | UDP | Acceptable | Network | 
 | 130 | HTTP_Connect | TCP | Acceptable | Web | 
 | 131 | HTTP_Proxy | TCP | Acceptable | Web | 
 | 132 | Citrix | TCP | Acceptable | Network | 
 | 133 | NetFlix | TCP | Fun | Video | 
 | 134 | LastFM | TCP | Fun | Music | 
 | 135 | Waze | TCP | Acceptable | Web | 
 | 136 | YouTubeUpload | TCP | Fun | Media | 
 | 137 | Hulu | TCP | Fun | Streaming | 
 | 138 | CHECKMK | TCP | Acceptable | DataTransfer | 
 | 139 | AJP | TCP | Acceptable | Web | 
 | 140 | Apple | TCP | Safe | Web | 
 | 141 | Webex | TCP | Acceptable | VoIP | 
 | 142 | WhatsApp | TCP | Acceptable | Chat | 
 | 143 | AppleiCloud | TCP | Acceptable | Web | 
 | 144 | Viber | UDP | Acceptable | VoIP | 
 | 145 | AppleiTunes | TCP | Fun | Streaming | 
 | 146 | Radius | UDP | Acceptable | Network | 
 | 147 | WindowsUpdate | TCP | Safe | SoftwareUpdate | 
 | 148 | TeamViewer | TCP/UDP | Acceptable | RemoteAccess | 
 | 149 | Tuenti | TCP | Acceptable | VoIP | 
 | 150 | LotusNotes | TCP | Acceptable | Collaborative | 
 | 151 | SAP | TCP | Acceptable | Network | 
 | 152 | GTP | UDP | Acceptable | Network | 
 | 153 | UPnP | UDP | Acceptable | Network | 
 | 154 | LLMNR | TCP | Acceptable | Network | 
 | 155 | RemoteScan | TCP | Potentially Dangerous | Network | 
 | 156 | Spotify | TCP/UDP | Acceptable | Music | 
 | 157 | Messenger | TCP | Acceptable | VoIP | 
 | 158 | H323 | TCP/UDP | Acceptable | VoIP | 
 | 159 | OpenVPN | TCP/UDP | Acceptable | VPN | 
 | 160 | NOE | TCP/UDP | Acceptable | VoIP | 
 | 161 | CiscoVPN | TCP/UDP | Acceptable | VPN | 
 | 162 | TeamSpeak | TCP/UDP | Acceptable | VoIP | 
 | 163 | Tor | TCP | Potentially Dangerous | VPN | 
 | 164 | CiscoSkinny | TCP | Acceptable | VoIP | 
 | 165 | RTCP | TCP/UDP | Acceptable | VoIP | 
 | 166 | RSYNC | TCP | Acceptable | DataTransfer |
 | 167 | Oracle | TCP | Acceptable | Database |
 | 168 | Corba | TCP | Acceptable | RPC |
 | 169 | UbuntuONE | TCP | Acceptable | Cloud |
 | 170 | Whois-DAS | TCP | Acceptable | Network |
 | 171 | Collectd | TCP | Acceptable | System |
 | 172 | SOCKS | TCP | Acceptable | Web |
 | 173 | Nintendo | UDP | Fun | Game |
 | 174 | RTMP | TCP | Acceptable | Media | 
 | 175 | FTP_DATA | TCP | Acceptable | Download-FileTransfer-FileSharing | 
 | 176 | Wikipedia | TCP | Safe | Web | 
 | 177 | ZeroMQ | TCP | Acceptable | RPC | 
 | 178 | Amazon | TCP | Acceptable | Web | 
 | 179 | eBay | TCP | Safe | Shopping | 
 | 180 | CNN | TCP | Safe | Web | 
 | 181 | Megaco | UDP | Acceptable | VoIP | 
 | 182 | Redis | TCP | Acceptable | Database | 
 | 183 | Pando_Media_Booster | TCP/UDP | Fun | Web | 
 | 184 | VHUA | UDP | Fun | VoIP | 
 | 185 | Telegram | TCP/UDP | Acceptable | Chat | 
 | 186 | Vevo | TCP | Fun | Music | 
 | 187 | Pandora | TCP | Fun | Streaming | 
 | 188 | QUIC | UDP | Acceptable | Web | 
 | 189 | Zoom | TCP | Acceptable | Video | 
 | 190 | EAQ | UDP | Acceptable | Network | 
 | 191 | Ookla | TCP | Safe | Network | 
 | 192 | AMQP | TCP | Acceptable | RPC | 
 | 193 | KakaoTalk | TCP | Acceptable | Chat | 
 | 194 | KakaoTalk_Voice | UDP | Acceptable | VoIP | 
 | 195 | Twitch | TCP | Fun | Video | 
 | 196 | DoH_DoT | TCP | Fun | Network | 
 | 197 | WeChat | TCP | Fun | Chat | 
 | 198 | MPEG_TS | UDP | Fun | Media | 
 | 199 | Snapchat | TCP | Fun | SocialNetwork | 
 | 200 | Sina(Weibo) | TCP | Fun | SocialNetwork | 
 | 201 | GoogleHangoutDuo | TCP/UDP | Acceptable | VoIP | 
 | 202 | IFLIX | TCP | Fun | Video | 
 | 203 | Github | TCP | Acceptable | Collaborative | 
 | 204 | BJNP | UDP | Acceptable | System | 
 | 205 | FREE_205 | TCP | Fun | VoIP | 
 | 206 | WireGuard | UDP | Acceptable | VPN | 
 | 207 | SMPP | TCP | Acceptable | Download-FileTransfer-FileSharing | 
 | 208 | DNScrypt | TCP | Safe | Network | 
 | 209 | TINC | TCP/UDP | Acceptable | VPN | 
 | 210 | Deezer | TCP | Fun | Music | 
 | 211 | Instagram | TCP | Fun | SocialNetwork | 
 | 212 | Microsoft | TCP | Safe | Cloud | 
 | 213 | Starcraft | TCP/UDP | Fun | Game | 
 | 214 | Teredo | UDP | Acceptable | Network | 
 | 215 | HotspotShield | TCP | Potentially Dangerous | VPN | 
 | 216 | IMO | UDP | Acceptable | VoIP | 
 | 217 | GoogleDrive | TCP | Acceptable | Cloud | 
 | 218 | OCS | TCP | Fun | Media | 
 | 219 | Office365 | TCP | Acceptable | Collaborative | 
 | 220 | Cloudflare | TCP | Acceptable | Web | 
 | 221 | MS_OneDrive | TCP | Acceptable | Cloud | 
 | 222 | MQTT | TCP | Acceptable | RPC | 
 | 223 | RX | UDP | Acceptable | RPC | 
 | 224 | AppleStore | TCP | Safe | SoftwareUpdate | 
 | 225 | OpenDNS | TCP | Acceptable | Web | 
 | 226 | Git | TCP | Safe | Collaborative | 
 | 227 | DRDA | TCP | Acceptable | Database | 
 | 228 | PlayStore | TCP | Safe | SoftwareUpdate | 
 | 229 | SOMEIP | TCP/UDP | Acceptable | RPC | 
 | 230 | FIX | TCP | Safe | RPC | 
 | 231 | Playstation | TCP | Fun | Game | 
 | 232 | Pastebin | TCP | Potentially Dangerous | Download-FileTransfer-FileSharing | 
 | 233 | LinkedIn | TCP | Fun | SocialNetwork | 
 | 234 | SoundCloud | TCP | Fun | Music | 
 | 235 | CSGO | UDP | Fun | Game | 
 | 236 | LISP | UDP | Acceptable | Cloud | 
 | 237 | Diameter | UDP | Acceptable | Network | 
 | 238 | ApplePush | TCP | Acceptable | Cloud | 
 | 239 | GoogleServices | TCP | Acceptable | Web | 
 | 240 | AmazonVideo | TCP/UDP | Acceptable | Cloud | 
 | 241 | GoogleDocs | TCP | Acceptable | Collaborative | 
 | 242 | WhatsAppFiles | TCP | Acceptable | Download-FileTransfer-FileSharing | 
 | 243 | Targus Dataspeed | TCP/UDP | Acceptable | Network | 
 | 244 | DNP3 | TCP | Acceptable | Network | 
 | 245 | IEC60870 | TCP | Acceptable | Network | 
 | 246 | Bloomberg | TCP | Acceptable | Network | 
 | 247 | CAPWAP | UDP | Acceptable | Network | 
 | 248 | Zabbix | TCP | Acceptable | Network
