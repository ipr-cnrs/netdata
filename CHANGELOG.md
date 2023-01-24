## v1.0.1

### Fix
* Use flatten to manage packages list.
* Prefix module with "ansible.builtin.".

## v1.0.0

### Features
* Install Netdata.
* Can choose to install recommends packages.
* Manage Netdata configuration (/etc).
* Ensure Netdata service is enabled and started.
* Manage IP address and port used.
* Manage memory mode.
* Define some vars to manage master and slaves configuration.
* Combine 4 vars to generate Netdata configuration.
* Slaves alarms can be disable, the master will not send alarms.
* Possibility to manage a registry.
* Possibility to overwrite default email recipient.
