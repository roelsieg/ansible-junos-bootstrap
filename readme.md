# Vagrant test Junos Bootstrap

This Repository is primarily meant as a testdrive for Ansible together with a couple of virtual Juniper machines

## Getting Started

These instructions will prepare the basic setup with an example lab for a small Junos network staged with some basic config by an Ansible host.
Copy the project files form Git and make sure the host system meets the prerequisites.

### Prerequisites

* Install vagrant https://www.vagrantup.com/downloads.html
* Install Virtualbox https://www.virtualbox.org/wiki/Downloads
* Install Junos plugins on the host system

```
vagrant plugin install vagrant-junos
vagrant plugin install vagrant-host-shell
```

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
git clone https://github.com/roelsieg/ansible-junos-bootsrap.git
cd ~working_dir~/ansible-junos-bootsrap/vagrant
vagrant up
```
### System description
Machines:
* Unbuntu host - running Ansible etc.
* spine01 - a Junos machine 
* spine02 - a Junos machine
* leaf03 - a Junos machine
* leaf04 - a Junos machine

The unbuntu host has roughly the following installed:
* A Python build with all essentials to run Ansible
* Ansible - provisioning, configuration management tool
* Napalm - Network Automation and Programmability Abstraction Layer with Multivendor support

The Junos hosts used to test are : juniper/ffp-12.1X47-D15.4-packetmode (fetched automagicaly & prepared by Juniper)

### Network design
```
 Mesh Topology
      +-------------------------------------+
      |                                     |
 +---------+e3          e3+---------+       |
 | spine01 |--------------| spine02 |----+  |
 +---------+              +---------+    |  |
   |e1    \ e2          e1 /    e2|      |  | 
   |        \            /        |      |  | 
   |          \        /          |      |  | 
   |            \    /            |   +---------+
   |              \/              |   | ansible |
   |             /  \             |   +---------+
   |           /      \           |      |  | 
   |         /          \         |      |  | 
   |e1     / e2        e1 \     e2|      |  |       
 +---------+              +---------+    |  |
 | leaf03  |--------------| leaf04  |----+  |
 +---------+e3          e3+---------+       |
      |                                     |
      +-------------------------------------+
```
## Running the playbooks

Start off course with the Prerequisites and Installation. 
Then test the setup by running a first playbook:

[playbook_0.yml](./provision/playbooks.md#playbook_0)
```
vagrant ssh ansible
sudo ansible-playbook provision/playbook_0.yml
```
More info and playbooks here:
* [playbook_1.yml](./provision/playbooks.md#playbook_1) #A playbook to provision lldp to verify connections
* [playbook_2.yml](./provision/playbooks.md#playbook_1) #A playbook to provision the junos boxes with simple BGP setup

## Deployment

No clue yet howto deploy this on a live system!

## Built With

* [ipspace install.sh](from https://github.com/ipspace/NetOpsWorkshop.git) - Basic setup of the ansible host
  See also https://www.ipspace.net/Ansible_for_Networking_Engineers


## Authors

* **Roeland SIegers** - *Initial work* 


