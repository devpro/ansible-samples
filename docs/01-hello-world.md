# Hello world Ansible tutorial

* Commands on a Linux system (Ubuntu on WSL for example)

```bash
# creates inventory file
sudo mkdir /etc/ansible
sudo vi /etc/ansible/hosts

# checks the inventory
ansible-inventory --list -y

# runs a connectivity test on all nodes from the default inventory (with the current username)
ansible all -m ping

# runs an ad-hoc command on all nodes from the default inventory
ansible all -a "df -h"

# runs ad-hoc commands from an Ansible module to a specific host (needs sudo privilege to install then remove a package)
ansible server1 -m apt -a "name=vim state=latest" --become --ask-become-pass
ansible server1 -m apt -a "name=vim state=absent" --become --ask-become-pass
```
