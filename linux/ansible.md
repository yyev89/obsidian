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

play to copy files to target nodes:
```yaml
  - name: copy html file for site
    tags: apache,ubuntu
    ansible.builtin.copy:
      src: default_site.html
      dest: /var/www/html/index.html
      owner: root
      group: root
      mode: 0644
```

play to check for specific service running:
```yaml
  - name: ensure apache is running (ubuntu)
    tags: apache,ubuntu
    ansible.builtin.service:
      name: apache2
      state: started
    when: ansible_distribution == "Ubuntu"
```

play to change one line in config file:
```yaml
  - name: change email address for admin
    tags: apache,fedora
    ansible.builtin.lineinfile:
      path: /etc/httpd/conf/httpd.conf
      regexp: '^ServerAdmin'
      line: ServerAdmin someone@example.com
    when: ansible_distribution == "Fedora"
    register: httpd
```

play to restart a service after previous changes:
```yaml
  - name: restart httpd (fedora)
    tags: apache,fedora
    ansible.builtin.service:
      name: httpd
      state: restarted
    when: httpd.changed
```

play to add a new user:
```yaml
  - name: create user
    tags: always
    ansible.builtin.user:
      name: simone
      groups: root
```

edit /etc/sudoers.d/simone file on target node:
```
simone ALL=(ALL) NOPASSWD: ALL
```

set correct permissions:
```bash
chmod 440 simone
```

add ansible.pub key from working station to simone user on working node:
```bash
sudo su - simone
mkdir ~/.ssh
chmod 700 ~/.ssh
vim ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

add line to ansible.cfg:
```
remote_user = simone
```

plays for configuring simone user automatically:
```yaml
  - name: add sudoers file for simone
    tags: always
    ansible.builtin.copy:
      src: sudoer_simone
      dest: /etc/sudoers.d/simone
      owner: root
      group: root
      mode: 0400

  - name: add ssh key for simone user
    tags: always
    ansible.builtin.authorized_key:
      user: simone
      key: "abc123"
```

sudoer_simone file:
```
# Managed by Ansible
simone ALL=(ALL) NOPASSWD: ALL
```