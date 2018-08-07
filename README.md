# Netdata

1. [Overview](#overview)
2. [Role Variables](#role-variables)
3. [Example Playbook](#example-playbook)
4. [Configuration](#configuration)
5. [Development](#development)
6. [License](#license)
7. [Author Information](#author-information)

## Overview

A role to manage Netdata installation and configuration.

## Role Variables

* **netdata__base_packages** : List of base packages in order to provide Netdata [default : `netdata`].
* **netdata__install_recommends** : If recommends packages should be install [default : `True`].
* **netdata__deploy_state** : The desired state this role should achieve. [default : `present`].
* **netdata__group_name** : Name of the directory which contains configuration files for this specific group [default : `nonexistent-host-group`].
* **netdata__default_etc_src** : Directory which contains configuration files for Netdata from this role [default : `../templates/etc/netdata`].
* **netdata__etc_src** : Directory which contains configuration files that should be managed on all hosts [default : `../templates/etc/netdata`].
* **netdata__group_etc_src** : Directory which contains configuration files for Netdata that should be managed by a specific Ansible inventory group [default : `{{ (inventory_dir | realpath + "/../resources/") + "files/by-group/" + netdata__group_name + "/etc/netdata" }}`].
* **netdata__host_etc_src** : Directory which contains configuration files for Netdata that should be managed on specific host [default : `{{ (inventory_dir | realpath + "/../resources/") + "files/by-host/" + inventory_hostname  + "/etc/netdata" }}`].
* **netdata__service_name** : The service name to manage [default : `netdata`].
* **netdata__service_manage** : If the Netdata services should be managed [default : `True`].
* **netdata__conf_bind_ip** : IP address used by Netdata to listen [default : `127.0.0.1`].
* **netdata__conf_bind_port** : Port used by Netdata to listen [default : `19999`].
* **netdata__conf_memory_mode** : The memory mode of the database [default : `ram`].
* **netdata__slave_enable** : If node should send metrics to a master [default : `False`].
* **netdata__slave_destination** : The destination to send metrics to [default : `netdata.{{ ansible_domain }}`].
* **netdata__slave_api_key** : The API KEY to use to identify with the master [default : `''`].
* **netdata__slave_buffer_size** : The buffer to use for sending metrics [default : ``].
* **netdata__slave_reconnect** : If the connection fails, or it disconnects, retry after that many seconds [default : `5`].
* **netdata__master_enable** : If node should receive metrics from other nodes [default : `False`].
* **netdata__master_api_key** : The API key to authenticate slaves [default : `''`].
* **netdata__master_history** : The number of entries in the database per hosts [default : `3600`].
* **netdata__master_memory_mode** : The memory mode to be used for all hosts using this API key [default : `ram`].
* **netdata__master_health_alarm** : Shall the master send alarms for hosts using the API key [default : `auto`].

## Example Playbook

* Use defaults vars :

``` yaml
- hosts: mynode.DOMAIN
  roles:
    - role: ipr-cnrs.netdata
      tags: ['role::netdata', 'ipr']
```

* Use your own Netdata's configuration as source :
  * Ensure you have resources directory contains only templates or sub-directories, such as :

``` sh
inventory
├── group_vars
│   ├── all
│   │   ├── ….yml
│   │   └── netdata.yml
│   └── …
resources
├── files
│   ├── by-group
│   │   └── all
│   │   │   ├── etc
│   │   │   │   ├── netdata
│   │   │   │   │   ├── health.d
│   │   │   │   │   │   └── ram.conf.j2
│   │   │   │   │   ├── plugins.d
│   │   │   │   │   │   └── python.d.plugin
│   │   │   │   │   └── python.d
│   │   │   │   │       └── fail2ban.chart.py
```

* Listen on LAN, be careful, Netdata is not designed to be exposed (see [issue 64][netdata issue 164]) :
``` yml
- hosts: mynode.DOMAIN
  roles:
    - role: ipr-cnrs.netdata
      netdata__etc_src: '{{ inventory_dir + "/../resources/host/mynode.DOMAIN/etc/netdata/" }}'
```

  * You can at least limit the access to the port **19999** to known ip addresses with your firewall [see the documentation about security][netdata wiki security],…

## Configuration

This role will :
* Install needed packages to provide `netdata` service.
* Manage Netdata configuration directory (/etc/netdata) from several sources (netdata__default_etc_src, netdata__etc_src, netdata__group_etc_src and netdata__host_etc_src).
* Ensure Netdata service is enabled and started.
* Set up some basics configuration (bind ip, port,…).

## Development

This source code comes from our [Gogs instance][netdata source] and the [Github repo][netdata github] exist just to be able to send the role to Ansible Galaxy…

But feel free to send issue/PR here :)

Thanks to this [hook][gogs to github hook], Github automatically got updates from our [Gogs instance][netdata source] :)

## License

[WTFPL][wtfpl website]

## Author Information

Jérémy Gardais
* Source : [on IPR's Gogs][netdata source]
* [IPR][ipr website] (Institut de Physique de Rennes)

[gogs to github hook]: https://stackoverflow.com/a/21998477
[netdata source]: https://git.ipr.univ-rennes1.fr/cellinfo/ansible.netdata
[netdata github]: https://github.com/ipr-cnrs/netdata
[wtfpl website]: http://www.wtfpl.net/about/
[ipr website]: https://ipr.univ-rennes1.fr/
[netdata issue 164]: https://github.com/firehol/netdata/issues/164
[netdata wiki security]: https://github.com/firehol/netdata/wiki/netdata-security#protect-netdata-from-the-internet
