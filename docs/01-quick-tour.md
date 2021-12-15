# Ansible quick tour

Examples taken from [How To Manage Remote Servers with Ansible - DigitalOcean](https://www.digitalocean.com/community/tutorial_series/how-to-manage-remote-servers-with-ansible). Commands must be ran on a linux system (Ubuntu on WSL for example).

## Inventory files

* File contents

```ini
# /etc/ansible/hosts
[servers]
server1 ansible_host=192.168.xxx.xxx

[all:vars]
ansible_python_interpreter=/usr/bin/python3

# ./inventory
[k8scontrolplane]
192.168.xxx.xxx

[web]
192.168.xxx.xxx

[development]
192.168.xxx.xxx

[production]
192.168.xxx.xxx

[k8scontrolplane:vars]
ansible_user=myusername
```

* Commands

```bash
# checks the inventory
ansible-inventory --list -y
```

## Ad-hoc

* Commands

```bash
# runs a connectivity test on all nodes from the default inventory (with the current username)
ansible all -m ping

# runs an ad-hoc command on all nodes from the default inventory
ansible all -a "df -h"

# runs ad-hoc commands from an Ansible module to a specific host (needs sudo privilege to install then remove a package)
ansible server1 -m apt -a "name=vim state=latest" --become --ask-become-pass
ansible server1 -m apt -a "name=vim state=absent" --become --ask-become-pass

# gather facts on a specific host
ansible server1 -m setup

# pings all k8s controles plans (not production) from the specified inventory
ansible k8scontrolplane:\!production -i inventory -m ping

# executes the uptime command on all servers from the specified inventory
ansible all -i inventory -a "uptime"
```

## Playbooks

* Commands

```bash
# executes the uptime command on all servers from the specified inventory
ansible all -i inventory -a "uptime"

# executes a playbook
ansible-playbook -i inventory playbooks/01-nginx.yml -l web --ask-become-pass

# you can now browse the page http://<ip>/ and see the output of the file :)

# cleans up the machine
ansible-playbook -i inventory playbooks/01-nginx-rollback.yml --ask-become-pass
```
