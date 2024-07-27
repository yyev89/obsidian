example of inventory file:
```
192.168.0.102 apache_package=apache2 php_package=libapache2-mod-php
192.168.0.103 apache_package=httpd php_package=php
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
  
  - name: install apache2 package
    ansible.builtin.package:
      name: 
        - "{{ apache_package }}"
        - "{{ php_package }}" 
      state: latest
      update_cache: yes
```

`state: absent` will remove packet from system

run a playbook:
```bash
ansible-playbook --ask-become-pass install_apache.yml
```

add groups for target servers in inventory file:
```
[web_servers]
192.168.0.102

[db_servers]
192.168.0.103
```

site.yml using targeting groups and tags:
```yaml
---
- hosts: all
  become: true
  pre_tasks:

  - name: install updates for fedora
    tags: always
    ansible.builtin.dnf:
      update_only: yes
      update_cache: yes
    when: ansible_distribution == "Fedora"
    
  - name: install updates for ubuntu
    tags: always
    ansible.builtin.apt:
      upgrade: dist
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

- hosts: web_servers
  become: true
  tasks:

  - name: install apache on web servers
    tags: apache,ubuntu
    ansible.builtin.apt:
      name:
        - apache2
        - libapache2-mod-php
    when: ansible_distribution == "Ubuntu"

- hosts: db_servers
  tags: db,fedora
  become: true
  tasks:
    - name: install mariadb package on db servers
      ansible.builtin.dnf:
        name: mariadb
        state: latest
      when: ansible_distribution == "Fedora"
```

run playbook with tags provided:
```bash
ansible-playbook --tags db --ask-become-pass site.yml
```