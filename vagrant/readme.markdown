setting up vagrant box to test ansible playbooks
===

how to install vagrant
---

1. first install virtualbox: https://www.virtualbox.org/wiki/Downloads
2. then install vagrant: http://www.vagrantup.com/downloads

how to create a vagrant vm
---

1. create ```~/vagrant``` folder

2. create a Vagrantfile inside of your ~/vagrant folder.
  ```bash
  $ vagrant init
  ```

3. populate your Vagrantfile, this will create a virtual ubuntu 14.04 linux server:
  ```bash
  # -*- mode: ruby -*-
  # vi: set ft=ruby :

  VAGRANTFILE_API_VERSION = "2"

  Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network :private_network, ip: "192.168.33.10"
    config.vm.network "forwarded_port", guest: 3000, host: 3000
    config.vm.network "forwarded_port", guest: 80, host: 8080
  end
  ```

4. spin up your vagrant box:
  ```bash
  $ vagrant up
  ```

5. ssh into your vagrant box:
  ```bash
  $ vagrant ssh
  ```

6. to exit your vagrant box:
  ```bash
  # exit
  ```
7. copy your id_rsa.pub key into the ```~/.ssh/authorized_keys``` file inside of your vagrant box.  It will prompt you for a password, which is "```vagrant```".
  ```bash
  ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@192.168.33.10
  ```

8. after exiting your vagrant box, ssh into your vagrant box using the conventional method:
  ```bash
  $ ssh vagrant@192.168.33.10
  ```

yml file to run ansible roles
---

```yaml
---
- hosts: vagrant
  remote_user: vagrant
  sudo: true
  vars:
    default_user: vagrant
    new_user: deploy
  roles:
    - create_new_user

- hosts: vagrant
  remote_user: deploy
  sudo: true
  roles:
    - install_dependencies

- hosts: vagrant
  remote_user: deploy
  sudo: false
  roles:
    - install_ruby_rbenv

- hosts: vagrant
  remote_user: deploy
  sudo: true
  vars:
    user: deploy
    password: some_password
    app_db: app_development
  roles:
    - install_postgresql

- hosts: vagrant
  remote_user: deploy
  sudo: true
  roles:
    - install_nginx

- hosts: vagrant
  remote_user: deploy
  sudo: true
  roles:
    - install_rails_dependencies
```
