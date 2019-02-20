# openvpn-client

This role installs and copys an openvpn configuration, either from the [openvpn server](https://github.com/stuvusIT/openvpn) or from files in the inventory.


## Requirements

An openvpn config file either on the OpenVPN server or in the inventory and an *apt* based system.

## Role Variables

| Variable                              | Default                    | Description                                    |
| ------------------------------------- | -------------------------- | ---------------------------------------------- |
| `openvpn_client_default_client_name`  | `{{ inventory_hostname }}` | Default for `client_name` in client instances  |
| `openvpn_client_default_server_name`  | *not defined*              | Default for `client_name` in client instances  |
| `openvpn_client_default_up_commands`  | `[]`                       | Default for `up_commands` in client instances  |
| `openvpn_client_default_extra_config` | `[]`                       | Default for `extra_config` in client instances |
| `openvpn_client_instances`            | *not defined*              | List of [client instances](#client-instances)  |

### Client instances

The `openvpn_client_instances` variable shall be a list of *client instance objects*, each
representing a separate OpenVPN client connection.
A *client instance object* is a dictionary which can contain the following keys.

| Keys                | Default                                    | Description                                                       |
| ------------------- | ------------------------------------------ | ----------------------------------------------------------------- |
| `client_name`       | `{{ openvpn_client_default_client_name }}` | Name of the client; doesn't need to match the host name           |
| `server_name`       | `{{ openvpn_client_default_server_name }}` | Ansible hostname of the OpenVPN server                            |
| `local_config_path` | *not defined*                              | Optional local [client configuration](#client-configuration) file |
| `up_commands`       | `{{ openvpn_client_default_up_commands }}` | Commands ran when the OpenVPN TAP/TUN interface goes up           |
| `extra_config`      | `{{ openvpn_client_instances }}`           | Extra lines added to the client configuration                     |

Note:
The lines in `extra_config` are only added if not already present.

### Client configuration

There are two ways to configure the OpenVPN client:

* Specify a configuration file in `local_config_path`.
  That path has to be relative to `files/{{ inventory_hostname }}/`.
  If this is done, then `server_name` can be of any value and doesn't necessarily need to match any
  Ansible hostname.
* Automatically pull the configuration file from the OpenVPN server.
  This is done when `local_config_path` is **not** defined.
  The OpenVPN server is then required to be an Ansible host with hostname `server_name`.
  It needs to have the configuration file, which is to be pulled, at
  `/etc/openvpn/{{ client_name }}-{{ server_name }}.ovpn`.
  If you use our [openvpn role](https://github.com/stuvusIT/openvpn) on the server and specify this
  client in its `openvpn_clients` variable,
  then the configuration file will automatically be placed correctly.

If you use the second option, then it's oftentimes enaugh to specify the `server_name` in the
*client instance object*.

## Example Playbook

For the second [client configuration](#client-configuration) option it's oftentimes enaugh to specify
the `server_name` in the *client instance object*.
Therefore a valid playbook can look like this:

```yml
---
- hosts: all
  become: true
  vars:
    openvpn_client_instances:
      - server_name: openvpn02
```

This assumes that `openvpn02` is an Ansible host playing the [openvpn role](https://github.com/stuvusIT/openvpn) which has this client in its `openvpn_clients` variable.

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information
 * [Fritz Otlinghaus (Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
