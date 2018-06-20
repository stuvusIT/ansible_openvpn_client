# openvpn-client

This role installs and copys an openvpn configuration, either from the [openvpn server](https://github.com/stuvusIT/openvpn) or from files in the inventory.


## Requirements

An openvpn config file either on the openvpn server or in the inventory and an apt based system.

## Role Variables

| Variable                          | Default / Mandatory        | Description                                                                                                                       |
|-----------------------------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `openvpn_client_server`           | :heavy_multiplication_x:   | Inventory name of the host running the [openvpn role](https://github.com/stuvusIT/openvpn)                                        |
| `openvpn_client_name`             | `{{ inventory_hostname }}` | Client name used in on the openvpn server                                                                                         |
| `openvpn_client_config_file_name` | `openvpn.ovpn`             | File name to copy over, only used when `openvpn_client_server` is not set. Looks for the file in `files/{{ inventory_hostname }}/ |

## Example Playbook

```yml
---
- hosts: all
  become: true
  vars:
    openvpn_client_server: openvpn02
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information
 * [Fritz Otlinghaus (Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
