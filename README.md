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

## Example Playbook

* Use defaults vars :

``` yaml
- hosts: mynode.domain
  roles:
    - role: ipr-cnrs.netdata
      tags: ['role::netdata', 'ipr']
```

## Configuration

This role will :
* Install needed packages to provide `netdata` service.

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
