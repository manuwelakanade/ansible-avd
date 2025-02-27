# router-bgp-v4-evpn
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
- [Authentication](#authentication)
- [Monitoring](#monitoring)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
- [Interfaces](#interfaces)
- [Routing](#routing)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BGP](#router-bgp)
- [Multicast](#multicast)
- [Filters](#filters)
- [ACL](#acl)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.73.255.122/24 | 10.73.255.2 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   vrf MGMT
   ip address 10.73.255.122/24
```

# Authentication

# Monitoring

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

**Default Allocation Policy**

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 4094 |

# Interfaces

# Routing

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |

### IP Routing Device Configuration

```eos
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65101|  192.168.255.3 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 2 ecmp 2 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Remote AS | 65001 |
| Source | Loopback0 |
| BFD | true |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### EXTENDED-COMMUNITY

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | extended |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65001 |
| Send community | all |
| Maximum routes | 12000 |

#### LARGE-COMMUNITY

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | large |

#### LOCAL-AS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Local AS | 65000 |

#### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65101 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### MULTIPLE-COMMUNITY

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | standard large |

#### NO-COMMUNITY

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |

#### STARDARD-COMMUNITY

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | standard |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Send-community | Maximum-routes | Allowas-in | BFD |
| -------- | --------- | --- | -------------- | -------------- | ---------- | --- |
| 10.255.251.1 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - |
| 172.31.255.0 | Inherited from peer group IPv4-UNDERLAY-PEERS | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 172.31.255.2 | Inherited from peer group IPv4-UNDERLAY-PEERS | default | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - |
| 192.168.255.1 | Inherited from peer group EVPN-OVERLAY-PEERS | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 192.168.255.2 | Inherited from peer group EVPN-OVERLAY-PEERS | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 10.255.251.1 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | TENANT_A_PROJECT01 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - |
| 10.2.3.4 | 1234 | TENANT_A_PROJECT01 | all | 0 (no limit) | - | - |
| 10.255.251.1 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | TENANT_A_PROJECT02 | standard | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - |
| 10.255.251.2 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | TENANT_A_PROJECT02 | extended | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - |
| 10.255.251.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | TENANT_A_PROJECT02 | large | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - |
| 10.255.251.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | TENANT_A_PROJECT02 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | True |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |
| IPv4-UNDERLAY-PEERS | False |
| MLAG-IPv4-UNDERLAY-PEER | False |

### Router BGP VLAN Aware Bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| B-ELAN-201 | 192.168.255.3:20201 | 20201:20201 | - | - | learned | 201 |
| TENANT_A_PROJECT01 | 192.168.255.3:11 | 11:11 | - | - | learned | 110 |
| TENANT_A_PROJECT02 | 192.168.255.3:12 | 12:12 | - | - | learned | 112 |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| TENANT_A_PROJECT01 | 192.168.255.3:11 | connected<br>static |
| TENANT_A_PROJECT02 | 192.168.255.3:12 | connected<br>static |

### Router BGP Device Configuration

```eos
!
router bgp 65101
   router-id 192.168.255.3
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 2 ecmp 2
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS remote-as 65001
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor EXTENDED-COMMUNITY peer group
   neighbor EXTENDED-COMMUNITY send-community extended
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS remote-as 65001
   neighbor IPv4-UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor LARGE-COMMUNITY peer group
   neighbor LARGE-COMMUNITY send-community large
   neighbor LOCAL-AS peer group
   neighbor LOCAL-AS local-as 65000 no-prepend replace-as
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65101
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 vnEaG8gMeQf3d3cN6PktXQ==
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-OUT out
   neighbor MULTIPLE-COMMUNITY peer group
   neighbor MULTIPLE-COMMUNITY send-community standard large
   neighbor NO-COMMUNITY peer group
   neighbor STARDARD-COMMUNITY peer group
   neighbor STARDARD-COMMUNITY send-community standard
   neighbor 10.255.251.1 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 172.31.255.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.31.255.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 192.168.255.1 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.2 peer group EVPN-OVERLAY-PEERS
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle B-ELAN-201
      rd 192.168.255.3:20201
      route-target both 20201:20201
      redistribute learned
      vlan 201
   !
   vlan-aware-bundle TENANT_A_PROJECT01
      rd 192.168.255.3:11
      route-target both 11:11
      redistribute learned
      vlan 110
   !
   vlan-aware-bundle TENANT_A_PROJECT02
      rd 192.168.255.3:12
      route-target both 12:12
      redistribute learned
      vlan 112
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
      no neighbor IPv4-UNDERLAY-PEERS activate
      no neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf TENANT_A_PROJECT01
      rd 192.168.255.3:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 192.168.255.3
      network 10.0.0.0/8
      network 100.64.0.0/10
      neighbor 10.2.3.4 remote-as 1234
      neighbor 10.2.3.4 local-as 123 no-prepend replace-as
      neighbor 10.2.3.4 description Tenant A BGP Peer
      neighbor 10.2.3.4 ebgp-multihop 3
      neighbor 10.2.3.4 send-community
      neighbor 10.2.3.4 maximum-routes 0
      neighbor 10.2.3.4 default-originate route-map RM-10.2.3.4-SET-NEXT-HOP-OUT always
      neighbor 10.2.3.4 route-map RM-10.2.3.4-SET-NEXT-HOP-OUT out
      neighbor 10.255.251.1 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
      redistribute static
      !
      address-family ipv4
         neighbor 10.2.3.4 activate
         network 10.0.0.0/8
         network 100.64.0.0/10 route-map RM-10.2.3.4
   !
   vrf TENANT_A_PROJECT02
      rd 192.168.255.3:12
      route-target import evpn 12:12
      route-target export evpn 12:12
      router-id 192.168.255.3
      timers bgp 5 15
      neighbor 10.255.251.1 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.251.1 description ABCDEFG
      neighbor 10.255.251.1 next-hop-self
      neighbor 10.255.251.1 timers 1 3
      neighbor 10.255.251.1 send-community standard
      neighbor 10.255.251.2 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.251.2 description ABCDEFGfg
      neighbor 10.255.251.2 timers 1 3
      neighbor 10.255.251.2 send-community extended
      neighbor 10.255.251.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.251.3 description ABCDEFGfgLCLCLCLC
      neighbor 10.255.251.3 next-hop-self
      neighbor 10.255.251.3 timers 1 3
      neighbor 10.255.251.3 send-community large
      neighbor 10.255.251.3 default-originate always
      neighbor 10.255.251.4 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.251.4 description Test_Bfd
      neighbor 10.255.251.4 bfd
      redistribute connected
      redistribute static route-map RM-CONN-2-BGP
```

# Multicast

# Filters

# ACL

# Quality Of Service
