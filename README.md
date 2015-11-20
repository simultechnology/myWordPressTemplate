# Setup Vagrant and Ansible Dev Environment 

## Install ansible
```sh
$ brew update  
$ brew install ansible  
```
## Setup vagrant 
```sh
$ vagrant box list  
centos64            (virtualbox, 0)  
.....  
$ vagrant init centos64  
```
**edit Vagrantfile**  
add folloing part to the file  
```ruby
config.vm.box = "centos64"
config.vm.hostname = "web"
config.vm.network :private_network, ip: "192.168.33.77"
# fixing port is better if you use more than 2 vagrants, otherwise port number can be changed automatically. 
config.vm.network :forwarded_port, guest: 22, host: 2223
```
```sh
$ vagrant up  
```
## Setting of SSH
```sh
$ vagrant ssh-config --host web >> ~/.ssh/config  
$ chmod 600 ~/.ssh/config
```

## Setup Ansible
###Create an Inventory file
create an inventory file, whose name is "hosts"  
```sh
$ vi hosts
```
following content  
access by the name "web"  
```
[web]
web
```
### use ansible command 
```sh
# use ping module to host "web"
$ ansible web -i hosts -m ping
web | success >> {
    "changed": false,
    "ping": "pong"
}
```
### Create ansible config file
if you create config file, you don't need to specify inventory file
```sh
$ vi ansible.cfg
```
following content 
```
[defaults]
hostfile = ./hosts
```
now that you can do like following
```sh
$ ansible web -m ping
web | success >> {
    "changed": false,
    "ping": "pong"
}
```
create playbook next  
