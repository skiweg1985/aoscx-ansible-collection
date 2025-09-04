# module: aoscx_interface

Interface module for Ansible.

Version added: 4.0.0

 - [Synopsis](#Synpsis)
 - [Parameters](#Parameters)
 - [Examples](#Examples)

## Synopsis

This module manages the interface attributes of Aruba AOSCX network devices.

## Parameters

| Parameter         | Type | Choices/Defaults                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Required | Comments                                                                                                                   |
|:------------------|:-----|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------:|:---------------------------------------------------------------------------------------------------------------------------|
| `name`            | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [x]      | Name of the interface. Should be in the format chassis/slot/port e.g. 1/2/3.                                               |
| `enabled`         | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Administrative state of the interface. Use true to administratively enable it.                                             |
| `description`     | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Description of the interface.                                                                                              |
| `configure_speed` | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Option to configure speed/duplex in the interface. If `true`, `autoneg` is required.                                       |
| `autoneg`         | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the auto-negotiation state of the interface. If `false` both `speeds`, and `duplex` are required.                |
| `mtu`             | int  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the MTU value in bytes in the range 46-9198.                                                                     |
| `duplex`          | str  | [`full`, `half`]                                                                                                                                                                                                                                                                                                                                                                                                                                                           | [ ]      | Configure the interface for full duplex or half duplex. If `autoneg` is `on` this must be omitted.                         |
| `speeds`          | list |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Configure the speeds of the interface in megabits per second. If `duplex` is defined only one speed may be specified.      |
| `acl_name`        | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of the ACL being applied or removed from the VLAN.                                                                    |
| `acl_type`        | str  | [`mac`, `ipv4`, `ipv6`]                                                                                                                                                                                                                                                                                                                                                                                                                                                    | [ ]      | Type of ACL being applied or removed from the VLAN.                                                                        |
| `acl_direction`   | str  | [`in`, `out`, `routed-in`, `routed-out`]                                                                                                                                                                                                                                                                                                                                                                                                                                   | [ ]      | Direction for which the ACL is to be applied or removed.                                                                    |
| `qos`             | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of existing QoS configuration to apply to the interface.                                                              |
| `no_qos`          | bool |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Flag to remove the existing Qos of the interface. Use True to remove it.                                                   |
| `queue_profile`   | str  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [ ]      | Name of queue profile to apply to interface.                                                                               |
| `qos_trust_mode`  | str  | [`cos`, `dscp`, `name`, `global`]                                                                                                                                                                                                                                                                                                                                                                                                                                          | [ ]      | Specifies the interface QoS Trust Mode. 'global' configures the interface to use the global configuration instead.         |
| `state`           | str  | [`create`, `delete`, `update`]/`create`                                                                                                                                                                                                                                                                                                                                                                                                                                    | [ ]      | The action to be taken with the current Interface.                                                                         |
| `vsx_sync`        | list | [`acl`, `irdp`, `qos`, `rate_limits`, `vlan`, `vsx_virtual`, `virtual_gw_l3_src_mac_enable`, `policy`, `threshold_profile`, `macsec_policy`, `mka_policy`, `portfilter`, `client_ip_track_configuration`, `mgmd_acl`, `mgmd_enable`, `mgmd_robustness`, `mgmd_querier_max_response_time`, `mgmd_mld_version`, `mgmd_querier_interval`, `mgmd_last_member_query_interval`, `mgmd_querier_enable`, `mgmd_mld_static_groups`, `mgmd_igmp_static_groups`, `mgmd_igmp_version`] | [ ]      | Controls which attributes should be synchonized between VSX peers.                                                         |
| `enforce_vlan_trunks`            | bool  | false                                                 | [ ]      | If true, the list of VLAN trunks is enforced exactly as provided. VLANs not in the list will be removed. If false (default), VLANs are merged/removed incrementally depending on `state`.                |
| `port_access_onboarding_precedence` | dict | Keys: `"1"`, `"2"`; Values: `aaa`, `device-profile` | [ ]      | Defines onboarding precedence per port. Use numeric string keys for order (e.g. `"1": "aaa"`, `"2": "device-profile"`). Only the values `aaa` and `device-profile` are allowed. In running configuration this renders as a single line: `port-access onboarding precedence <first> <second>` (order as provided). |
| `mac_auth`                       | dict  |                                                     | [ ]      | Configure Port-Access **MAC Authentication** subresource on the interface. Only provided keys are applied. The subresource is created automatically if missing.                                         |
| `dot1x`                          | dict  |                                                     | [ ]      | Configure Port-Access **802.1X** subresource on the interface. Only provided keys are applied. The subresource is created automatically if missing.                                                     |
 
### Port-Access authentication suboptions

The following keys are supported inside the `mac_auth` and `dot1x` dictionaries (keys not provided are left unchanged):

| Key                           | Type | Notes |
|:------------------------------|:-----|:------|
| `auth_enable`                 | bool | Enable/disable authentication for the method. |
| `cached_reauth_enable`        | bool | Enable cached reauthentication. |
| `cached_reauth_period`        | int  | Period (seconds) for cached reauth. |
| `canned_eap_success_enable`   | bool | Allow canned EAP success. |
| `discovery_period`            | int  | Discovery period (seconds). |
| `eapol_timeout`               | int  | Timeout (seconds) for EAPOL; `null` to leave unchanged. |
| `initial_auth_response_timeout` | int | Initial auth response timeout (seconds); `null` to leave unchanged. |
| `macsec_enable`               | bool | Enable MACsec. |
| `max_requests`                | int  | Maximum requests. |
| `max_retries`                 | int  | Maximum retries. |
| `mka_cak_length`              | str  | `"16"` or `"32"`. |
| `quiet_period`                | int  | Quiet period (seconds). |
| `radius_server_group`         | str  | RADIUS server group name (optional). |
| `reauth_enable`               | bool | Enable periodic reauthentication. |
| `reauth_period`               | int  | Reauthentication period (seconds). |

> **Note:** `authentication_method` is implicitly set by the subresource (`mac-auth` vs `dot1x`) and does not need to be specified.


## Examples

### Configure MAC-Auth on an interface

Before Device Configuration:
```
interface 1/1/10
    no shutdown
    vlan access 200
```
Playbook:
```YAML
- name: Configure MAC-Auth on interface 1/1/10
  aoscx_interface:
    name: 1/1/10
    mac_auth:
      auth_enable: true
      cached_reauth_enable: true
      cached_reauth_period: 30
      canned_eap_success_enable: false
      discovery_period: 30
      max_requests: 5
      max_retries: 2
      mka_cak_length: "32"
      quiet_period: 60
      reauth_enable: true
      reauth_period: 3600
      # radius_server_group: "RADIUS-GRP1"   # optional
```
After Device Configuration:
```
interface 1/1/10
    no shutdown
    vlan access 200
    port-access mac-auth enable
    !
    # (other mac-auth timers/parameters as configured)
```

### Configure 802.1X on an interface

Before Device Configuration:
```
interface 1/1/11
    no shutdown
    vlan access 200
```
Playbook:
```YAML
- name: Configure 802.1X on interface 1/1/11
  aoscx_interface:
    name: 1/1/11
    dot1x:
      auth_enable: true
      cached_reauth_enable: false
      cached_reauth_period: 30
      canned_eap_success_enable: false
      discovery_period: 30
      max_requests: 5
      max_retries: 2
      mka_cak_length: "32"
      quiet_period: 60
      reauth_enable: false
      reauth_period: 3600
      # radius_server_group: "RADIUS-GRP1"   # optional
```
After Device Configuration:
```
interface 1/1/11
    no shutdown
    vlan access 200
    port-access dot1x enable
    !
    # (other dot1x timers/parameters as configured)
```

### Enforce VLAN trunks (hard sync)

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan trunk allowed 10,20,40
    vlan trunk native 200
```
Playbook:
```YAML
- name: Enforce exact VLAN trunks on 1/1/3 (only 200,300)
  aoscx_interface:
    name: 1/1/3
    # assuming this interface is set to trunk mode elsewhere
    enforce_vlan_trunks: true
    # when true, only the VLANs you provide remain allowed on the trunk
    # (this module enforces the desired list on the device)
    # NOTE: Provide the target trunk list via your L2 settings/tasks.
```
After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan trunk allowed 200,300
    vlan trunk native 200
```

### Set Port-Access onboarding precedence

Before Device Configuration:
```
interface 1/1/12
    no shutdown
    vlan access 200
```
Playbook:
```YAML
- name: Set onboarding precedence (AAA before device-profile)
  aoscx_interface:
    name: 1/1/12
    port_access_onboarding_precedence:
      "1": "aaa"
      "2": "device-profile"
```
After Device Configuration:
```
interface 1/1/12
    no shutdown
    vlan access 200
    port-access onboarding precedence aaa device-profile
```

### Configure speed/duplex for an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - full duplex, speed of 1000 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: full
    speeds:
      - 1000
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 1000-full
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - half duplex, speed of 10 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: half
    speeds:
      - 10
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 10-half
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - advertise only 100 Mbps and 1000 Mbps speeds
    and duplex auto-negotiaton.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
    speeds:
      - 100
      - 1000
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m 1g
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - speeds and duplex auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 100-full
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - speeds and duplex auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m
    vlan access 1
```

### Delete configuration of speed/duplex for an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 1000-full
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete full duplex, speed of 1000 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: full
    speeds:
      - 1000
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed 10-half
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete half duplex, speed of 10 Mbps and no
    auto-negotiation.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: off
    duplex: half
    speeds:
      - 10
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    speed auto 100m 1g
    vlan access 1
```

Playbook:
```YAML
- name: >
    Configure Interface 1/1/2 - delete 100 Mbps and 1000 Mbps speeds and
    duplex auto-negotiaton.
  aoscx_interface:
    name: 1/1/2
    configure_speed: true
    autoneg: on
    speeds:
      - 100
      - 1000
    state: delete
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

### Apply ipv4 ACL IN to an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Apply ipv4 ACL to interfaces (new method)
  aoscx_interface:
    name: "1/1/2"
    acl_name: ipv4_acl
    acl_type: ipv4
    acl_direction: in
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    apply access-list ip ipv4_acl in
```

### Administratively disable an interface

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Administratively disable interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    enabled: false
```

After Device Configuration:
```
interface 1/1/2
    shutdown
    vlan access 1
```

### Configure a QoS trust mode

It is possible to set an specific trust mode for a particular interface, or to
configure an interface to use the global default trust mode of the device.

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set a QoS trust mode for interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    qos_trust_mode: cos
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    qos trust cos
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
    qos trust cos
```

Playbook:
```YAML
- name: Set interface 1/1/3 to use global trust mode
  aoscx_interface:
    name: 1/1/3
    qos_trust_mode: global
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Configure a Queue Profile trust mode

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set a Queue Profile for interface 1/1/2
  aoscx_interface:
    name: 1/1/2
    queue_profile: STRICT-PROFILE
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set interface 1/1/3 to use global Queue Profile
  aoscx_interface:
    name: 1/1/3
    use_global_queue_profile: true
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Associate QoS Schedule Profiles to an interface

To assign a Schedule Profile to an interface, you have to specify the name, to
remove it simply use the `no_qos` option.

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Schedule Profile on an interface
  aoscx_interface:
    name: 1/1/2
    qos: STRICT-PROFILE
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
    apply qos schedule-profile STRICT-PROFILE
        !actual schedule-profile factory-default
```

Before Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
    apply qos schedule-profile STRICT-PROFILE
        !actual schedule-profile factory-default
```

Playbook:
```YAML
- name: Remove a Schedule Profile from an interface
  aoscx_interface:
    name: 1/1/3
    no_qos: true
```

After Device Configuration:
```
interface 1/1/3
    no shutdown
    vlan access 1
```

### Set QoS rate for an interface

Before Device Configuration:
```
interface 1/1/17
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Set the QoS rate to the 1/1/17 Interface
  aoscx_interface:
    name: 1/1/17
    qos_rate:
      broadcast: 200pps
      unknown_unicast: 100kbps
      multicast: 200pps
```

After Device Configuration:
```
interface 1/1/17
    no shutdown
    vlan access 1
    rate-limit unknown-unicast 100 kbps
    rate_limit broadcast 200 pps
    rate-limit multicast 200 pps
```

### Enable vsx-sync for interface 1/1/2

Before Device Configuration:
```
interface 1/1/2
    no shutdown
    vlan access 1
```

Playbook:
```YAML
- name: Configure Interface 1/1/2 - enable vsx-sync features
  aoscx_interface:
    name: 1/1/2
    vsx_sync:
      - acl
      - irdp
      - qos
      - rate_limits
      - vlan
      - vsx_virtual
```

After Device Configuration:
```
interface 1/1/2
    no shutdown
    vsx-sync access-lists irdp qos rate-limits vlans
    vlan access 1
```
