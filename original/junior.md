# Junior Level

## 1. Create a memo list

Unix time start at 00:00:00, Thursday, 1 January 1970. Create a memo list for every single day of 1970.

- Create memo list on **desktop.example.com**.
- The directory of memo list is **/home/Memo_List**, create if it doesn't existing.
- There should be only one empty ASCII text file for one day. Use command "cal 1970" to get calendar of 1970.
- File name format: %Y-%m-%d (e.g., 1970-01-01).

```bash
[root@desktop ~]# ssh root@172.25.0.10
[root@desktop ~]# mkdir /home/Memo_List
[root@desktop ~]# touch /home/Memo_List/1970-{01,03,05,07,08,10,12}-{01..31} /home/Memo_List/1970-{04,06,09,11}-{01..30} /home/Memo_List/1970-02-{01..28}
[root@desktop ~]# ls /home/Memo_List/ | wc -l
365
```

## 2. Report file path

October is a important month for wkshi. Please report **all** Octobgagsd sd gasdf asdf er memo list file path as a ASCII text file.

- Report on **desktop.example.com**.
- Report in **/home/report.txt**, create if it doesn't existing.
- It's a ASCII text file.
- Content of report is October memo list file's **absolute path**. Sort as **dictionary order**.

```bash
[root@desktop ~]# ls /home/Memo_List/*-10-* | sort -d > /home/report.txt
[root@desktop ~]# grep -c -e "Memo_List" -e "-10-" /home/report.txt
31
```

## 3. Create local Account

Linux is a multi-user, multi-tasking operating system. Several user usually share one host. Please create local account for them.

- Account you created should on **desktop.example.com**.
- Create group "straw-hat" with GID 1500.
- Create group "pirate-empress" with GID 1600.
- Create required account:

|Username|Password|Fullname|UID|Supplementary Groups|Shell|
|-|-|-|-|-|-|
|luffy|onepiece|Monkey D. Luffy|1501|straw-hat|/bin/bash|
|zoro|onepiece|Roronoa Zoro|1502|straw-hat|/bin/bash|
|robin|onepiece|Nico Robin|1503|straw-hat|/sbin/nologin|
|hancock|onepiece|Boa Hancock|1504|pirate-empress|/bin/bash|

- Force **all** users to change their password on first login.

```bash
[root@desktop ~]# groupadd -g 1500 straw-hat
[root@desktop ~]# groupadd -g 1600 pirate-empress
[root@desktop ~]# useradd -u 1501 -c "Monkey D. Luffy" -G straw-hat luffy
[root@desktop ~]# useradd -u 1502 -c "Roronoa Zoro" -G straw-hat zoro
[root@desktop ~]# useradd -u 1503 -c "Nico Robin" -G straw-hat -s /sbin/nologin robin
[root@desktop ~]# useradd -u 1504 -c "Boa Hancock" -G pirate-empress hancock
[root@desktop ~]# echo onepiece | passwd --stdin luffy
[root@desktop ~]# echo onepiece | passwd --stdin zoro
[root@desktop ~]# echo onepiece | passwd --stdin robin
[root@desktop ~]# echo onepiece | passwd --stdin hancock
[root@desktop ~]# chage -d 0 luffy
[root@desktop ~]# chage -d 0 zoro
[root@desktop ~]# chage -d 0 robin
[root@desktop ~]# chage -d 0 hancock
[root@desktop ~]# id luffy
uid=1501(luffy) gid=1501(luffy) groups=1501(luffy),1500(straw-hat)
[root@desktop ~]# id zoro
uid=1502(zoro) gid=1502(zoro) groups=1502(zoro),1500(straw-hat)
[root@desktop ~]# id robin
uid=1503(robin) gid=1503(robin) groups=1503(robin),1500(straw-hat)
[root@desktop ~]# id hancock
uid=1504(hancock) gid=1504(hancock) groups=1504(hancock),1600(pirate-empress)
[root@desktop ~]# groupmems -g straw-hat -l
luffy  zoro  robin
[root@desktop ~]# groupmems -g pirate-empress -l
hancock
```
## 4. Create Share directory

People must work together in modern world. Please create shared directory for several users.

- Create directories on **desktop.example.com**.
- Create directories as listed tree-like format:

```
/home/straw-hat/
├── luffy
├── zoro
└── share
/home/pirate-empress/
```

- Configure permission as required:

|Directory|Ownership Description|Permission Description|
|-|-|-|-|-|
|/home/straw-hat/|straw-hat group|straw-hat group, hancock: read, others: no permission|
|/home/straw-hat/luffy/|luffy|luffy: all permission, hancock: read, others: no permission|
|/home/straw-hat/zoro/|zoro|zoro: all permission, others: no permission|
|/home/straw-hat/share/|straw-hat group|straw-hat group: all permission, all file created must belong to straw-hat group, nobody could delete file created by others,others: no permission|
|/home/pirate-empress/|hancock|hancock: all permission, others: no permission|

```bash
[root@desktop ~]# mkdir -p /home/straw-hat/{luffy,zoro,share} /home/pirate-empress
[root@desktop ~]# ls -R /home/straw-hat/
/home/straw-hat/:
luffy  share  zoro

/home/straw-hat/luffy:

/home/straw-hat/share:

/home/straw-hat/zoro:
[root@desktop ~]# ls -R /home/pirate-empress/
/home/pirate-empress/:

[root@desktop ~]# chgrp -R straw-hat /home/straw-hat/
[root@desktop ~]# chmod 750 /home/straw-hat/
[root@desktop ~]# setfacl -m u:hancock:rx /home/straw-hat/
[root@desktop ~]# getfacl /home/straw-hat/
getfacl: Removing leading '/' from absolute path names
# file: home/straw-hat/
# owner: root
# group: straw-hat
user::rwx
user:hancock:r-x
group::r-x
mask::r-x
other::---

[root@desktop ~]# chown luffy:luffy /home/straw-hat/luffy/
[root@desktop ~]# chmod 770 /home/straw-hat/luffy/
[root@desktop ~]# setfacl -m u:hancock:rx /home/straw-hat/luffy/
[root@desktop ~]# getfacl /home/straw-hat/luffy/
getfacl: Removing leading '/' from absolute path names
# file: home/straw-hat/luffy/
# owner: luffy
# group: luffy
user::rwx
user:hancock:r-x
group::rwx
mask::rwx
other::---

[root@desktop ~]# chown zoro:zoro /home/straw-hat/zoro/
[root@desktop ~]# chmod 770 /home/straw-hat/zoro/
[root@desktop ~]# getfacl /home/straw-hat/zoro/
getfacl: Removing leading '/' from absolute path names
# file: home/straw-hat/zoro/
# owner: zoro
# group: zoro
user::rwx
group::rwx
other::---

[root@desktop ~]# chgrp straw-hat /home/straw-hat/share/
[root@desktop ~]# chmod 770 /home/straw-hat/share/
[root@desktop ~]# chmod g+s,o+t /home/straw-hat/share/
[root@desktop ~]# getfacl /home/straw-hat/share/
getfacl: Removing leading '/' from absolute path names
# file: home/straw-hat/share/
# owner: root
# group: straw-hat
# flags: -st
user::rwx
group::rwx
other::---

[root@desktop ~]# chown hancock:hancock /home/pirate-empress/
[root@desktop ~]# chmod 770 /home/pirate-empress/
[root@desktop ~]# getfacl /home/pirate-empress/
getfacl: Removing leading '/' from absolute path names
# file: home/pirate-empress/
# owner: hancock
# group: hancock
user::rwx
group::rwx
other::---

```

## 5. Configure openssh service

One of the best part of Linux is that you could control remote host by command line. Please Configure openssh service.

- Generate public/private rsa key pair for **luffy** on **desktop.example.com**.

|Identification|Public Key|Passphrase|
|-|-|-|
|/home/luffy/.ssh/id_rsa|/home/luffy/.ssh/id_rsa.pub|-|

- Make sure luffy could login to **server.example.com** as **root** without password.
- Disable sshd password authentication on **server.example.com**.

```bash
[root@desktop ~]# su - luffy
[luffy@desktop ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/luffy/.ssh/id_rsa):
Created directory '/home/luffy/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/luffy/.ssh/id_rsa.
Your public key has been saved in /home/luffy/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:ajYVV5CAmQ4bfxqSWqcUinAkMVY4P0ebAELImc5a4ZE luffy@desktop.example.com
The key's randomart image is:
+---[RSA 2048]----+
|O*Bo   +...o.    |
|=OE.+.+   ..     |
|++o+oOo . .      |
|.o+o*+= .o       |
|.. +o+ +S        |
|. . . .o         |
|      =          |
|     o .         |
|                 |
+----[SHA256]-----+
[luffy@desktop ~]$ ssh-copy-id root@server.example.com
/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/luffy/.ssh/id_rsa.pub"
The authenticity of host 'server.example.com (172.25.0.11)' can't be established.
ECDSA key fingerprint is SHA256:1uN2z3acLRzzLvWlORkKs84Z8COie5mJKC4urQOJ1Z0.
ECDSA key fingerprint is MD5:f4:78:64:cc:a2:db:a7:db:90:d3:cf:90:db:6d:00:d3.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@server.example.com's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@server.example.com'"
and check to make sure that only the key(s) you wanted were added.

[luffy@desktop ~]$ ssh 'root@server.example.com' hostname
server.example.com

[luffy@desktop ~]$ ssh 'root@server.example.com'
[root@server ~]# sed -i "/PasswordAuthentication/a\PasswordAuthentication no" /etc/ssh/sshd_config
[root@server ~]# systemctl restart sshd

[root@desktop ~]# ssh root@server.example.com hostname
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
[luffy@desktop ~]$ ssh root@server.example.com hostname
server.example.com
```

## 6. Set time

Chinese said, time is money. It's important for some services too. Please set local clocks and time zone.

- Synchronize the hardware clock with a NTP time source.
- Adjust the timezone to **Asia/Shanghai**.
- Finish above setting both on **desktop.example.com** and **server.example.com**.

```bash
[root@desktop ~]# timedatectl set-timezone Asia/Shanghai
[root@desktop ~]# sed -i -e "/iburst/d" -e "/www.pool.ntp.org/a\server lab.example.com iburst" /etc/chrony.conf
[root@desktop ~]# systemctl restart chronyd
[root@desktop ~]# timedatectl
...
NTP enabled: yes
NTP synchronized: yes
...

[root@server ~]# timedatectl set-timezone Asia/Shanghai
[root@server ~]# sed -i -e "/iburst/d" -e "/www.pool.ntp.org/a\server lab.example.com iburst" /etc/chrony.conf
[root@server ~]# systemctl restart chronyd
[root@server ~]# timedatectl
...
NTP enabled: yes
NTP synchronized: yes
...
```

## 7. Configure networking

A basic understanding of networking is important for anyone managing a server. Configure networking.

- Configure **eth1** both on **desktop.example.com** and **server.example.com**.
- Do **not** modify network setting on other networking device.
- Make sure listed network setting existing and has effect.

|Host|Device|Connection Name|IPv4 Address|IPv4 Gateway|IPv4 Method|IPv4 DNS|
|-|-|-|-|-|-|-|
|desktop.example.com|eth1|eht1|172.25.0.10/24, 172.25.1.10/24|-|manual|172.25.0.254|
|server.example.com|eth1|eth1|172.25.0.11/24, 172.25.1.11/24|-|manual|172.25.0.254|

```bash
[root@desktop ~]# nmcli connection add con-name eth1 ifname eth1 type ethernet ipv4.addresses 172.25.0.10/24 ipv4.dns 172.25.0.254 ipv4.method manual connection.autoconnect yes
Connection 'eth1' (d12861d1-fe1b-4065-8692-e10e355900aa) successfully added.
[root@desktop ~]# nmcli connection modify eth1 +ipv4.addresses 172.25.1.10/24
[root@desktop ~]# nmcli connection delete "System eth1"
Connection 'System eth1' (9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04) successfully deleted.
[root@desktop ~]# nmcli connection show
NAME         UUID                                  TYPE            DEVICE
System eth0  5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  802-3-ethernet  eth0   
System eth2  3a73717e-65ab-93e8-b518-24f5af32dc0d  802-3-ethernet  eth2   
System eth3  c5ca8081-6db2-4602-4b46-d771f4330a6d  802-3-ethernet  eth3   
eth1         d12861d1-fe1b-4065-8692-e10e355900aa  802-3-ethernet  eth1   
[root@desktop ~]# ip addr show eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 52:54:00:f2:b1:b3 brd ff:ff:ff:ff:ff:ff
    inet 172.25.0.10/24 brd 172.25.0.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet 172.25.1.10/24 brd 172.25.1.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::def8:9818:f2c4:1a07/64 scope link
       valid_lft forever preferred_lft forever

[root@server ~]# nmcli connection add con-name eth1 ifname eth1 type ethernet ipv4.addresses 172.25.0.11/24 ipv4.dns 172.25.0.254 ipv4.method manual connection.autoconnect yes
Connection 'eth1' (c9a369b3-0832-4798-b0b9-f4a9038d6b98) successfully added.
[root@server ~]# nmcli connection modify eth1 +ipv4.addresses 172.25.1.11/24
[root@server ~]# nmcli connection delete "System eth1"
Connection 'System eth1' (9c92fad9-6ecb-3e6c-eb4d-8a47c6f50c04) successfully deleted.
[root@server ~]# nmcli connection show
NAME         UUID                                  TYPE            DEVICE
System eth0  5fb06bd0-0bb0-7ffb-45f1-d6edd65f3e03  802-3-ethernet  eth0   
System eth2  3a73717e-65ab-93e8-b518-24f5af32dc0d  802-3-ethernet  eth2   
System eth3  c5ca8081-6db2-4602-4b46-d771f4330a6d  802-3-ethernet  eth3   
eth1         c9a369b3-0832-4798-b0b9-f4a9038d6b98  802-3-ethernet  eth1   
[root@server ~]# ip addr show eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 52:54:00:20:e5:db brd ff:ff:ff:ff:ff:ff
    inet 172.25.0.11/24 brd 172.25.0.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet 172.25.1.11/24 brd 172.25.1.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::ed32:5198:9846:b87c/64 scope link
       valid_lft forever preferred_lft forever

[root@desktop ~]# ping -c1 172.25.1.11
PING 172.25.1.11 (172.25.1.11) 56(84) bytes of data.
64 bytes from 172.25.1.11: icmp_seq=1 ttl=64 time=0.385 ms
...
[root@desktop ~]# ping -c1 lab.example.com
PING lab.example.com (172.25.0.254) 56(84) bytes of data.
64 bytes from lab.example.com (172.25.0.254): icmp_seq=1 ttl=64 time=0.185 ms
...
[root@server ~]# ping -c1 172.25.1.10
PING 172.25.1.10 (172.25.1.10) 56(84) bytes of data.
64 bytes from 172.25.1.10: icmp_seq=1 ttl=64 time=0.529 ms
...
[root@server ~]# ping -c1 lab.example.com
PING lab.example.com (172.25.0.254) 56(84) bytes of data.
64 bytes from lab.example.com (172.25.0.254): icmp_seq=1 ttl=64 time=0.340 ms
...
```
## 8. Configure YUM package repositories

Base on information security reason, some organization may have internal YUM package repositories. Please configure one.

- Download lastest [CentOS DVD ISO](https://www.centos.org/download/).
- Attach the ISO to **lab.example.com**, make sure it persistent mount on **/var/www/html/pub/dvd/**.
- Remove all present YUM package repositories both on **server.example.com** and **desktop.example.com**.
- Add a new repository from http://lab.example.com/pub/dvd/ both on **server.example.com** and **desktop.example.com**.

```bash
#Download and attach the ISO, it should available as /dev/sr0
[root@lab ~]# mkdir /var/www/html/pub/dvd
[root@lab ~]# echo "/dev/sr0        /var/www/html/pub/dvd   iso9660 loop    0 0" >> /etc/fstab
[root@lab ~]# mount -a
[root@lab ~]# df -h
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup00-LogVol00   38G  1.1G   37G   3% /
devtmpfs                         236M     0  236M   0% /dev
tmpfs                            245M     0  245M   0% /dev/shm
tmpfs                            245M  4.4M  240M   2% /run
tmpfs                            245M     0  245M   0% /sys/fs/cgroup
/dev/vda2                       1014M   66M  949M   7% /boot
tmpfs                             49M     0   49M   0% /run/user/0
/dev/loop0                       4.3G  4.3G     0 100% /var/www/html/pub/dvd

[root@desktop ~]# mkdir /etc/yum.repos.d/backup
[root@desktop ~]# mv /etc/yum.repos.d/* /etc/yum.repos.d/backup/
mv: cannot move ‘/etc/yum.repos.d/backup’ to a subdirectory of itself, ‘/etc/yum.repos.d/backup/backup’
[root@desktop ~]# ls /etc/yum.repos.d/
backup
[root@desktop ~]# yum-config-manager --add-repo=http://lab.example.com/pub/dvd
Loaded plugins: fastestmirror
adding repo from: http://lab.example.com/pub/dvd

[lab.example.com_pub_dvd]
name=added from: http://lab.example.com/pub/dvd
baseurl=http://lab.example.com/pub/dvd
enabled=1


[root@desktop ~]# sed -i "/enabled=1/a\gpgcheck=0" /etc/yum.repos.d/lab.example.com_pub_dvd.repo
[root@desktop ~]# cat /etc/yum.repos.d/lab.example.com_pub_dvd.repo

[lab.example.com_pub_dvd]
name=added from: http://lab.example.com/pub/dvd
baseurl=http://lab.example.com/pub/dvd
enabled=1
gpgcheck=0
[root@desktop ~]# yum clean all
[root@desktop ~]# yum install tree -y &> /dev/null && echo ok
ok

[root@server ~]# mkdir /etc/yum.repos.d/backup
[root@server ~]# mv /etc/yum.repos.d/* /etc/yum.repos.d/backup/
mv: cannot move ‘/etc/yum.repos.d/backup’ to a subdirectory of itself, ‘/etc/yum.repos.d/backup/backup’
[root@server ~]# ls /etc/yum.repos.d/
backup
[root@server ~]# yum-config-manager --add-repo=http://lab.example.com/pub/dvd
Loaded plugins: fastestmirror
adding repo from: http://lab.example.com/pub/dvd

[lab.example.com_pub_dvd]
name=added from: http://lab.example.com/pub/dvd
baseurl=http://lab.example.com/pub/dvd
enabled=1


[root@server ~]# sed -i "/enabled=1/a\gpgcheck=0" /etc/yum.repos.d/lab.example.com_pub_dvd.repo
[root@server ~]# cat /etc/yum.repos.d/lab.example.com_pub_dvd.repo

[lab.example.com_pub_dvd]
name=added from: http://lab.example.com/pub/dvd
baseurl=http://lab.example.com/pub/dvd
enabled=1
gpgcheck=0

[root@server ~]# yum clean all
[root@server ~]# yum install tree -y &> /dev/null && echo ok
ok
```
## 9. Schedule recurring backup jobs

Almost all sensitive job will schedule on non work time. Please schedule recurring backup jobs follow requiring.

- Schedule jobs on **desktop.example.com**.
- User **hancock** is not allow to use cron.
- Make sure listed backup plan works.

|Remote Host|Remote Directory|Local Synchronize Directory|User|Schedule|
|-|-|-|-|-|
|server.example.com|/var/log/|/var/remote-log|System cron jobs|22:40, every day|

```bash
[root@desktop ~]# echo hancock >> /etc/cron.deny
[root@desktop ~]# cat /etc/cron.deny
hancock
[root@desktop ~]# su - hancock
[hancock@desktop ~]$ crontab -e
You (hancock) are not allowed to use this program (crontab)
See crontab(1) for more information

[root@desktop ~]# cp /home/luffy/.ssh/id_rsa ~/.ssh/id_rsa
[root@desktop ~]# mkdir /var/remote-log
[root@desktop ~]# mkdir ~/bin
[root@desktop ~]# vim ~/bin/backup-remote-log.sh
[root@desktop ~]# cat ~/bin/backup-remote-log.sh
#!/bin/bash
/usr/bin/rsync -av root@server.example.com:/var/log/* /var/remote-log/
[root@desktop ~]# chmod +x ~/bin/backup-remote-log.sh
[root@desktop ~]# echo "40 22 * * * root /root/bin/backup-remote-log.sh" >> /etc/crontab
```
