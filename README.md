# openvpn-client

This role installs and copys an openvpn configuration, either from the [openvpn server](https://github.com/stuvusIT/openvpn) or from files in the inventory.


## Requirements

An openvpn config file either on the openvpn server or in the inventory and an apt based system.

## Role Variables

| Variable                           | Default                    | Description                                                                             |
| ---------------------------------- | -------------------------- | --------------------------------------------------------------------------------------- |
| `openvpn_client_name`              | `{{ inventory_hostname }}` | Name of the client; doesn't need to match the host name                                 |
| `openvpn_client_server`            | `default-server`           | Ansible hostname of the OpenVPN server                                                  |
| `openvpn_client_local_config_path` |                            | The OpenVPN client configuration to use, [read below](#client-configuration)            |
| `openvpn_client_up_commands`       | `[]`                       | List of commands thats ran as soon as the OpenVPN TAP/TUN interface goes up             |
| `openvpn_client_extra_config`      | `[]`                       | List of extra lines that are, if not already present, added to the client configuration |

### Client configuration

There are two ways to configure the OpenVPN client:

* Specifying a configuration file in `openvpn_client_local_config_path`.
  If this is done, then `openvpn_client_server` can be of any value and doesn't necessarily need to match any Ansible hostname. 
* Pulling the configuration file from the OpenVPN server.
  This is done when `openvpn_client_local_config_path` is **not** specified.
  The OpenVPN server is then required to be an Ansible host with hostname `openvpn_client_server`.
  It needs to have the configuration file, which is to be pulled, at `/etc/openvpn/{{ openvpn_client_name }}-{{ openvpn_client_server }}.ovpn`.
  If you use our [openvpn role](https://github.com/stuvusIT/openvpn) on the server and specify this client in its `openvpn_clients` variable,
  then the configuration will automatically be placed correctly.

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
