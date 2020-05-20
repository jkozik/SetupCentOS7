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
```
Login as jkozik




    6  hostnamectl set-hostname "Zabbix 5.0 Server"
```
# apk update
# apk add vim
# vi /etc/apk/repositories # uncomment all of the repositories
# apk update
# adduser jkozik
# apk add sudo
# visudo    # 
```
At this point, alpine is setup just enough that you can login over ssh to user jkozik. Login with ssh to jkozik@192.168.100.176 using password, and setup rsa key.
```
$ mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
$ vi ~/.ssh/authorized_keys    # cut and paste a contents of a id_rsa.pub public key file into here
```
At this point, you can ssh into the alpine linux instance with ssh key, no password.  To me this is now ready to be customized.  I want this VM to run docker, so I log back in and continue setting up the image.
```
$ sudo apk add docker docker-compose
$ sudo rc-update add docker boot
$ sudo service docker start
$ sudo service docker status    # verify docker is running
$ sudo docker run hello-world   # this should work

# I need to setup group permissions so I don't need sudo for each docker command per
# http://web.ist.utl.pt/joao.leao.guerreiro/post/alpinedocker/

$ sudo visudo   #uncomment %wheel ALL=(ALL) ALL
$ sudo vi /etc/group  # wheel:x:10:root,jkozik,  and docker:x:102:jkozik
$ docker run hello-world  # this should now work I think you need to logout and log back in to verify.
```
At this point, let's pull in a known working docker repository and test run it.
```
$ sudo apk add git
$ git clone https://github.com/jkozik/InstallNw.com
$ cd InstallNw.com
$ docker build -t jkozik/nw.com .
$ docker run -dit --name nw.com-app -p 8082:80  jkozik/nw.com
$ docker exec -it nw.com-app /bin/bash
```

Side note:  VB lets you share files from the host OS.  Here's the raw steps I followed:
# apk add virtualbox-guest-additions virtualbox-guest-modules-virt
# mkdir /mount
# mkdir /mount/weather
# mkdir /mount/wjr
# mount -t vboxsf weather /mount/weather
# mount -t vboxsf wjr     /mount/wjr
# vi /etc/fstab    #add "weather /mount/weather  vboxsf  defaults 0 0"
                   #add "wjr     /mount/wjr      vboxsf  defaults 0 0"
# vi /etc/modules  #add "vboxfs"



