# router-pim-sparse-mode
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
- [Multicast](#multicast)
  - [Router PIM Sparse Mode](#router-pim-sparse-mode)
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

# Multicast

## Router PIM Sparse Mode

### IP Sparse Mode Information

### IP Rendez-vous Information

| Rendez-vous Point Address | Group Address |
| ------------------------- | ------------- |
| 10.238.1.161 | 239.12.12.12/32 |
| 10.238.1.161 | 239.12.12.13/32 |
| 10.238.1.161 | 239.12.12.14/32 |
| 10.238.1.161 | 239.12.12.16/32 |
| 10.238.1.161 | 239.12.12.20/32 |
| 10.238.1.161 | 239.12.12.21/32 |

### IP Anycast Information

| IP Anycast Address | Other Rendez-vous Point Address | Register Count |
| ------------------ | ------------------------------- | -------------- |
| 10.38.1.161 | 10.50.64.16 |  15 | |

### Router Multicast Device Configuration

```eos
!
router pim sparse-mode
   ipv4
      rp address 10.238.1.161 239.12.12.12/32
      rp address 10.238.1.161 239.12.12.13/32
      rp address 10.238.1.161 239.12.12.14/32
      rp address 10.238.1.161 239.12.12.16/32
      rp address 10.238.1.161 239.12.12.20/32
      rp address 10.238.1.161 239.12.12.21/32
      anycast-rp 10.38.1.161 10.50.64.16 register-count 15
      ssm range standard
```

# Filters

# ACL

# Quality Of Service
