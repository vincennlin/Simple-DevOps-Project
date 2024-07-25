1. Install Ansible on AWS EC2
````shell
 sudo su -
 useradd ansadmin
 passwd ansadmin
````
password: anspassw0rd
```sh
visudo
```
```sh
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
ansadmin ALL=(ALL)       ALL
```
```sh
vi /etc/ssh/sshd_config
```
```sh
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
#PermitEmptyPasswords no
#PasswordAuthentication no
```
```sh
service sshd reload
```
```sh
ssh-keygen
```
```sh
sudo su -
sudo amazon-linux-extras install ansible2
```

2. Manage DockerHost with Ansible
2-1. Switch to Docker Server
```sh
sudo su -
useradd ansadmin
passwd ansadmin
visudo
```
```sh
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL
ansadmin ALL=(ALL)       NOPASSWD: ALL
```

2-2. Query the DockerHost ip address using
```sh
ifconfig
```
172.31.12.38

2-3. Switch to Ansible Server
Paste DockerHost ip address in Host file
```sh
vi /etc/ansible/hosts
```

2-4. Copy the public key from Ansible Server to DockerHost
```sh
sudo su - ansadmin
cd .ssh
ssh-copy-id 172.31.12.38
```

2-5 Test the connection in Ansible Server
```sh
ansible all -m ping
```
```sh
ansible all -m command -a uptime
```