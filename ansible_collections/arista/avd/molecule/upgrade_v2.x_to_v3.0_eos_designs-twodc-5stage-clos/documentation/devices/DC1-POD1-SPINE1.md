# DC1-POD1-SPINE1
# Table of Contents

- [Management](#management)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [SNMP](#snmp)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)
- [EOS CLI](#eos-cli)

# Management

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS |
| ---- | ----- |
| False | True |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| admin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 $6$eJ5TvI8oru5i9e8G$R1X/SbtGTk9xoEHEBQASc7SC2nHYmi.crVgp2pXuCXwxsXEA81e4E0cXgQ6kX08fIeQzauqhv2kS.RGJFCon5/
```

# Monitoring

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | TWODC_5STAGE_CLOS DC1 DC1_POD1 DC1-POD1-SPINE1 | All | Disabled |

### SNMP Device Configuration

```eos
!
snmp-server location TWODC_5STAGE_CLOS DC1 DC1_POD1 DC1-POD1-SPINE1
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **none**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_DC1-SUPER-SPINE1_Ethernet1 | routed | - | 172.16.11.1/31 | default | 1500 | false | - | - |
| Ethernet2 | P2P_LINK_TO_DC1-SUPER-SPINE2_Ethernet1 | routed | - | 172.16.11.65/31 | default | 1500 | false | - | - |
| Ethernet3 | P2P_LINK_TO_DC1-POD1-LEAF1A_Ethernet1 | routed | - | 172.17.110.0/31 | default | 1500 | false | - | - |
| Ethernet4 | P2P_LINK_TO_DC1-POD1-LEAF2A_Ethernet1 | routed | - | 172.17.110.4/31 | default | 1500 | false | - | - |
| Ethernet5 | P2P_LINK_TO_DC1-POD1-LEAF2B_Ethernet1 | routed | - | 172.17.110.8/31 | default | 1500 | false | - | - |
| Ethernet6 | P2P_LINK_TO_DC1-RS1_Ethernet2 | routed | - | 172.17.10.2/31 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_DC1-SUPER-SPINE1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.16.11.1/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet2
   description P2P_LINK_TO_DC1-SUPER-SPINE2_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.16.11.65/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet3
   description P2P_LINK_TO_DC1-POD1-LEAF1A_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.110.0/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet4
   description P2P_LINK_TO_DC1-POD1-LEAF2A_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.110.4/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet5
   description P2P_LINK_TO_DC1-POD1-LEAF2B_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.110.8/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet6
   description P2P_LINK_TO_DC1-RS1_Ethernet2
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.10.2/31
   service-profile QOS-PROFILE
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 172.16.110.1/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 172.16.110.1/32
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| MGMT | false |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| MGMT | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT  | 0.0.0.0/0 |  192.168.1.254  |  -  |  1  |  -  |  -  |  - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.1.254
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65110|  172.16.110.1 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | true |
| Ebgp multihop | 5 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Send-community | Maximum-routes | Allowas-in | BFD |
| -------- | --------- | --- | -------------- | -------------- | ---------- | --- |
| 172.16.11.0 | 65100 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.16.11.64 | 65100 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.16.20.1 | 65201 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.110.4 | 65112 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.110.5 | 65112 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.200.1 | 65200 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.210.1 | 65210 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.210.3 | 65211 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.17.10.3 | 65101 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | True |
| 172.17.110.1 | 65111 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.17.110.5 | 65112 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.17.110.9 | 65112 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP Device Configuration

```eos
!
router bgp 65110
   router-id 172.16.110.1
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 5
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.16.11.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.16.11.0 remote-as 65100
   neighbor 172.16.11.0 description DC1-SUPER-SPINE1_Ethernet1
   neighbor 172.16.11.64 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.16.11.64 remote-as 65100
   neighbor 172.16.11.64 description DC1-SUPER-SPINE2_Ethernet1
   neighbor 172.16.20.1 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.20.1 remote-as 65201
   neighbor 172.16.20.1 description DC2-RS1
   neighbor 172.16.20.1 route-map RM-EVPN-FILTER-AS65201 out
   neighbor 172.16.110.4 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.110.4 remote-as 65112
   neighbor 172.16.110.4 description DC1-POD1-LEAF2A
   neighbor 172.16.110.5 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.110.5 remote-as 65112
   neighbor 172.16.110.5 description DC1-POD1-LEAF2B
   neighbor 172.16.200.1 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.200.1 remote-as 65200
   neighbor 172.16.200.1 description DC2-SUPER-SPINE1
   neighbor 172.16.200.1 route-map RM-EVPN-FILTER-AS65200 out
   neighbor 172.16.210.1 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.210.1 remote-as 65210
   neighbor 172.16.210.1 description DC2-POD1-SPINE1
   neighbor 172.16.210.1 route-map RM-EVPN-FILTER-AS65210 out
   neighbor 172.16.210.3 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.210.3 remote-as 65211
   neighbor 172.16.210.3 description DC2-POD1-LEAF1A
   neighbor 172.16.210.3 route-map RM-EVPN-FILTER-AS65211 out
   neighbor 172.17.10.3 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.10.3 remote-as 65101
   neighbor 172.17.10.3 description DC1-RS1_Ethernet2
   neighbor 172.17.10.3 bfd
   neighbor 172.17.110.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.110.1 remote-as 65111
   neighbor 172.17.110.1 description DC1-POD1-LEAF1A_Ethernet1
   neighbor 172.17.110.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.110.5 remote-as 65112
   neighbor 172.17.110.5 description DC1-POD1-LEAF2A_Ethernet1
   neighbor 172.17.110.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.110.9 remote-as 65112
   neighbor 172.17.110.9 description DC1-POD1-LEAF2B_Ethernet1
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family rt-membership
      neighbor EVPN-OVERLAY-PEERS activate
      neighbor EVPN-OVERLAY-PEERS default-route-target only
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

# Multicast

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 172.16.110.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 172.16.110.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

#### RM-EVPN-FILTER-AS65200

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | deny | match as 65200 |

#### RM-EVPN-FILTER-AS65201

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | deny | match as 65201 |

#### RM-EVPN-FILTER-AS65210

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | deny | match as 65210 |

#### RM-EVPN-FILTER-AS65211

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | deny | match as 65211 |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-EVPN-FILTER-AS65200 deny 10
   match as 65200
!
route-map RM-EVPN-FILTER-AS65200 permit 20
!
route-map RM-EVPN-FILTER-AS65201 deny 10
   match as 65201
!
route-map RM-EVPN-FILTER-AS65201 permit 20
!
route-map RM-EVPN-FILTER-AS65210 deny 10
   match as 65210
!
route-map RM-EVPN-FILTER-AS65210 permit 20
!
route-map RM-EVPN-FILTER-AS65211 deny 10
   match as 65211
!
route-map RM-EVPN-FILTER-AS65211 permit 20
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```

# Quality Of Service

# EOS CLI

```eos
!
interface Loopback1111
  description Loopback created from raw_eos_cli under platform_settings vEOS-LAB

```
