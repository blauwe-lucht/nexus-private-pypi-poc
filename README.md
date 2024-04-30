# Nexus private pypi repo PoC

This PoC shows:

- how to build a Nexus server using Ansible,
- how to download pypi packages, scan them for security issues and upload them to Nexus,
- how to install a pypi package from Nexus on machine without internet.

## Requirements

- [VirtualBox](https://www.virtualbox.org/): tested with version 6.1.50
- [Vagrant](https://www.vagrantup.com/): tested with version 2.4.1

## Usage

### Setup

``` bash
vagrant up
```

``` bash
vagrant ssh acs
cd /vagrant
ansible-playbook playbook-nexus.yml -v
```

For some reason the Nexus pypi repo is created incorrectly. It has created a proxy repo instead of a hosted repo.
I haven't been able to figure out why, the Ansible code looks correct.
For now we'll have to fix it manually:

- Point your browser at the [Nexus Repository Manager](http://192.168.7.33:8081) and login with admin/Abcd1234!.
- Click on the cog on the top ('Server administration').
- Click on 'Repositories'.
- Click on the arrow right of 'pypi-all' and remove the repo.
- Do the same for the pypi repo.
- Click 'Create repository', select 'pypi (hosted)', name it 'pypi' and click 'Create repository'.
- Run the playbook 'playbook-nexus.yml' again to fix the privileges.

To download the packages from internet and upload them to Nexus:

``` bash
ansible-playbook playbook-upload-pypi-packages.yml -v
```

When this is successful you can see the packages [in the Nexus browser](http://192.168.7.33:8081/#browse/browse:pypi).

The package can be installed on the nointernet VM by running on the acs:

``` bash
ansible-playbook playbook-nointernet.yml -v
```

### Teardown

On the machine where this repo was cloned:

``` bash
vagrant destroy -f
```
