# Hello world Ansible tutorial

* Examples taken from [How To Manage Remote Servers with Ansible - DigitalOcean](https://www.digitalocean.com/community/tutorial_series/how-to-manage-remote-servers-with-ansible)

* Commands on a Linux system (Ubuntu on WSL for example)

```bash
# creates inventory file
sudo mkdir /etc/ansible
sudo vi /etc/ansible/hosts

# example of ansible hosts file
# [servers]
# server1 ansible_host=192.168.xxx.xxx
#
# [all:vars]
# ansible_python_interpreter=/usr/bin/python3

# checks the inventory
ansible-inventory --list -y

# runs a connectivity test on all nodes from the default inventory (with the current username)
ansible all -m ping

# runs an ad-hoc command on all nodes from the default inventory
ansible all -a "df -h"

# runs ad-hoc commands from an Ansible module to a specific host (needs sudo privilege to install then remove a package)
ansible server1 -m apt -a "name=vim state=latest" --become --ask-become-pass
ansible server1 -m apt -a "name=vim state=absent" --become --ask-become-pass

# gather facts
ansible server1 -m setup

# creates local inventory
mkdir ansible
vi ansible/inventory

# pings all k8s controles plans (not production)
ansible k8scontrolplane:\!production -i ansible/inventory -m ping

# executes the uptime command on all servers from the specified inventory
ansible all -i ansible/inventory -a "uptime"

# executes a playbook
ansible-playbook -i ansible/inventory samples/playbooks/01-nginx.yml -l web --ask-become-pass

# you can now browse the page http://<ip>/ and see the output of the file :)
```
