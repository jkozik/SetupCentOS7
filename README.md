# SetupCentOS7
Notes on setting up CentOS 7

# SetupAlpineVM
Notes for setting up Alpine in a VirtualBox VM

I have been using Centos 7 for my VMs.  Most of the time now, I run containers in my VMs.  Here's notes on how I setup my docker host based on Centos 7.
```
# yum update
# hostnamectl set-hostname linode2.kozik.net
# hostnamectl --pretty set-hostname "Zabbix 5.0 Server"
# vi /etc/hosts   Add "linode2.kozik.net" at the end of each line
```
Add user
```
# adduser jkozik
# passwd jkozik
# cd /root/.ssh
# mkdir -p /home/jkozik/.ssh && chmod 700 /home/jkozik/.ssh
# cp authorized_keys /home/jkozik/.ssh && chmod 600 authorized_keys
# chown -R jkozik:jkozik /home/jkozik/.ssh
# gpasswd -a jkozik wheel
```
Now verify that one can login using ssh to user jkozik.  Verify sudo works. 

Install git, docker, docker-compose.  Verify status and hello-world
```
# yum install -y yum-utils device-mapper-persistent-data lvm2
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# yum install git docker
# git --version
# systemctl start docker
# systemctl enable docker
# systemctl status docker0
# systemctl status docker
# docker run hello-world
# docker ps
# curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# chmod +x /usr/local/bin/docker-compose
# docker-compose --version
# groupadd docker
# usermod -aG docker jkozik
# chmod 666 /var/run/docker.sock
```
Login as jkozik. Verify that docker, docker-compose and git all work

```
# git --version
# docker run hello-world
# git clone https://github.com/jkozik/SetupCentOS7
# cd SetupCentOS7
# docker-compose up
```

