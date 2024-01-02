# setup ssh connection
imagine that you have a large number of servers and you want to manage them using ansible, using this role
you can setup ssh connection for any number of servers in seconds


## this role will:
+ add a new user named ansible
+ add your ssh public key to ansible user
+ grant root access for ansible user
+ disable ssh root login and restart ssh service


## how to use it:
1- copy your servers ip addresses and ssh user and password to the inventory file:

    [servers]
    123.45.678.1    ansible_user=root    ansible_password=server-password   ansible_port=22
    123.45.678.2    ansible_user=root    ansible_password=server-password   ansible_port=22
    123.45.678.3    ansible_user=root    ansible_password=server-password   ansible_port=22
    123.45.678.4    ansible_user=root    ansible_password=server-password   ansible_port=22
    123.45.678.5    ansible_user=root    ansible_password=server-password   ansible_port=22
    
2- open /files/authorized_keys file and add your ssh public key

3- done, now you can use the role, you can also modify ssh service configs in /templates/sshd_config.js file



## how to run the role:
1- outside of the setup_ssh directory, create a yml file:

    touch ssh.yml
    
so the directory structure should be like this:
    
    ├── setup_ssh
    │   ├── files
    │   │   ├── ansible_sudoer
    │   │   └── authorized_keys
    │   ├── handlers
    │   │   └── main.yml
    │   ├── inventory
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   └── templates
    │       └── sshd_config.j2
    └── ssh.yml
    
2- modify ssh.yml and disable strict host key checking and any other configs you want:

    ---
    - hosts: all
      gather_facts: no
      vars:
        ansible_ssh_common_args: '-o StrictHostKeyChecking=no -o PreferredAuthentications=password'
      roles:
        - setup_ssh

3- now run the role:

    ansible-playbook -i setup_ssh/inventory   ssh.yml

**note that after setting up ssh connection, remove your servers password from inventory file**

## tested on:
1- ubuntu

