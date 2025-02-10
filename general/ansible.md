
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

create default commented-out config file:
```bash
ansible-config init --disabled > ansible.cfg
```

explore current configuration:
```bash
ansible-config list
# Current values:
ansible-config dump
# Only changes from default:
ansible-config dump --only-change
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


install or remove role from ansible galaxy:
```bash
ansible-galaxy role install[remove] geerlingguy.apache
```

list installed roles:
```bash
ansible-galaxy list
```

use role in site.yml:
```yaml
- hosts: apache
  become: true
  roles:
    - geerlingguy.apache
```

create new secret vault:
```bash
ansible-vault create secret.txt
```

encrypt or decrypt an existing file:
```bash
ansible-vault encrypt[decrypt] secret.txt
```

view encrypted file without decryption:
```bash
ansible-vault view secret.txt
```

edit encrypted file:
```bash
ansible-vault edit secret.txt
```

save vault password in a file and use it:
```bash
echo "test123" > ~/.vault_key
chmod 600 ~/.vault_key
ansible-vault encrypt secret.txt --vault-password-file ~/.vault_key
```

local.yml playbook:
```yaml
---
- hosts: localhost
  connection: local
  become: true
  tasks:

  - name: update all packages
    ansible.builtin.apt:
      upgrade: dist
      update_cache: yes
    when: ansible_distribution == "Ubuntu"

  - name: install apache2
    ansible.builtin.apt:
      name: apache2
      state: latest
    when: ansible_distribution == "Ubuntu"
```

on target machine:
```bash
ansible-pull --ask-become-pass -U https://github.com/yyev89/ansible_pull.git
```

check groups in the inventory:
```bash
ansible-inventory --graph
```

show documentation for the service module:
```bash
ansible-doc service
```

validate playbooks:
```bash
ansible-playbook --syntax-check webservers-tls.yml
ansible-lint webservers-tls.yml
yamllint webservers-tls.yml
```

simple loop example with conditions and variables:
```yaml
---
- name: simple loop demo
  hosts: localhost

  tasks:
    - name: echo a value from the loop
      ansible.builtin.command: echo "{{ item }}"
      loop:
        - 1
        - 2
        - 3
        - 4
        - 5
      when: item|int > 2
      register: loopresult

    - name: print results of the loop
      ansible.builtin.debug:
        var: loopresult
```

to run set of tasks as one unit:
```yaml
...
    - name: block to handle errors
      block:
        - task 1
        - task 2
        - task 3
      rescue:
        - otherwise 1
        - otherwise 2
        - otherwise 3
      always:
        - task 1
```