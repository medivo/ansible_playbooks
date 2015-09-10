ansible playbooks
===

how to use
---

### Prep

1. Create a hosts inventory file with your server hostname if you don't have one.  For this example, we've created ```~/ansible/hosts``` and the contents of the file is shown below.  The name that goes inside of the square brackets is what ansible will look for when you refer to a host by name.  The line below the square brackets is what you name your host inside of your ```~/.ssh/config``` file.

  ```bash
  [hostname_recognized_by_ansible]
  name_that_you_use_inside_of_your_ssh_config_file
  ```

2. add environment variable to your ```.bashrc``` file: ```export ANSIBLE_HOSTS=~/ansible/hosts```.  For more info on how to configure your hosts inventory, see this: https://valdhaus.co/writings/ansible-post-install/

3. copy the role subdirectory of this repo inside of your ```~/ansible``` directory.  Your ```~/ansible``` directory should now contain a ```hosts``` file and a ```roles``` subdirectory.

### using roles

1. create an ```run.yml``` file inside of your ```~/ansible``` directory.

2. A role is called inside of a playbook and the playbook is run.  As a example, we will call the *create_new_user* role inside of the ```run.yml``` playbook.  Your ```run.yml``` should look the same as below.

  ```bash
  ---
  - hosts: hostname_recognized_by_ansible
    remote_user: login_user
    sudo: true
    vars:
      default_user: login_user
      new_user: deploy
    roles:
      - create_new_user
  ```

3. The content that follows the ```hosts:``` label is the hostname you used inside of your hosts inventory file.  The ```remote_user:``` is the user you will ssh into your instance with.  ```sudo:``` is true because you need sudo privilege to create a new user.  ```vars:``` are the variables that are needed inside of the role, for this case, ```default_user:``` is the user we use to ssh in and ```new_user:``` is the name of the user we wish to create.  ```roles:``` is the role we wish to include in this play.

4. Run the ```run.yml``` playbook by typing in ```ansible-playbook run.yml``` on your command line.

5. To include multiple roles inside of your playbook, configure your ```run.yml``` to what is shown below.

  ```bash
  ---
  - hosts: hostname_recognized_by_ansible
    remote_user: login_user
    sudo: true
    vars:
      default_user: login_user
      new_user: deploy
    roles:
      - create_new_user

  - hosts: hostname_recognized_by_ansible
    remote_user: deploy
    sudo: true
    roles:
      - install_dependencies

  - hosts: hostname_recognized_by_ansible
    remote_user: deploy
    sudo: false
    roles:
      - install_ruby_rvm

  - hosts: hostname_recognized_by_ansible
    remote_user: deploy
    sudo: true
    vars:
      user: deploy
      password: password
      app_db: app_db
    roles:
      - install_postgresql

  - hosts: hostname_recognized_by_ansible
    remote_user: deploy
    sudo: true
    roles:
      - install_nginx

  - hosts: hostname_recognized_by_ansible
    remote_user: deploy
    sudo: true
    roles:
      - install_rails_dependencies
  ```

