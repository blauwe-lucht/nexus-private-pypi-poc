# Ansible Nexus yum PoC

This PoC shows:

- how to build a Nexus server using Ansible
- how to build a yum package and upload it to Nexus
- how to install that package using Ansible

## Requirements

- [VirtualBox](https://www.virtualbox.org/): tested with version 6.1.50
- [Vagrant](https://www.vagrantup.com/): tested with version 2.4.1
- A RedHat developers subscription: to get one visit the [RedHat developer site](https://developers.redhat.com/?source=sso) and click on 'Join Red Hat Developer'

## Usage

### Preparation

Set the environment variables RH_USER and RH_PASS to the credentials of your Red Hat login.

### Create the VM

``` bash
vagrant up
```

The registered VM can be seen in the [Systems page](https://access.redhat.com/management/systems) of your Customer Portal.

### Destroy the VM

``` bash
vagrant destroy -f
```

The VM will be automatically unregistered.
