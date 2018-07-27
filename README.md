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
* **netdata__etc_src** : Directory used as source to templating /etc/netdata configuration content [default : `../templates/etc/netdata`].
* **netdata__service_name** : The service name to manage [default : `netdata`].
* **netdata__service_manage** : If the Netdata services should be managed [default : `True`].
* **netdata__conf_bind_ip** : IP address used by Netdata to listen [default : `127.0.0.1`].
* **netdata__conf_bind_port** : Port used by Netdata to listen [default : `19999`].

## Example Playbook

* Use defaults vars :

``` yaml
- hosts: mynode.DOMAIN
  roles:
    - role: ipr-cnrs.netdata
      tags: ['role::netdata', 'ipr']
```

* Use your own Netdata's configuration as source :

``` yml
- hosts: mynode.DOMAIN
  roles:
    - role: ipr-cnrs.netdata
      netdata__etc_src: '{{ inventory_dir + "/../resources/host/mynode.DOMAIN/etc/netdata/" }}'
```

  * Ensure your directory contains only templates or sub-directories, such as :

``` sh
mynode.DOMAIN
└── etc
    └── netdata
        ├── fping.conf.j2
        ├── health_alarm_notify.conf.j2
        ├── netdata.conf.j2
        └── node.d
            ├── named.conf.md.j2
            ├── README.md.j2
            ├── sma_webbox.conf.md.j2
            └── snmp.conf.md.j2
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
* Manage Netdata configuration directory (/etc/netdata).
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
