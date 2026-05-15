
to work with VMWare workstation on linux download "Vagrant VMWare Utility" *.deb* file and install:
```
sudo apt install ./vagrant-vmware-utility-<version>.deb
```

add Vagrant VMWare provider:
```
vagrant plugin install vagrant-vmware-desktop
```

initialize env with (example):
```
vagrant init bento/almalinux-9 --box-version 202511.24.0
```

or create a Vagrantfile with (example):
```
Vagrant.configure("2") do |config|
  config.vm.box = "bento/almalinux-9"
  config.vm.box_version = "202511.24.0"
end
```

validate your Vagrantfile for errors:
```
vagrant validate
```

boot up the machine:
```
vagrant up
```

check the current status:
```
vagrant status [name|id]
```

connect to shell:
```
vagrant ssh [name|id]
```

shut down the machine and clean the env (if needed):
```
vagrant halt [name|id]
vagrant destroy [name|id]
```

reboot the machine if Vagrantfile was edited:
```
vagrant reload [name|id]
```

enable autocomplete:
```
vagrant autocomplete install --bash
```