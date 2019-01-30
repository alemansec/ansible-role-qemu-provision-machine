
# About #

QEMU amd64 debian Virtual Machines provisionning using debian preseed mechanism.


# Requirements #

- on the "ansible client" machine :
  - ansible >= 2.7.5
- on QEMU host(s) (a separate ansible role is responsible for installing those, with networking, directories and preseed.cfg files generation ; more on this topic later on) :
  - bridge-utils
  - libvirt-daemon
  - libvirt-clients
  - qemu
  - qemu-utils
  - uml-utilities
  - vde2
  - virtinst

'''Note :''' when provisionning multiple Virtual Machines, using a local apt-cacher-ng installation IS A MUST-HAVE ; otherwise, every single VM getting installed will download the same debian packages from the internet, wasting resources, time and energy ... i'll add optional apt-cacher-ng proxy support in preseed.cfg template soon.
(preseed.cfg file is generated on qemu hypervisors/hosts using a separate role i intend to publish soon beside this one)


# Example #

A usage sample is provided under ```sample_usage/```

![sample playbook execution complete](sample-complete.png?raw=true)

![sample playbook virt-manager](sample-virt-manager.png?raw=true)

The provided ```ansible.cfg``` file uses the user account ```confmgmt``` to ssh into remote qemu hosts ; either ensure you previously ```ssh-copy-id confmgmt@your_remote_qemu_host``` or edit sample_usage/ansible.cfg accordiningly.

This role depends on ```ansible-role-qemu-host``` separate ansible role (not actual dependency for now) ; i'll publish this separate role soon.
('ansible-role-qemu-host' role is responsible for installing and configuring qemu on chosen qemu hosts, it also downloads debian ISO files and generates the preseed.cfg files used here).

```
  cd sample_usage/
  # edit inventory/sample/hosts and set desired VM(s) parameters
  ansible-playbook book_provision.yml
```

# TODO #

- [ ] include provisioned-VM(s) networking configuration in this role (when first implementing this, a later part of the actual playbooks were responsible for this step) ; right now provisionned VM(s) have dynamically-allocated IP addresses.
- [ ] publish 'ansible-role-qemu-host' role.
- [ ] set tags on every single task

