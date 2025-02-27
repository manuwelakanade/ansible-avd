# DC1-LEAF2B
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Name Servers](#name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SNMP](#snmp)
- [Hardware TCAM Profile](#hardware-tcam-profile)
  - [Hardware TCAM configuration](#hardware-tcam-configuration)
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
  - [Port-Channel Interfaces](#port-channel-interfaces)
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
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [Platform](#platform)
  - [Platform Summary](#platform-summary)
  - [Platform Configuration](#platform-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.200.107/24 | 192.168.200.5 |

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
   ip address 192.168.200.107/24
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 192.168.200.5 | MGMT |
| 8.8.8.8 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 192.168.200.5
```

## NTP

### NTP Summary

#### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.200.5 | MGMT | True | - | - | - | - | - | - | - |

### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT 192.168.200.5 prefer
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
| cvpadmin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username cvpadmin privilege 15 role network-admin secret sha512 $6$rZKcbIZ7iWGAWTUM$TCgDn1KcavS0s.OV8lacMTUkxTByfzcGlFlYUWroxYuU7M/9bIodhRO7nXGzMweUxvbk8mJmQl8Bh44cRktUj.
username cvpadmin ssh-key ssh-rsa AAAAB3NzaC1yc2EAA82spi2mkxp4FgaLi4CjWkpnL1A/MD7WhrSNgqXToF7QCb9Lidagy9IHafQxfu7LwkFdyQIMu8XNwDZIycuf29wHbDdz1N+YNVK8zwyNAbMOeKMqblsEm2YIorgjzQX1m9+/rJeFBKz77PSgeMp/Rc3txFVuSmFmeTy3aMkU= cvpadmin@hostmachine.local
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.200.11:9910 | MGMT | key,telarista | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.200.11:9910 -cvauth=key,telarista -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| example@example.com | DC1_FABRIC rackD DC1-LEAF2B | All | Disabled |

### SNMP Device Configuration

```eos
!
snmp-server contact example@example.com
snmp-server location DC1_FABRIC rackD DC1-LEAF2B
```

# Hardware TCAM Profile

TCAM profile __`vxlan-routing`__ is active

## Hardware TCAM configuration

```eos
!
hardware tcam
   system profile vxlan-routing
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

STP Root Super: **True**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

## Spanning Tree Device Configuration

```eos
!
spanning-tree root super
spanning-tree mode mstp
spanning-tree mst 0 priority 4096
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
| 120 | Tenant_A_WEB_Zone_1 | - |
| 121 | Tenant_A_WEBZone_2 | - |
| 130 | Tenant_A_APP_Zone_1 | - |
| 131 | Tenant_A_APP_Zone_2 | - |
| 140 | Tenant_A_DB_BZone_1 | - |
| 141 | Tenant_A_DB_Zone_2 | - |
| 160 | Tenant_A_VMOTION | - |
| 161 | Tenant_A_NFS | - |
| 210 | Tenant_B_OP_Zone_1 | - |
| 211 | Tenant_B_OP_Zone_2 | - |
| 310 | Tenant_C_OP_Zone_1 | - |
| 311 | Tenant_C_OP_Zone_2 | - |

## VLANs Device Configuration

```eos
!
vlan 110
   name Tenant_A_OP_Zone_1
!
vlan 111
   name Tenant_A_OP_Zone_2
!
vlan 120
   name Tenant_A_WEB_Zone_1
!
vlan 121
   name Tenant_A_WEBZone_2
!
vlan 130
   name Tenant_A_APP_Zone_1
!
vlan 131
   name Tenant_A_APP_Zone_2
!
vlan 140
   name Tenant_A_DB_BZone_1
!
vlan 141
   name Tenant_A_DB_Zone_2
!
vlan 160
   name Tenant_A_VMOTION
!
vlan 161
   name Tenant_A_NFS
!
vlan 210
   name Tenant_B_OP_Zone_1
!
vlan 211
   name Tenant_B_OP_Zone_2
!
vlan 310
   name Tenant_C_OP_Zone_1
!
vlan 311
   name Tenant_C_OP_Zone_2
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet7 | DC1-L2LEAF1A_Ethernet2 | *trunk | *110-111,120-121,130-131,160-161 | *- | *- | 7 |
| Ethernet8 | DC1-L2LEAF1B_Ethernet2 | *trunk | *110-111,120-121,130-131,160-161 | *- | *- | 7 |
| Ethernet9 | DC1-L2LEAF3A_Ethernet2 | *trunk | *110-111,120-121,130-131,160-161 | *- | *- | 9 |
| Ethernet10 | server01_MLAG_Eth3 | *trunk | *210-211 | *- | *- | 10 |
| Ethernet11 | server01_MTU_PROFILE_MLAG_Eth5 | *access | *110 | *- | *- | 11 |
| Ethernet12 | server01_MTU_ADAPTOR_MLAG_Eth7 | *access | *- | *- | *- | 12 |
| Ethernet20 | FIREWALL01_E1 | *trunk | *110-111,210-211 | *- | *- | 20 |
| Ethernet21 |  ROUTER01_Eth1 | access | 110 | - | - | - |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_DC1-SPINE1_Ethernet3 | routed | - | 172.31.255.33/31 | default | 1500 | false | - | - |
| Ethernet2 | P2P_LINK_TO_DC1-SPINE2_Ethernet3 | routed | - | 172.31.255.35/31 | default | 1500 | false | - | - |
| Ethernet3 | P2P_LINK_TO_DC1-SPINE3_Ethernet3 | routed | - | 172.31.255.37/31 | default | 1500 | false | - | - |
| Ethernet4 | P2P_LINK_TO_DC1-SPINE4_Ethernet3 | routed | - | 172.31.255.39/31 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_DC1-SPINE1_Ethernet3
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.33/31
!
interface Ethernet2
   description P2P_LINK_TO_DC1-SPINE2_Ethernet3
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.35/31
!
interface Ethernet3
   description P2P_LINK_TO_DC1-SPINE3_Ethernet3
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.37/31
!
interface Ethernet4
   description P2P_LINK_TO_DC1-SPINE4_Ethernet3
   no shutdown
   speed forced 100gfull
   mtu 1500
   no switchport
   ip address 172.31.255.39/31
!
interface Ethernet7
   description DC1-L2LEAF1A_Ethernet2
   no shutdown
   channel-group 7 mode active
!
interface Ethernet8
   description DC1-L2LEAF1B_Ethernet2
   no shutdown
   channel-group 7 mode active
!
interface Ethernet9
   description DC1-L2LEAF3A_Ethernet2
   no shutdown
   channel-group 9 mode active
!
interface Ethernet10
   description server01_MLAG_Eth3
   no shutdown
   channel-group 10 mode active
!
interface Ethernet11
   description server01_MTU_PROFILE_MLAG_Eth5
   no shutdown
   channel-group 11 mode active
!
interface Ethernet12
   description server01_MTU_ADAPTOR_MLAG_Eth7
   no shutdown
   channel-group 12 mode active
!
interface Ethernet20
   description FIREWALL01_E1
   no shutdown
   channel-group 20 mode active
!
interface Ethernet21
   description ROUTER01_Eth1
   no shutdown
   switchport
   switchport access vlan 110
   switchport mode access
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel7 | DC1_L2LEAF1_Po1 | switched | trunk | 110-111,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:0808:0707:0606 |
| Port-Channel9 | DC1-L2LEAF3A_Po1 | switched | trunk | 110-111,120-121,130-131,160-161 | - | - | - | - | - | 0000:0000:0606:0707:0808 |
| Port-Channel10 | server01_MLAG_PortChanne1 | switched | trunk | 210-211 | - | - | - | - | - | - |
| Port-Channel11 | server01_MTU_PROFILE_MLAG_PortChanne1 | switched | access | 110 | - | - | - | - | - | - |
| Port-Channel12 | server01_MTU_ADAPTOR_MLAG_PortChanne1 | switched | access | - | - | - | - | - | - | - |
| Port-Channel20 | FIREWALL01_PortChanne1 | switched | trunk | 110-111,210-211 | - | - | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel7
   description DC1_L2LEAF1_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-111,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:0808:0707:0606
      route-target import 08:08:07:07:06:06
   lacp system-id 0808.0707.0606
!
interface Port-Channel9
   description DC1-L2LEAF3A_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-111,120-121,130-131,160-161
   switchport mode trunk
   evpn ethernet-segment
      identifier 0000:0000:0606:0707:0808
      route-target import 06:06:07:07:08:08
   lacp system-id 0606.0707.0808
!
interface Port-Channel10
   description server01_MLAG_PortChanne1
   no shutdown
   switchport
   switchport trunk allowed vlan 210-211
   switchport mode trunk
   spanning-tree bpduguard disable
   spanning-tree bpdufilter disable
!
interface Port-Channel11
   description server01_MTU_PROFILE_MLAG_PortChanne1
   no shutdown
   mtu 1600
   switchport
   switchport access vlan 110
   spanning-tree bpduguard enable
   spanning-tree bpdufilter enable
!
interface Port-Channel12
   description server01_MTU_ADAPTOR_MLAG_PortChanne1
   no shutdown
   mtu 1601
   switchport
   spanning-tree bpduguard enable
   spanning-tree bpdufilter enable
!
interface Port-Channel20
   description FIREWALL01_PortChanne1
   no shutdown
   switchport
   switchport trunk allowed vlan 110-111,210-211
   switchport mode trunk
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 192.168.255.11/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 192.168.254.11/32 |
| Loopback100 | Tenant_A_OP_Zone_VTEP_DIAGNOSTICS | Tenant_A_OP_Zone | 10.255.1.11/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback100 | Tenant_A_OP_Zone_VTEP_DIAGNOSTICS | Tenant_A_OP_Zone | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 192.168.255.11/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 192.168.254.11/32
!
interface Loopback100
   description Tenant_A_OP_Zone_VTEP_DIAGNOSTICS
   no shutdown
   vrf Tenant_A_OP_Zone
   ip address 10.255.1.11/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan110 |  Tenant_A_OP_Zone_1  |  Tenant_A_OP_Zone  |  -  |  false  |
| Vlan111 |  Tenant_A_OP_Zone_2  |  Tenant_A_OP_Zone  |  -  |  false  |
| Vlan120 |  Tenant_A_WEB_Zone_1  |  Tenant_A_WEB_Zone  |  -  |  false  |
| Vlan121 |  Tenant_A_WEBZone_2  |  Tenant_A_WEB_Zone  |  1560  |  true  |
| Vlan130 |  Tenant_A_APP_Zone_1  |  Tenant_A_APP_Zone  |  -  |  false  |
| Vlan131 |  Tenant_A_APP_Zone_2  |  Tenant_A_APP_Zone  |  -  |  false  |
| Vlan140 |  Tenant_A_DB_BZone_1  |  Tenant_A_DB_Zone  |  -  |  false  |
| Vlan141 |  Tenant_A_DB_Zone_2  |  Tenant_A_DB_Zone  |  -  |  false  |
| Vlan210 |  Tenant_B_OP_Zone_1  |  Tenant_B_OP_Zone  |  -  |  false  |
| Vlan211 |  Tenant_B_OP_Zone_2  |  Tenant_B_OP_Zone  |  -  |  false  |
| Vlan310 |  Tenant_C_OP_Zone_1  |  Tenant_C_OP_Zone  |  -  |  false  |
| Vlan311 |  Tenant_C_OP_Zone_2  |  Tenant_C_OP_Zone  |  -  |  false  |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan110 |  Tenant_A_OP_Zone  |  -  |  10.1.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan111 |  Tenant_A_OP_Zone  |  -  |  10.1.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan120 |  Tenant_A_WEB_Zone  |  -  |  10.1.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan121 |  Tenant_A_WEB_Zone  |  -  |  10.1.10.254/24  |  -  |  -  |  -  |  -  |
| Vlan130 |  Tenant_A_APP_Zone  |  -  |  10.1.30.1/24  |  -  |  -  |  -  |  -  |
| Vlan131 |  Tenant_A_APP_Zone  |  -  |  10.1.31.1/24  |  -  |  -  |  -  |  -  |
| Vlan140 |  Tenant_A_DB_Zone  |  -  |  10.1.40.1/24  |  -  |  -  |  -  |  -  |
| Vlan141 |  Tenant_A_DB_Zone  |  -  |  10.1.41.1/24  |  -  |  -  |  -  |  -  |
| Vlan210 |  Tenant_B_OP_Zone  |  -  |  10.2.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan211 |  Tenant_B_OP_Zone  |  -  |  10.2.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan310 |  Tenant_C_OP_Zone  |  -  |  10.3.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan311 |  Tenant_C_OP_Zone  |  -  |  10.3.11.1/24  |  -  |  -  |  -  |  -  |


### VLAN Interfaces Device Configuration

```eos
!
interface Vlan110
   description Tenant_A_OP_Zone_1
   no shutdown
   vrf Tenant_A_OP_Zone
   ip address virtual 10.1.10.1/24
!
interface Vlan111
   description Tenant_A_OP_Zone_2
   no shutdown
   vrf Tenant_A_OP_Zone
   ip address virtual 10.1.11.1/24
   ip helper-address 1.1.1.1 vrf MGMT source-interface lo100
!
interface Vlan120
   description Tenant_A_WEB_Zone_1
   no shutdown
   vrf Tenant_A_WEB_Zone
   ip address virtual 10.1.20.1/24
   ip address virtual 10.2.20.1/24 secondary
   ip address virtual 10.2.21.1/24 secondary
   ip helper-address 1.1.1.1 vrf TEST source-interface lo100
!
interface Vlan121
   description Tenant_A_WEBZone_2
   shutdown
   mtu 1560
   vrf Tenant_A_WEB_Zone
   ip address virtual 10.1.10.254/24
!
interface Vlan130
   description Tenant_A_APP_Zone_1
   no shutdown
   vrf Tenant_A_APP_Zone
   ip address virtual 10.1.30.1/24
!
interface Vlan131
   description Tenant_A_APP_Zone_2
   no shutdown
   vrf Tenant_A_APP_Zone
   ip address virtual 10.1.31.1/24
!
interface Vlan140
   description Tenant_A_DB_BZone_1
   no shutdown
   vrf Tenant_A_DB_Zone
   ip address virtual 10.1.40.1/24
!
interface Vlan141
   description Tenant_A_DB_Zone_2
   no shutdown
   vrf Tenant_A_DB_Zone
   ip address virtual 10.1.41.1/24
!
interface Vlan210
   description Tenant_B_OP_Zone_1
   no shutdown
   vrf Tenant_B_OP_Zone
   ip address virtual 10.2.10.1/24
!
interface Vlan211
   description Tenant_B_OP_Zone_2
   no shutdown
   vrf Tenant_B_OP_Zone
   ip address virtual 10.2.11.1/24
!
interface Vlan310
   description Tenant_C_OP_Zone_1
   no shutdown
   vrf Tenant_C_OP_Zone
   ip address virtual 10.3.10.1/24
!
interface Vlan311
   description Tenant_C_OP_Zone_2
   no shutdown
   vrf Tenant_C_OP_Zone
   ip address virtual 10.3.11.1/24
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
| 120 | 10120 | - | - |
| 121 | 10121 | - | - |
| 130 | 10130 | - | - |
| 131 | 10131 | - | - |
| 140 | 10140 | - | - |
| 141 | 10141 | - | - |
| 160 | 10160 | - | - |
| 161 | 10161 | - | - |
| 210 | 20210 | - | - |
| 211 | 20211 | - | - |
| 310 | 30310 | - | - |
| 311 | 30311 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| Tenant_A_APP_Zone | 12 | - |
| Tenant_A_DB_Zone | 13 | - |
| Tenant_A_OP_Zone | 10 | - |
| Tenant_A_WEB_Zone | 11 | - |
| Tenant_B_OP_Zone | 20 | - |
| Tenant_C_OP_Zone | 30 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description DC1-LEAF2B_VTEP
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 111 vni 50111
   vxlan vlan 120 vni 10120
   vxlan vlan 121 vni 10121
   vxlan vlan 130 vni 10130
   vxlan vlan 131 vni 10131
   vxlan vlan 140 vni 10140
   vxlan vlan 141 vni 10141
   vxlan vlan 160 vni 10160
   vxlan vlan 161 vni 10161
   vxlan vlan 210 vni 20210
   vxlan vlan 211 vni 20211
   vxlan vlan 310 vni 30310
   vxlan vlan 311 vni 30311
   vxlan vrf Tenant_A_APP_Zone vni 12
   vxlan vrf Tenant_A_DB_Zone vni 13
   vxlan vrf Tenant_A_OP_Zone vni 10
   vxlan vrf Tenant_A_WEB_Zone vni 11
   vxlan vrf Tenant_B_OP_Zone vni 20
   vxlan vrf Tenant_C_OP_Zone vni 30
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

#### Virtual Router MAC Address: 00:dc:00:00:00:0a

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:dc:00:00:00:0a
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| MGMT | false |
| Tenant_A_APP_Zone | true |
| Tenant_A_DB_Zone | true |
| Tenant_A_OP_Zone | true |
| Tenant_A_WEB_Zone | true |
| Tenant_B_OP_Zone | true |
| Tenant_C_OP_Zone | true |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf Tenant_A_APP_Zone
ip routing vrf Tenant_A_DB_Zone
ip routing vrf Tenant_A_OP_Zone
ip routing vrf Tenant_A_WEB_Zone
ip routing vrf Tenant_B_OP_Zone
ip routing vrf Tenant_C_OP_Zone
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| MGMT | false |
| Tenant_A_APP_Zone | false |
| Tenant_A_DB_Zone | false |
| Tenant_A_OP_Zone | false |
| Tenant_A_WEB_Zone | false |
| Tenant_B_OP_Zone | false |
| Tenant_C_OP_Zone | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT  | 0.0.0.0/0 |  192.168.200.5  |  -  |  1  |  -  |  -  |  - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.200.5
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102|  192.168.255.11 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | true |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Send-community | Maximum-routes | Allowas-in | BFD |
| -------- | --------- | --- | -------------- | -------------- | ---------- | --- |
| 172.31.255.32 | 65001 | default | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - |
| 172.31.255.34 | 65001 | default | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - |
| 172.31.255.36 | 65001 | default | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - |
| 172.31.255.38 | 65001 | default | Inherited from peer group UNDERLAY-PEERS | Inherited from peer group UNDERLAY-PEERS | - | - |
| 192.168.255.1 | 65001 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 192.168.255.2 | 65001 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 192.168.255.3 | 65001 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |
| 192.168.255.4 | 65001 | default | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP VLAN Aware Bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| Tenant_A_APP_Zone | 65001:12 | 12:12 | - | - | learned | 130-131 |
| Tenant_A_DB_Zone | 65001:13 | 13:13 | - | - | learned | 140-141 |
| Tenant_A_NFS | 65001:10161 | 10161:10161 | - | - | learned | 161 |
| Tenant_A_OP_Zone | 65001:10 | 10:10 | - | - | learned | 110-111 |
| Tenant_A_VMOTION | 65001:10160 | 10160:10160 | - | - | learned | 160 |
| Tenant_A_WEB_Zone | 65001:11 | 11:11 | - | - | learned | 120-121 |
| Tenant_B_OP_Zone | 65001:20 | 20:20 | - | - | learned | 210-211 |
| Tenant_C_OP_Zone | 65001:30 | 30:30 | - | - | learned | 310-311 |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Tenant_A_APP_Zone | 65001:12 | connected |
| Tenant_A_DB_Zone | 65001:13 | connected |
| Tenant_A_OP_Zone | 65001:10 | connected |
| Tenant_A_WEB_Zone | 65001:11 | connected |
| Tenant_B_OP_Zone | 65001:20 | connected |
| Tenant_C_OP_Zone | 65001:30 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 192.168.255.11
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 q+VNViP5i4rVjW1cxFv2wA==
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor UNDERLAY-PEERS peer group
   neighbor UNDERLAY-PEERS password 7 AQQvKeimxJu+uGQ/yYvv9w==
   neighbor UNDERLAY-PEERS send-community
   neighbor UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.31.255.32 peer group UNDERLAY-PEERS
   neighbor 172.31.255.32 remote-as 65001
   neighbor 172.31.255.32 description DC1-SPINE1_Ethernet3
   neighbor 172.31.255.34 peer group UNDERLAY-PEERS
   neighbor 172.31.255.34 remote-as 65001
   neighbor 172.31.255.34 description DC1-SPINE2_Ethernet3
   neighbor 172.31.255.36 peer group UNDERLAY-PEERS
   neighbor 172.31.255.36 remote-as 65001
   neighbor 172.31.255.36 description DC1-SPINE3_Ethernet3
   neighbor 172.31.255.38 peer group UNDERLAY-PEERS
   neighbor 172.31.255.38 remote-as 65001
   neighbor 172.31.255.38 description DC1-SPINE4_Ethernet3
   neighbor 192.168.255.1 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.1 remote-as 65001
   neighbor 192.168.255.1 description DC1-SPINE1
   neighbor 192.168.255.2 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.2 remote-as 65001
   neighbor 192.168.255.2 description DC1-SPINE2
   neighbor 192.168.255.3 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.3 remote-as 65001
   neighbor 192.168.255.3 description DC1-SPINE3
   neighbor 192.168.255.4 peer group EVPN-OVERLAY-PEERS
   neighbor 192.168.255.4 remote-as 65001
   neighbor 192.168.255.4 description DC1-SPINE4
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle Tenant_A_APP_Zone
      rd 65001:12
      route-target both 12:12
      redistribute learned
      vlan 130-131
   !
   vlan-aware-bundle Tenant_A_DB_Zone
      rd 65001:13
      route-target both 13:13
      redistribute learned
      vlan 140-141
   !
   vlan-aware-bundle Tenant_A_NFS
      rd 65001:10161
      route-target both 10161:10161
      redistribute learned
      vlan 161
   !
   vlan-aware-bundle Tenant_A_OP_Zone
      rd 65001:10
      route-target both 10:10
      redistribute learned
      vlan 110-111
   !
   vlan-aware-bundle Tenant_A_VMOTION
      rd 65001:10160
      route-target both 10160:10160
      redistribute learned
      vlan 160
   !
   vlan-aware-bundle Tenant_A_WEB_Zone
      rd 65001:11
      route-target both 11:11
      redistribute learned
      vlan 120-121
   !
   vlan-aware-bundle Tenant_B_OP_Zone
      rd 65001:20
      route-target both 20:20
      redistribute learned
      vlan 210-211
   !
   vlan-aware-bundle Tenant_C_OP_Zone
      rd 65001:30
      route-target both 30:30
      redistribute learned
      vlan 310-311
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor UNDERLAY-PEERS activate
   !
   vrf Tenant_A_APP_Zone
      rd 65001:12
      route-target import evpn 12:12
      route-target export evpn 12:12
      router-id 192.168.255.11
      redistribute connected
   !
   vrf Tenant_A_DB_Zone
      rd 65001:13
      route-target import evpn 13:13
      route-target export evpn 13:13
      router-id 192.168.255.11
      redistribute connected
   !
   vrf Tenant_A_OP_Zone
      rd 65001:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 192.168.255.11
      redistribute connected
   !
   vrf Tenant_A_WEB_Zone
      rd 65001:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 192.168.255.11
      redistribute connected
   !
   vrf Tenant_B_OP_Zone
      rd 65001:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 192.168.255.11
      redistribute connected
   !
   vrf Tenant_C_OP_Zone
      rd 65001:30
      route-target import evpn 30:30
      route-target export evpn 30:30
      router-id 192.168.255.11
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

IGMP snooping is globally enabled.


| VLAN | IGMP Snooping |
| --- | --------------- |
| 120 | disabled |

### IP IGMP Snooping Device Configuration

```eos
!
no ip igmp snooping vlan 120
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 192.168.255.0/24 eq 32 |
| 20 | permit 192.168.254.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 192.168.255.0/24 eq 32
   seq 20 permit 192.168.254.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| Tenant_A_APP_Zone | enabled |
| Tenant_A_DB_Zone | enabled |
| Tenant_A_OP_Zone | enabled |
| Tenant_A_WEB_Zone | enabled |
| Tenant_B_OP_Zone | enabled |
| Tenant_C_OP_Zone | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance Tenant_A_APP_Zone
!
vrf instance Tenant_A_DB_Zone
!
vrf instance Tenant_A_OP_Zone
!
vrf instance Tenant_A_WEB_Zone
!
vrf instance Tenant_B_OP_Zone
!
vrf instance Tenant_C_OP_Zone
```

# Virtual Source NAT

## Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| Tenant_A_OP_Zone | 10.255.1.11 |

## Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf Tenant_A_OP_Zone address 10.255.1.11
```

# Platform

## Platform Summary

### Platform Sand Summary

| Settings | Value |
| -------- | ----- |
| lag.hardware_only | True |

## Platform Configuration

```eos
!
platform sand lag hardware-only
```

# Quality Of Service
