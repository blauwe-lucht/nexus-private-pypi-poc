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

### Setup

``` bash
vagrant up
```

The registered VMs can be seen in the [Systems page](https://access.redhat.com/management/systems) of your Customer Portal.

``` bash
vagrant ssh acs
cd /vagrant
ansible-playbook playbook-nexus.yml
```

To build and upload the RPM, on the acs run:

``` bash
ansible-playbook playbook-package.yml
```

When this is successful you can see the package in Nexus: point your browser at the [Nexus Repository Manager](http://192.168.14.34:8081) and
login with admin/Abcd1234!.

The package can be installed on the nexus VM by running on the acs:

``` bash
ansible-playbook playbook-install-package.yml
```

The results can be found in /opt/poc.

### Teardown

On the machine where this repo was cloned:

``` bash
vagrant destroy -f
```

The VMs will be automatically unregistered.
