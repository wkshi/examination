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

## 10. Attach system to centralized authentication services

Keeping local user accounts from multi machines in sync is a daunting task. Please attach system to centralized LDAP and Kerberos servers.

- Attach both **server.example.com** and **desktop.example.com** to centralized LDAP and Kerberos servers.
- Follow listed setting to finish task.

|Server|Base DN|TLS Cert|Realm|kdc|Admin Server|
|-|-|-|-|-|-|
|lab.example.com|dc=example,dc=com|http://lab.example.com/pub/EXAMPLE-CA-CRT|EXAMPLE.COM|lab.example.com|lab.example.com|

- Make sure listed user could login successfully, with remote home directory mounted. Their home directory export by NFS on **lab.example.com:/home/guests**.

|Username|Password|Home Directory|
|-|-|-|
|bruce|kerberos|/home/guests/bruce/|
|charles|kerberos|/home/guests/charles/|
|loki|kerberos|/home/guests/loki/|
|peter|kerberos|/home/guests/peter/|
|steve|kerberos|/home/guests/steve/|

```bash
[root@desktop ~]# yum install sssd krb5-workstation setuptool autofs wget -y
[root@desktop ~]# setup
[*] Use LDAP
[*] Use Kerberos
[*] Use TLS
Server: lab.example.com
Base DN: dc=example,dc=com
Realm: EXAMPLE.COM
KDC: lab.example.com
Admin Server: lab.example.com
[root@desktop ~]# wget -O /etc/openldap/cacerts/EXAMPLE-CA-CRT http://lab.example.com/pub/EXAMPLE-CA-CRT
Then press OK.
[root@desktop ~]# systemctl status sssd

[root@desktop ~]# vim /etc/auto.master.d/lab.autofs
[root@desktop ~]# vim /etc/lab.master
[root@desktop ~]# cat /etc/auto.master.d/lab.autofs
/home/guests /etc/lab.master
[root@desktop ~]# cat /etc/lab.master
*	lab.example.com:/home/guests/&
[root@desktop ~]# systemctl enable autofs
[root@desktop ~]# systemctl start autofs
[root@desktop ~]# ssh loki@desktop.example.com
# succeed...

[root@server ~]# yum install sssd krb5-workstation setuptool autofs wget -y
[root@server ~]# setup
[*] Use LDAP
[*] Use Kerberos
[*] Use TLS
Server: lab.example.com
Base DN: dc=example,dc=com
Realm: EXAMPLE.COM
KDC: lab.example.com
Admin Server: lab.example.com
[root@server ~]# wget -O /etc/openldap/cacerts/EXAMPLE-CA-CRT http://lab.example.com/pub/EXAMPLE-CA-CRT
Then press OK.
[root@server ~]# systemctl status sssd

[root@server ~]# vim /etc/auto.master.d/lab.autofs
[root@server ~]# vim /etc/lab.master
[root@server ~]# cat /etc/auto.master.d/lab.autofs
/home/guests /etc/lab.master
[root@server ~]# cat /etc/lab.master
*	lab.example.com:/home/guests/&
[root@server ~]# systemctl enable autofs
[root@server ~]# systemctl start autofs
[root@server ~]# su - loki
# succeed...
```

## 11. Add disk and partitions

There usually more than one disk on a server. Please add disk and partitions.

- Attach a 10G disk to **desktop.example.com**.
- Add listed partitions.

|Device|Size|Mount Point|File System|
|-|-|-|-|
|/dev/vdb1|1G|/mnt/disk1/|ext4|
|/dev/vdb2|2G|/mnt/disk2/|xfs|
|/dev/vdb3|3G|/mnt/disk3/|xfs|

```bash
# Attach disk by provider
[root@desktop ~]# fdisk -l /dev/vdb

Disk /dev/vdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
[root@desktop ~]# fdisk /dev/vdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x10196a06.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p):
Using default response p
Partition number (1-4, default 1):
First sector (2048-20971519, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +1G
Partition 1 of type Linux and of size 1 GiB is set

Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p):
Using default response p
Partition number (2-4, default 2):
First sector (2099200-20971519, default 2099200):    
Using default value 2099200
Last sector, +sectors or +size{K,M,G} (2099200-20971519, default 20971519): +2G
Partition 2 of type Linux and of size 2 GiB is set

Command (m for help): n
Partition type:
   p   primary (2 primary, 0 extended, 2 free)
   e   extended
Select (default p):
Using default response p
Partition number (3,4, default 3):
First sector (6293504-20971519, default 6293504):
Using default value 6293504
Last sector, +sectors or +size{K,M,G} (6293504-20971519, default 20971519): +3G
Partition 3 of type Linux and of size 3 GiB is set

Command (m for help): p

Disk /dev/vdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x10196a06

   Device Boot      Start         End      Blocks   Id  System
/dev/vdb1            2048     2099199     1048576   83  Linux
/dev/vdb2         2099200     6293503     2097152   83  Linux
/dev/vdb3         6293504    12584959     3145728   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
[root@desktop ~]# mkfs.ext4 /dev/vdb1
[root@desktop ~]# mkfs.xfs /dev/vdb2
[root@desktop ~]# mkfs.xfs /dev/vdb3
[root@desktop ~]# mkdir /mnt/disk{1..3}
[root@desktop ~]# echo "/dev/vdb1 /mnt/disk1 ext4 defaults 0 0" >> /etc/fstab
[root@desktop ~]# echo "/dev/vdb2 /mnt/disk2 xfs  defaults 0 0" >> /etc/fstab
[root@desktop ~]# echo "/dev/vdb3 /mnt/disk3 xfs  defaults 0 0" >> /etc/fstab
[root@desktop ~]# mount -a
```

## 12. Manage logical volume management (LVM)

Logical volumes and logical volume management could make it easier to manage disk space. Please create LVM.

- Create LVM on **desktop.example.com**.
- Make sure listed requiring.

|PV|PV Size|VG|PE Size|LV|LV Size|File System|Mount Point|
|-|-|-|-|-|-|
|/dev/vdb5|500M|data-pool|4M|data-storage|200 PEs|xfs|/mnt/data-storage/|
|/dev/vdb6|500M|data-pool|4M|meta-data|100M|xfs|/mnt/meta-data/|
|/dev/vdb7|2G|backup-pool|8M|backup-storage|1G|xfs|/mnt/backup-storage/|

- Extend **backup-storage** to **1.2G**.

```bash
[root@desktop ~]# fdisk /dev/vdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): n
Partition type:
   p   primary (3 primary, 0 extended, 1 free)
   e   extended
Select (default e):
Using default response e
Selected partition 4
First sector (12584960-20971519, default 12584960):
Using default value 12584960
Last sector, +sectors or +size{K,M,G} (12584960-20971519, default 20971519):
Using default value 20971519
Partition 4 of type Extended and of size 4 GiB is set

Command (m for help): n
All primary partitions are in use
Adding logical partition 5
First sector (12587008-20971519, default 12587008):
Using default value 12587008
Last sector, +sectors or +size{K,M,G} (12587008-20971519, default 20971519): +500M
Partition 5 of type Linux and of size 500 MiB is set

Command (m for help): t
Partition number (1-5, default 5):
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): n
All primary partitions are in use
Adding logical partition 6
First sector (13613056-20971519, default 13613056):
Using default value 13613056
Last sector, +sectors or +size{K,M,G} (13613056-20971519, default 20971519): +500M
Partition 6 of type Linux and of size 500 MiB is set

Command (m for help): t
Partition number (1-6, default 6):
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): n
All primary partitions are in use
Adding logical partition 7
First sector (14639104-20971519, default 14639104):
Using default value 14639104
Last sector, +sectors or +size{K,M,G} (14639104-20971519, default 20971519): +2G
Partition 7 of type Linux and of size 2 GiB is set

Command (m for help): t
Partition number (1-7, default 7):
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): p

Disk /dev/vdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x10196a06

   Device Boot      Start         End      Blocks   Id  System
/dev/vdb1            2048     2099199     1048576   83  Linux
/dev/vdb2         2099200     6293503     2097152   83  Linux
/dev/vdb3         6293504    12584959     3145728   83  Linux
/dev/vdb4        12584960    20971519     4193280    5  Extended
/dev/vdb5        12587008    13611007      512000   8e  Linux LVM
/dev/vdb6        13613056    14637055      512000   8e  Linux LVM
/dev/vdb7        14639104    18833407     2097152   8e  Linux LVM

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
[root@desktop ~]# partprobe

[root@desktop ~]# pvcreate /dev/vdb{5,6,7}
  Physical volume "/dev/vdb5" successfully created.
  Physical volume "/dev/vdb6" successfully created.
  Physical volume "/dev/vdb7" successfully created.
[root@desktop ~]# vgcreate data-pool /dev/vdb{5,6}
  Volume group "data-pool" successfully created
[root@desktop ~]# vgcreate -s 8M backup-pool /dev/vdb7
  Volume group "backup-pool" successfully created
[root@desktop ~]# vgs
  VG          #PV #LV #SN Attr   VSize   VFree  
  VolGroup00    1   2   0 wz--n- <38.97g      0
  backup-pool   1   0   0 wz--n-   1.99g   1.99g
  data-pool     2   0   0 wz--n- 992.00m 992.00m
[root@desktop ~]# lvcreate -l 200 -n data-storage data-pool
  Logical volume "data-storage" created.
[root@desktop ~]# lvcreate -L 100M -n meta-data data-pool
  Logical volume "meta-data" created.
[root@desktop ~]# lvcreate -L 1G -n backup-storage backup-pool
  Logical volume "backup-storage" created.

[root@desktop ~]# mkdir /mnt/{data-storage,meta-data,backup-storage}

[root@desktop ~]# mkfs.xfs /dev/data-pool/data-storage
[root@desktop ~]# mkfs.xfs /dev/data-pool/meta-data
[root@desktop ~]# mkfs.xfs /dev/backup-pool/backup-storage

[root@desktop ~]# echo "/dev/data-pool/data-storage /mnt/data-storage xfs defaults 0 0" >> /etc/fstab
[root@desktop ~]# echo "/dev/data-pool/meta-data /mnt/meta-data xfs defaults 0 0" >> /etc/fstab
[root@desktop ~]# echo "/dev/backup-pool/backup-storage /mnt/backup-storage xfs defaults 0 0" >> /etc/fstab
[root@desktop ~]# mount -a

[root@desktop ~]# lvextend -L 1.2G /dev/backup-pool/backup-storage
  Rounding size to boundary between physical extents: 1.20 GiB.
  Size of logical volume backup-pool/backup-storage changed from 1.00 GiB (128 extents) to 1.20 GiB (154 extents).
  Logical volume backup-pool/backup-storage successfully resized.
[root@desktop ~]# xfs_growfs /dev/backup-pool/backup-storage
```
