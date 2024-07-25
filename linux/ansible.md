example of inventory file:
```
192.168.0.103
```

create ssh-key for ansible host machine and connect to  the target:
```bash
ssh-keygen # Name "ansible"
ssh 192.168.0.103
```

copy ssh-key from ansible host to the target:
```bash
ssh-copy-id -i ~/.ssh/ansible 192.168.0.103
```

run ssh-agent:
```bash
eval $(ssh-agent)
ssh-add ~/.ssh/ansible
```

run ping module on all ansible target hosts with key provided:
```bash
ansible all --key-file ~/.ssh/ansible -i inventory -m ping
```

ansible.cfg file:
```
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible
```

list all the nodes:
```bash
ansible all --list-hosts
```

get info about nodes:
```bash
ansible all -m gather_facts
# Limit to one node:
ansible all -m gather_facts --limit 192.168.0.103
```

update packages on target node with password prompt:
```bash
ansible all -m apt -a update-cache=true --become --ask-become-pass
```

install vim on target node and if it exists, check for latest version and update:
```bash
ansible all -m apt -a "name=vim-nox state=latest" --become --ask-become-pass
```

run dist-upgrade on target node:
```bash
ansible all -m apt -a upgrade=dist --become --ask-become-pass
```

install_apache.yml:
```yaml
---
- hosts: all
  become: true
  tasks: 

  - name: update repository index
    ansible.builtin.apt:
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

  - name: install apache2 package
    ansible.builtin.apt:
      name: apache2
      state: latest
    when: ansible_distribution == "Ubuntu"

  - name: install support for php
    ansible.builtin.apt:
      name: libapache2-mod-php
      state: latest
    when: ansible_distribution == "Ubuntu"
```

`state: absent` will remove packet from system

run a playbook:
```bash
ansible-playbook --ask-become-pass install_apache.yml
```