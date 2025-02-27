# DC1-POD2-LEAF1A
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
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
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
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

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.1.15/24 | 192.168.1.254 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.1.15/24
```

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
| - | TWODC_5STAGE_CLOS DC1 DC1_POD2 DC1-POD2-LEAF1A | All | Disabled |

### SNMP Device Configuration

```eos
!
snmp-server location TWODC_5STAGE_CLOS DC1 DC1_POD2 DC1-POD2-LEAF1A
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

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 110 | Tenant_A_OP_Zone_1 | - |
| 111 | Tenant_A_OP_Zone_2 | - |
| 112 | Tenant_A_OP_Zone_3 | - |
| 2500 | web-l2-vlan | - |
| 2600 | web-l2-vlan-2 | - |

## VLANs Device Configuration

```eos
!
vlan 110
   name Tenant_A_OP_Zone_1
!
vlan 111
   name Tenant_A_OP_Zone_2
!
vlan 112
   name Tenant_A_OP_Zone_3
!
vlan 2500
   name web-l2-vlan
!
vlan 2600
   name web-l2-vlan-2
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
| Ethernet1 | P2P_LINK_TO_DC1-POD2-SPINE1_Ethernet3 | routed | - | 172.17.120.1/31 | default | 1500 | false | - | - |
| Ethernet2 | P2P_LINK_TO_DC1-POD2-SPINE2_Ethernet3 | routed | - | 172.17.120.3/31 | default | 1500 | false | - | - |
| Ethernet3 | P2P_LINK_TO_DC1-RS2_Ethernet3 | routed | - | 172.17.10.12/31 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_DC1-POD2-SPINE1_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.120.1/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet2
   description P2P_LINK_TO_DC1-POD2-SPINE2_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.120.3/31
   ptp enable
   service-profile QOS-PROFILE
!
interface Ethernet3
   description P2P_LINK_TO_DC1-RS2_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.10.12/31
   service-profile QOS-PROFILE
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 172.16.120.3/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 172.18.120.3/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 172.16.120.3/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 172.18.120.3/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan110 |  Tenant_A_OP_Zone_1  |  Common_VRF  |  -  |  false  |
| Vlan111 |  Tenant_A_OP_Zone_2  |  Common_VRF  |  -  |  true  |
| Vlan112 |  Tenant_A_OP_Zone_3  |  Common_VRF  |  -  |  false  |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan110 |  Common_VRF  |  -  |  10.1.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan111 |  Common_VRF  |  -  |  10.1.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan112 |  Common_VRF  |  -  |  10.1.12.1/24  |  -  |  -  |  -  |  -  |


### VLAN Interfaces Device Configuration

```eos
!
interface Vlan110
   description Tenant_A_OP_Zone_1
   no shutdown
   vrf Common_VRF
   ip address virtual 10.1.10.1/24
!
interface Vlan111
   description Tenant_A_OP_Zone_2
   shutdown
   vrf Common_VRF
   ip address virtual 10.1.11.1/24
!
interface Vlan112
   description Tenant_A_OP_Zone_3
   no shutdown
   vrf Common_VRF
   ip address virtual 10.1.12.1/24
   comment
   Comment created from raw_eos_cli under SVI 112 in VRF Common_VRF
   EOF

```

## VXLAN Interface

### VXLAN Interface Summary

#### Source Interface: Loopback1

#### UDP port: 4789

#### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 110 | 10110 | - | - |
| 111 | 50111 | - | - |
| 112 | 50112 | - | - |
| 2500 | 2500 | - | - |
| 2600 | 2600 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| Common_VRF | 1025 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description DC1-POD2-LEAF1A_VTEP
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 111 vni 50111
   vxlan vlan 112 vni 50112
   vxlan vlan 2500 vni 2500
   vxlan vlan 2600 vni 2600
   vxlan vrf Common_VRF vni 1025
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:1c:73:00:dc:01

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:01
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| Common_VRF | true |
| MGMT | false |

### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf Common_VRF
no ip routing vrf MGMT
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| Common_VRF | false |
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
| 65121|  172.16.120.3 |

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
| Remote AS | 65120 |
| Source | Loopback0 |
| BFD | true |
| Ebgp multihop | 5 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65120 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Send-community | Maximum-routes | Allowas-in | BFD |
| -------- | --------- | --- | -------------- | -------------- | ---------- | --- |
| 172.16.120.1 | 65120 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.16.120.2 | 65120 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 172.17.10.13 | 65102 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | True |
| 172.17.120.0 | 65120 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.17.120.2 | 65120 | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 110 | 172.16.120.3:10110 | 10110:10110 | - | - | learned |
| 111 | 172.16.120.3:50111 | 50111:50111 | - | - | learned |
| 112 | 172.16.120.3:50112 | 50112:50112 | - | - | learned |
| 2500 | 172.16.120.3:2500 | 2500:2500 | - | - | learned |
| 2600 | 172.16.120.3:2600 | 2600:2600 | - | - | learned |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Common_VRF | 172.16.120.3:1025 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65121
   router-id 172.16.120.3
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS remote-as 65120
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 5
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS remote-as 65120
   neighbor IPv4-UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.16.120.1 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.120.1 remote-as 65120
   neighbor 172.16.120.1 description DC1-POD2-SPINE1
   neighbor 172.16.120.1 route-map RM-EVPN-FILTER-AS65120 out
   neighbor 172.16.120.2 peer group EVPN-OVERLAY-PEERS
   neighbor 172.16.120.2 remote-as 65120
   neighbor 172.16.120.2 description DC1-POD2-SPINE2
   neighbor 172.16.120.2 route-map RM-EVPN-FILTER-AS65120 out
   neighbor 172.17.10.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.10.13 remote-as 65102
   neighbor 172.17.10.13 description DC1-RS2_Ethernet3
   neighbor 172.17.10.13 bfd
   neighbor 172.17.120.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.120.0 remote-as 65120
   neighbor 172.17.120.0 description DC1-POD2-SPINE1_Ethernet3
   neighbor 172.17.120.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.120.2 remote-as 65120
   neighbor 172.17.120.2 description DC1-POD2-SPINE2_Ethernet3
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 110
      rd 172.16.120.3:10110
      route-target both 10110:10110
      redistribute learned
   !
   vlan 111
      rd 172.16.120.3:50111
      route-target both 50111:50111
      redistribute learned
   !
   vlan 112
      rd 172.16.120.3:50112
      route-target both 50112:50112
      redistribute learned
   !
   vlan 2500
      rd 172.16.120.3:2500
      route-target both 2500:2500
      redistribute learned
   !
   vlan 2600
      rd 172.16.120.3:2600
      route-target both 2600:2600
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family rt-membership
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
   !
   vrf Common_VRF
      rd 172.16.120.3:1025
      route-target import evpn 1025:1025
      route-target export evpn 1025:1025
      router-id 172.16.120.3
      redistribute connected
      !
      comment
      Comment created from raw_eos_cli under BGP for VRF Common_VRF
      EOF

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

## IP IGMP Snooping

### IP IGMP Snooping Summary

IGMP snooping is globally enabled.


### IP IGMP Snooping Device Configuration

```eos
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 172.16.120.0/24 eq 32 |
| 20 | permit 172.18.120.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 172.16.120.0/24 eq 32
   seq 20 permit 172.18.120.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

#### RM-EVPN-FILTER-AS65120

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | deny | match as 65120 |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-EVPN-FILTER-AS65120 deny 10
   match as 65120
!
route-map RM-EVPN-FILTER-AS65120 permit 20
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| Common_VRF | enabled |
| MGMT | disabled |

## VRF Instances Device Configuration

```eos
!
vrf instance Common_VRF
!
vrf instance MGMT
```

# Quality Of Service

# EOS CLI

```eos
!
interface Loopback1111
  description Loopback created from raw_eos_cli under platform_settings vEOS-LAB

interface Loopback1000
  description Loopback created from raw_eos_cli under VRF Common_VRF

```
