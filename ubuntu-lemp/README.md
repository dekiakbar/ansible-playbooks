# Ubuntu LEMP

ansible playbook for configuring your server as web server, when this playbook executed it will install Nginx, Mysql and PHP. Admin user will be created during setup (when run `playbook.yml`).

## Feature
> note : server block a.k.a virtual host on apache2
 - Auto create admin user (sudoer)
 - Auto disabled ssh login using password.
 - Auto disabled ssh login for root user.
 - Auto install Nginx, Mysql, PHP
 - Auto create unique user for each web server
 - Auto create unique mysql user and database for each server block
 - Auto configure php fpm pool for unique web user
 - Auto configure domain for each server block
 - Auto add your public key every you create new server block (`you must have a pair of ssh key, default it will be located at /home/<yourusername>/.ssh/id_rsa.pub`)
 - Auto configure UFW (`default just port 22,80 and 443 will be opened`)
 - Auto install SSL (Lets encrypt)
 - Auto install Composer

## Installation

- make sure ansible is installed in your control node. read [ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) doc to install ansible.
- make sure you can connect using ssh key based (passwordless) from control node to remote host as **root** user. read [here](https://www.redhat.com/sysadmin/passwordless-ssh) if you have not configure it. test your connection to server (remote host) : `ssh root@your-server-ip-address` , if you can connect yo can start with this playbooks.
- clone this repository `git clone https://github.com/dekiakbar/ansible-playbooks`
- `cd ubuntu-lemp`
- `cp hosts.example hosts`
- edit hosts with your favourite IDE. and fill the name and the ip address of your server.
- run this command to test that ansible is configured properly : `ansible testserver -m ping -u root`. when you get this message it mean ansible is able to onnect to your server.
```
testserver | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
- before you make a new virtual host / server block you need to run initial script and make sure your server / VPS is fresh install of ubuntu (I recommend ubuntu 20.04), for initial server setup you can run : `ansible-playbook playbook.yml -l <your-server-name-defined-in-hosts> -u root`, flag `-u root` tell ansible to connect as root, this is just for setup, after you run this you can use `admin` user for create new server block or remote server through ssh (maybe).
- wait till it done.
- and enjoy :)

## FAQ
* How to change admin user ?
  * Navigate to `ubuntu-lemp/roles/harden-security/defaults/main.yml` you can configure admin username here.

* How to change mysql root password ?
  * Navigate to `ubuntu-lemp/roles/mysql/defaults/main.yml` you can fill it with your own custom password, by default ansible will store mysql root password in `/root/ansible_mysql_root.txt`, I recommend to leave it as it as, because this will be used when you run ansible to generate new server block.

* How to change PHP version ?
  * Navigate to  `ubuntu-lemp/roles/php/defaults/main.yml`, here you can change PHP version, example : 7.0 or 7.2 or 7.4

* How to set Domain name, unique user, mysql user and database for server block ?
  * Navigate to `ubuntu-lemp/roles/new-nginx-block/defaults/main.yml`, here you can set : 
    * web_user : user for server block (ssh user)
    * web_domain : somain for new server block
    * web_mysql_user : mysq user for new server block
    * web_mysql_password : mysql pass for new server block
    * web_mysql_database : database name for new server block
  
* Do I need re-run `playbook.yml` for generate new server block ?
  * No, you just need to run it once. if you want to generate new server block you can run :
    * `ansible-playbook newsite.yml -l <your-server-name-defined-in-hosts> -u <your-admin-user>`
    * **use admin user instead of root, root will be rejected by ssh because ssh login as root has been disable for security reason**
    * **before run `newsite.yml` please reconfigure the `ubuntu-lemp/roles/new-nginx-block/defaults/main.yml` configuration, if not your site will be overwritten**

## License
[GNU GPL 3.0](https://github.com/dekiakbar/ansible-playbooks/blob/master/LICENSE)