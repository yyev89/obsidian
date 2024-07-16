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
