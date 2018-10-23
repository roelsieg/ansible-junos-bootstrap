# Ansible playbooks

## playbook_0

[A test drive playbook reading out the version of the junos boxes](https://github.com/roelsieg/ansible-junos-bootstrap/blob/master/provision/playbook_0.yml)

## playbook_1

[A test drive playbook to provision lldp to verify connections](https://github.com/roelsieg/ansible-junos-bootstrap/blob/master/provision/playbook_1.yml)
```
vagrant ssh ansible
sudo ansible-playbook ./provision/playbook_1.yml
```
## playbook_2

* [A playbook to provision the junos boxes with simple BGP setup](https://github.com/roelsieg/ansible-junos-bootstrap/blob/master/provision/playbook_2/playbook_2.yml)
* [A group_vas file holding the IP information used](https://github.com/roelsieg/ansible-junos-bootstrap/blob/master/provision/playbook_2/group_vars)
* [Several host_var file to define values in YAML files](https://github.com/roelsieg/ansible-junos-bootstrap/tree/master/provision/playbook_2/host_vars)
* [One template to provision all 4 boxes](https://github.com/roelsieg/ansible-junos-bootstrap/blob/master/provision/playbook_2/template.j2)
```
vagrant ssh ansible
sudo ansible-playbook ./provision/playbook_2/playbook_2.yml
```
Verify connectivity by some basic commands:
```
vagrant ssh spine01
show lldp neighbor
show interfaces terse | match inet
show bgp summary
show route receive-protocol bgp 172.16.0.x
show route advertising-protocol bgp 172.16.0.x
```
## playbook rollback
```
sudo ansible-playbook ./provision/rollback/pb_rollback.yml --extra-var rbid=1
```