ubuntu
------
## docker container start time
```sh
$ docker container inspect --format='{{.State.StartedAt}}' signature_sync | xargs date +%s -d
1604383050
```

## mail
```sh
echo "hello world" | mail -s "a subject" someone@somewhere.com

mail -s "a subject" someone@somewhere.com < /tmp/application.log

echo "Sending an attachment." | mutt -a file.zip -s "attachment" target@email.com
```


## RPI 树莓派系统备份
Backup from SD Card A
```sh
sudo dd bs=4M if=/dev/sdc | gzip>/home/ubuntu/raspberry.gz
```
Format a new SD Card B

Restore
```sh
sudo gzip -dc /home/ubuntu/raspberry.gz | sudo dd bs=4M of=/dev/sdc
```
https://www.raspberrypi.org/documentation/linux/filesystem/backup.md

## count files in dir
```sh
ls -l | wc -l
```

## rsync dir
```sh
rsync -av --update /local/dir/* one@remote:/remote/dir/


rsync -auvz one@remote:/remote/dir/* /local/dir/.

-a --archive
-u --update: skip files that are newer on the receiver
-v --verbose
-z --compress: compress file data during the transfer
```

## swap create
```sh
sudo swapoff /swapfile
sudo rm  /swapfile

# 4G
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

sudo vi /etc/fstab
/swapfile swap swap defaults 0 0

sudo swapon --show
sudo free -h
```

## virtualenv
```sh
virtualenv -p python3.6 venv
```

## mount as read/write
```sh
mount -o remount, rw /
```

## boot time
```sh
$ uptime -s
2019-06-25 05:58:55
$ last reboot
wtmp begins Mon Jan 13 06:41:22 2020
$ last shutdown

automation@ubuntu:~$

# 开机时间
systemd-analyze
```

## ubuntu/linux kernel version
```sh
$ uname -a
Linux ngdocker 4.15.0-54-generic #58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

$ uname -v
#58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019

$ uname -r
4.15.0-54-generic

$ cat /proc/version
Linux version 4.15.0-54-generic (buildd@lgw01-amd64-014) (gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)) #58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019

$ cat /etc/issue
Ubuntu 18.04.2 LTS \n \l

$ cat /etc/os-release
NAME="Ubuntu"
VERSION="18.04.2 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.2 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic

$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.2 LTS
Release:	18.04
Codename:	bionic
```
## du size of dir
```sh
$ du -sh ~/projects/
2.5G	/home/automation/projects/
```

## find largest files
```
sudo du -a /dir/ | sort -n -r | head -n 20

#
sudo du -a / 2>/dev/null | sort -n -r | head -n 20
```

## find with delete/compress
```sh
find /var/log/monitor/ -type f -name '*.log' -mtime +7 | xargs gzip -9
find /var/log/monitor/ -mtime +60 -delete
```

## netstat/ss/lsof  网络端口/network port
```
sudo netstat -tulpn | grep 80
sudo netstat -apn | grep 80

t: tcp
u: udp
a: all
l: listening ports
p: process
n: network

sudo ss -tulpn | grep 80

n: numeric, resolve service port as name

sudo losf -i:80

```

## PS process tree 
```
ps auxf|grep xy
```


## date
```
sudo dpkg-reconfigure tzdata
```

## supervisor
https://blog.csdn.net/A156348933/article/details/85158005

`/etc/supervisor/supervisord.conf`
```
[unix_http_server]
file=/var/run/supervisor.sock   ; the path to the socket file, set to be /var/run instead of /tmp

[supervisord]
logfile=/var/log/supervisor/supervisord.log ; main log file; default $CWD/supervisord.log, set to be /var/run instead of /tmp
logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
loglevel=info                ; log level; default info; others: debug,warn,trace
pidfile=/var/run/supervisord.pid ; supervisord pidfile; default supervisord.pid, set to be /var/run instead of /tmp

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket, set to be /var/run instead of /tmp, need to be matched with the setting in [unix_http_server]

[inet_http_server]         ; inet (TCP) server disabled by default
port=127.0.0.1:9001        ; ip_address:port specifier, *:port for all iface
username=automation        ; default is no username (open server)
password=password          ; default is no password (open server)

[include]
;files = relative/directory/*.ini
files = /etc/supervisor/conf.d/*.conf
```
`/etc/supervisor/conf.d/android.conf`
```
[program:android]
command=python3 android.py  ;program start command
environment=HOME="/home/automation",USER="automation"  ;environment variables
user=automation  ;process execute user
autostart=true   
autorestart=true 
stderr_logfile=/var/log/supervisor/android.err.log
stdout_logfile=/var/log/supervisor/android.log
loglevel=info
```
**supervisorctl**
```
# start|stop program
sudo supervisorctl stop|start program_name 
# restart program
sudo supervisorctl restart program_name
# check status
sudo supervisorctl status program_name
```

## KVM in KVM
https://www.linux-kvm.org/page/Nested_Guests
```
# If you have an Intel CPU, use this:
$ cat /etc/modprobe.d/kvm_intel.conf
options kvm-intel nested=Y

# If you have an AMD CPU, then this:
$ cat /etc/modprobe.d/kvm_amd.conf
options kvm-amd nested=1
```
## KVM qcow2 扩容 expand storage
```
[root@vp-01 export]# qemu-img info test.qcow2
image: test.qcow2
file format: qcow2
virtual size: 20G (21474836480 bytes)
disk size: 2.1G
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: false
    refcount bits: 16
    corrupt: false
新增磁盘容量大小20G

[root@vp-01 export]# qemu-img resize test.qcow2 +20G
Image resized.
[root@vp-01 export]# qemu-img info test.qcow2
image: test.qcow2
file format: qcow2
virtual size: 40G (42949672960 bytes)
disk size: 2.1G
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: false
    refcount bits: 16
    corrupt: false
    
然后使用 Gparted 扩容，保存
```
## Add persistent disk
```
# persistant mount
$ sudo fdisk -l
...
Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

$ sudo blkid
...
/dev/sdb: UUID="b77a4987-15f6-4d24-88ff-777c0eb77a3a" TYPE="ext4"

$ sudo vi /etc/fstab
...
# /dev/sdb
UUID=b77a4987-15f6-4d24-88ff-777c0eb77a3a /var/lib/docker ext4 defaults 0 0
```
## move /var/lib/docker to a new disk
```
$ systemctl stop docker
$ ps aux | grep -i docker | grep -v grep

$ systemctl daemon-reload

# sync old dir to new location
$ sudo mv /var/lib/docker /var/lib/docker.bak

$ sudo mkdir /var/lib/docker
$ sudo mount /dev/sdb /var/lib/docker

$ sudo rsync -aqxP /var/lib/docker.bak/ /var/lib/docker/

$ sudo systemctl start docker

$ ps aux | grep -i docker | grep -v grep
root      2095  0.2  0.4 664472 36176 ?        Ssl  18:14   0:00 /usr/bin/docker daemon -g  /new/path/docker -H fd://
root      2100  0.0  0.1 360300 10444 ?        Ssl  18:14   0:00 docker-containerd -l /var/run/docker/libcontainerd/docker-containerd.sock --runtime docker-runc
```
[move to a new path](https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux)
```
`/lib/systemd/system/docker.service`
FROM:
ExecStart=/usr/bin/docker daemon -H fd://
TO:
ExecStart=/usr/bin/docker daemon -g /new/path/docker -H fd://

other step is the same
```

## KVM for Android x86_64
https://fosspost.org/tutorials/install-android-8-1-oreo-on-linux
```sh
qemu-img create -f qcow2 android.img 15G

sudo qemu-system-x86_64 -m 2048 -boot d -enable-kvm -smp 3 -net nic -net user -hda android.img -cdrom /home/mhsabbagh/android-x86_64-8.1-r1.iso

# Auto_Installation

sudo qemu-system-x86_64 -m 2048 -boot d -enable-kvm -smp 3 -net nic -net user -hda android.img

# removed the -cdrom option to no boot from the ISO
```


## Managing JAVA
```
apt install default-jre
apt install openjdk-11-jre-headless
apt install openjdk-8-jre-headless
apt install openjdk-9-jre-headless
```
```
sudo update-alternatives --config java

>>> Output
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      manual mode
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
  3            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode
```
JAVA HOME
```
sudo vi /etc/environment

# add
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64/bin/"
```


## RDP
```sh
# Step 1 – Install xRDP and XFCE4
sudo apt update
sudo apt install xrdp xfce4 xfce4-terminal
# Step 2 - Configure
echo xfce4-session > ~/.xsession
# Step 3 - Restart xRDP
sudo systemctl restart xrdp
# Step 4 - Testing
On Windows PC, use mstsc to connect to the ubuntu server
```
## VNC remote desktop
https://www.makeuseof.com/tag/how-to-establish-simple-remote-desktop-access-between-ubuntu-and-windows/
```
sudo apt install tigervnc-standalone-server tigervnc-xorg-extension tigervnc-viewer

```
`vi ~/.vnc/xstartup`
```
#start Gnome 3 Desktop 
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
vncconfig -iconic &
dbus-launch --exit-with-session gnome-session &
```
```
vncpasswd

vncserver -list

vncserver -kill :1
vncserver -kill :*

ssh -L 10.138.14.34:5909:127.0.0.1:5901 -C -N -l automation 10.138.14.34
```

## linux bridge
* virsh define bridge
```
automation@watsng-u1804-kvm-14-12:~$ virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
 net-manage           active     yes           yes

automation@watsng-u1804-kvm-14-12:~$ vi /tmp/net-manage.xml
<network>
  <name>net-manage</name>
  <forward mode='bridge'/>
  **<bridge name='br-manage'/>**
</network>

automation@watsng-u1804-kvm-14-12:~$ virsh net-define /tmp/net-manage.xml

automation@watsng-u1804-kvm-14-12:~$ virsh net-dumpxml net-manage
<network connections='1'>
  <name>net-manage</name>
  <uuid>2fa5c4cb-af9d-4ca8-b030-bdf6dc11e5ca</uuid>
  <forward mode='bridge'/>
  **<bridge name='br-manage'/>**
</network>

automation@watsng-u1804-kvm-14-12:~$ virsh net-list --all
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
 net-manage           active     yes           yes

automation@watsng-u1804-kvm-14-12:~$ virsh net-autostart net-manage
Network net-manage marked as autostarted

```
/etc/netplan/50-cloud-init.yaml
```
network:
    bridges:
        br-manage:
            dhcp4: false
            addresses:
              - 10.138.14.12/24
            gateway4: 10.138.14.254
            nameservers:
                addresses:
                  - 172.23.0.8
                search: []
            interfaces:
              - enp3s0
    enp3s0:
            addresses: []
            dhcp4: false

```

## std output to null
```
python3 main.py > /dev/null 2>&1 &
```
输出重定向

- stdin(0): 标准输入, keyboard 键盘输入, 并返回在前端
- stdout(1): 标准输出, monitor 正确返回值输出到前端
- stderr(2): 标准错误, monitor 错误返回值 输出到前端


- &1 表示输出通道1
- 1>&2 意思是把标准输出重定向到标准错误.
- 2>&1 意思是把标准错误输出重定向到标准输出。
- &>filename 意思是把标准输出和标准错误输出都重定向到文件filename中

## nohup
no hang up

```sh
nohup Command [ Arg ... ] [　& ]
```
nohup命令运行由Command参数和任何相关的Arg参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用nohup命令运行后台中的程序。要运行后台中的nohup命令，添加&（表示“and”的符号）到命令的尾部。

```sh
nohup command > myout.file 2>&1 &

nohup command >/dev/null 2>&1 &
```

使用jobs查看任务。使用fg %n关闭。

## polipo http proxy
```
# This file only needs to list configuration variables that deviate  
# from the default values.  See /usr/share/doc/polipo/examples/config.sample  
# and "polipo -v" for variables you can tweak and further information.  
  
logSyslog = true  
logFile = /var/log/polipo/polipo.log  
  
proxyAddress = "0.0.0.0"  
  
socksParentProxy = "127.0.0.1:1080"  
socksProxyType = socks5  
  
chunkHighMark = 50331648  
objectHighMark = 16384  
  
serverMaxSlots = 64  
serverSlots = 16  
serverSlots1 = 32  
```
## plasma linux

https://plasma-mobile.org/

## gnome install

blog.csdn.net/czwin32768/article/details/51703043

## youdao-dict
```
从Ubuntu 14.04升级到16.04以后，有道词典就安装不上了。因为官方的deb包（Ubuntu版本的）依赖gstreamer0.10-plugins-ugly，但是该软件在16.04里面已经没有了。但其实没有该包，完全不影响有道词典的使用。所以我们可以去掉deb包里面对于该库的依赖。具体操作如下：
1. 从官方下载Ubuntu版本的deb包：youdao-dict_1.1.0-0-ubuntu_amd64.deb
2. 创建youdao目录，把该deb包解压到youdao目录：
allan@NYC:~$ dpkg -X ./youdao-dict_1.1.0-0-ubuntu_amd64.deb  youdao
3. 解压deb包中的control信息（包的依赖就写在这个文件里面）：
allan@NYC:~$ dpkg -e ./youdao-dict_1.1.0-0-ubuntu_amd64.deb youdao/DEBIAN
4. 编辑control文件，删除Depends里面的gstreamer0.10-plugins-ugly。
5. 创建youdaobuild目录，重新打包：
allan@NYC:~$ dpkg-deb -b youdao youdaobuild/
这样，在youdaobuild里面就会生成一个新的deb包。我们安装这个包就不会存在依赖的问题了。
Ubuntu 16.04之前的版本安装有道词典linux版本可参见：《Ubuntu中使用有道词典》。
```
## show git branch shell
```
force_color_prompt=yes

# Add git branch if its present to PS1
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
unset color_prompt force_color_prompt
```

## chrome-headless
```
google-chrome --headless --disable-gpu --dump-dom http://www.baidu.com
```
## get PID of process by specifying process name and store it in a variable to kill, ps to kill
```
ps axf | grep <process name> | grep -v grep | awk '{print "kill -9 " $1}' | sh
```
## restore ipv6 on eth0
```
ifconfig eth0 up
sysctl -w net.ipv6.conf.eth0.disable_ipv6=0
```
## add new user
```
$是普通管员，#是系统管理员，在Ubuntu下，root用户默认是没有密码的，因此也就无法使用（据说是为了安全）。想用root的话，得给root用户设置一个密码：
sudo passwd root
然后登录时用户名输入root，再输入密码就行了。
ubuntu建用户最好用adduser，虽然adduser和useradd是一样的在别的linux糸统下，但是我在ubuntu下用useradd时，并没有创建同名的用户主目录。
例子：adduser user1
这样他就会自动创建用户主目录，创建用户同名的组。
root@ubuntu:~# sudo adduser linuxidc
[sudo] password for xx:
输入xx用户的密码，出现如下信息
正在添加用户"linuxidc"…
正在添加新组"linuxidc" (1006)…
正在添加新用户"linuxidc" (1006) 到组"linuxidc"…
创建主目录"/home/linuxidc"…
正在从"/etc/skel"复制文件…
输入新的 UNIX 口令：
重新输入新的 UNIX 口令：
两次输入linuxidc的初始密码，出现的信息如下
passwd: password updated successfully
Changing the user information for linuxidc
Enter the new value, or press ENTER for the default
Full Name []:
Room Number []:
Work Phone []:
Home Phone []:
Other []:
Full Name []:等信息一路回车
这个信息是否正确？ [Y/n] y
到此，用户添加成功。如果需要让此用户有root权限，执行命令：
root@ubuntu:~# sudo vim /etc/sudoers
修改文件如下：
# User privilege specification
root ALL=(ALL) ALL
linuxidc ALL=(ALL) ALL
保存退出，linuxidc用户就拥有了root权限。
```
## ssh-keygen
```
vultr@vultr:~/.ssh$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vultr/.ssh/id_rsa): 
/home/vultr/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vultr/.ssh/id_rsa.
Your public key has been saved in /home/vultr/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:W1Dqwbl+ZTPUQVoqXiunkoHBhWOPHeIEXgBqntuGRq0 vultr@vultr.guest
The key's randomart image is:
+---[RSA 4096]----+
|  ..oo... .  .+  |
| . . o*o.+   = . |
|..  .+o**.. = .  |
|o o   o+o= + .   |
| + .  . S + B    |
|. =    . = * o   |
| E o    = o      |
|. .      o       |
|                 |
+----[SHA256]-----+
vultr@vultr:~/.ssh$ 
vultr@vultr:~/.ssh$ cat id_rsa.pub >> authorized_keys 
vultr@vultr:~/.ssh$ sudo service sshd restart
```
## ssh 免密登录　local to remote without password
```
local:
ssh-keygen -t rsa -b 4096
ssh-copy-id [path/to/rsa] user@remote
ssh-add
# fix sign_and_send_pubkey: signing failed: agent refused operation

server:
do nothing
cat ~/.ssh/authorized_keys
```

## ssh 测试
```sh
ssh -Tv git@gitlab.com
```

## backup and migrage

http://www.jianshu.com/p/5c12289dbcd1

http://blog.csdn.net/dark5669/article/details/53547955

https://www.findhao.net/easycoding/2070

http://www.cnblogs.com/xred/p/3898678.html

http://blog.csdn.net/lixiang201101/article/details/36531557

## route
```
ip route add 172.16.6.0/24 via 172.16.2.254 dev eth0
ip route del gw 172.16.2.254
ip route del 172.16.6.0/24 dev eth0
ip route

route add -net 172.16.6.0 netmask 255.255.255.0 gw 172.16.2.254 dev eth0
/* 增加一条网络172.16.6.0/24 经过172.16.2.254 eth0 */
/* -net增加网络 -host增加主机 netmask 子网掩码 gw 网关 dev 装置,设备,这里是你的网卡名*/
route del gw 172.16.2.254 /* 删除默认网关172.16.2.254 */
route del -net 172.16.86.0/24 /* 删除默认网络172.16.86.0 */
route /* 显示当前路由表 */
```
## install chrome
```
echo deb http://archive.ubuntu.com/ubuntu/ xenial-security multiverse >> /etc/apt/sources.list &&     
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list &&     
curl -O https://dl.google.com/linux/linux_signing_key.pub &&     
apt-key add linux_signing_key.pub && apt update &&     
apt install google-chrome-stable
```
## samba 
 ```
 [osmc]
    browsable = yes
    read only = no
    writable = yes
    guest only = yes
    guest ok = yes
    valid users = osmc
    map to guest = bad user
    path = /mnt/hdd
    create mask = 0666
    directory mask = 0777
 ```
## KVM
```
virsh qemu-agent-command ubuntu16.04 '{"execute":"guest-info"}'
virsh qemu-agent-command ubuntu16.04 '{"execute":"guest-shutdown"}'
virsh qemu-agent-command ubuntu16.04 '{"execute":"guest-exec","arguments":{"path":"ip","arg":["addr","add","5.5.5.5/24","dev","ens3"],"capture-output":true}}'


virsh list --all

virsh dumpxml xxx


virsh net-list
virsh net-autostart default
virsh net-start default
```

### qemu-img info
```sh
qemu-img info ubuntu-1604.qcow2

# backing image chain
qemu-img info --backing-chain SOM-device.qcow2

# rebase backing image, 其他修改合并到sn3
qemu-img rebase -b centos2_sn1.qcow2 centos2_sn3.qcow2
```
https://opengers.github.io/virtualization/kvm-libvirt-qemu-5/


```sh
qemu-img create -f qcow2 -b /home/automation/kvm-base-ubuntu-1604.qcow2 ubuntu-1604.qcow2
```

### merge 2 qco2
```sh
# cp test1-base new-master
# qemu-img rebase -b new-master test1-master-0
# qemu-img commit test1-master-0
Image committed.
# qemu-img info new-master 
```

### clone kvm base on base.qcow2 and base.xml

**device**
```sh
$ sudo qemu-img create -f qcow2 -F qcow2 -b base-device-12_4-tmpl.qcow2 mock_device_14_39.qcow2         
Formatting 'mock_device_14_39.qcow2', fmt=qcow2 size=4729536000 backing_file=base-device-12_4-tmpl.qcow2 cluster_size=65536 lazy_refcounts=off refcount_bits=16
$ virt-clone --connect=qemu:///system --original-xml /tmp/temp_device.xml -n Mock-device-14.39 --preserve-data -f mock_device_14_39.qcow2 --print-xml | sed s/4555/$sn_port/g | sed s/ovsname/ovs1412/g > /tmp/mock_device_14_39.xml

$ sudo vi /tmp/mock_device_14_39.xml
# add <target dev="vnet9"/> in <interface .../>

$ virsh define /tmp/mock_device_14_39.xml
Domain Mock-device-14.39 defined from /tmp/mock_device_14_39.xml
$ virsh start Mock-device-14.39
```
**Ubuntu**
```sh
$ sudo qemu-img create -f qcow2 -F qcow2 -b ./base/kvm-u1804-docker-base.qcow2 mock_ubuntu_14_40.qcow2
$ virsh dumpxml fan-ubuntu-14.59  > /tmp/fan-ubuntu-14.59.xml

$ virt-clone --connect=qemu:///system --original-xml /tmp/fan-ubuntu-14.59.xml -n Mock-Ubuntu-14.40 --preserve-data -f mock_ubuntu_14_40.qcow2 --print-xml > /tmp/mock_ubuntu_14_40.xml

$ virsh define /tmp/mock_ubuntu_14_40.xml
$  virsh start Mock-Ubuntu-14.40
```

```sh
base_xml=/home/automation/scripts/device.xml
base_disk=/var/lib/libvirt/images/clone/base-xtmv-12-1.qcow2
ovsname=watsng-vswitch

for i in {1..8}; do
    name=xtmv-$i
    port_prefix=ovs-

    new_disk=/var/lib/libvirt/images/clone/$name.qcow2
    sudo qemu-img create -f qcow2 -b $base_disk $new_disk
    sudo chown libvirt-qemu $new_disk

    sn_port=$((5000+i))
    virt-clone --connect=qemu:///system --original-xml $base_xml -n $name --preserve-data -f $new_disk --print-xml | sed s/4555/$sn_port/g | sed s/ovsname/$ovsname/g > /tmp/$name.xml
    python3 insert_ovs_port_name.py /tmp/$name.xml $port_prefix $i
    virsh define /tmp/$name.xml
    virsh start $name
done
```

Batch start
```sh
virsh list --all | grep ovs1412 | awk '{print $2}' | while read name; do virsh start $name; sleep 5; done
```
## Move kvm VM to a new host
```sh
1. copy VM's qcow2 disk image in /var/lib/libvirt/images
2. `virsh dumpxml VMNAME > /tmp/VMNAME.xml` and copy
3. on the destination host run `virsh define VMNAME.xml
```

## open vswitch

http://fishcried.com/2016-02-09/openvswitch-ops-guide/


```
ovs-vsctl show

# add bridge
ovs-vsctl add-br watsng-vswitch

# bind host port to bridge
ovs-vsctl add-port watsng-vswitch "eno2"
ovs-vsctl del-port watsng-vswitch "eno2"

# list ports/ifaces of bridge
ovs-vsctl list-ports br-tun
ovs-vsctl list-ifaces br-tun

# set port vlan
ovs-vsctl set port vnet1 tag=999
ovs-vsctl set port vnet1 trunks=10,999
ovs-vsctl set port vnet1 vlan_mode=access

# clear port vlan to default
ovs-vsctl remove port vnet1 tag 999
ovs-vsctl set port vnet1 tag=1

# list port config
ovs-vsctl list port vnet1

# open flow
ovs-ofctl show, dump-ports, dump-flows, add-flow, mod-flows, del-flows
ovs-ofctl dump-flows br-ex
ovs-ofctl show br-ex

# log
ovsdb-tools show-log -m

# datapath 
# statistics
ovs-dpctl show          
ovs-dpctl show -s
ovs-ofctl dump-ports br-ex

# mac addr table
ovs-appctl fdb/show br-ex

```
## brdige
```
brctl addbr br0         #添加bridge
brctl addif br0 eth0    #将br0和eth0绑定起来 
ifconfig eth0 xxx       #将eth0的IP设置为xxx


```
## /etc/network/interfaces
```
# The primary network interface
auto eno1
iface eno1 inet static
        address 10.254.254.254
        netmask 255.255.255.0
        up brctl addbr br-manage
        up brctl addif br-manage eno1
        down ip link set br-manage down
        down brctl delbr br-manage

auto eno2
iface eno2 inet static
        address 10.254.253.254
        netmask 255.255.255.0

auto br-manage
iface br-manage inet static
        address 10.138.40.13
        netmask 255.255.255.0
        network 10.138.40.0
        broadcast 10.138.40.255
        gateway 10.138.40.254
        dns-search example.net
        dns-nameservers 172.23.0.5 114.114.114.114
#       bridge_ports eno1
#       bridge_stp off
#       bridge_fd 0
#       bridge_maxwait 0


auto macvlan0
iface macvlan0 inet static
        pre-up ip link add link eth0 dev macvlan0 type macvlan mode bridge
        address 10.138.40.111
        netmask 255.255.255.0
        gateway 10.138.40.254
        dns-search example.net
        dns-nameservers 172.23.0.5 114.114.114.114
        post-down ip link del dev macvlan0


```
## ip / ifconfig
https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf
```
# Show network devices and configuration
ifconfig

ip addr show
ip link show

ip addr
ip addr show dev eth0

ip link
ip link show dev eth0
ip -s link

# Enable a network interface
ifconfig eth0 up
ip link set eth0 up
ifup eth0

# Set IP Address
ifconfig eth0 x.x.x.x
ip address add x.x.x.x dev eth0

ip address add x.x.x.x/x broadcast x.x.x.x dev eth0

# Delete an IP address
ip addr del x.x.x.x/x dev eth0

# Add alias interface
ip addr add x.x.x.x/x dev eth0 label eth0:1

# ARP
arp -a
ip neigh

# ip route
ip route add default via 192.168.1.1 dev eth0
ip route add 192.168.1.0/24 via 192.168.1.1
ip route add 192.168.1.0/24 dev eth0

ip route delete 192.168.1.0/24 vis 192.168.1.1

ip route get 192.168.1.5

ip route replace 192.168.1.0/24 dev eth0


```
## ssh tunnel
```
ssh -o StrictHostKeyChecking=no -N -D 0.0.0.0:xx 127.0.0.1 & 

ssh -R <local port>:<remote host>:<remote port> <SSH hostname>

在需要被访问的内网机器上运行： ssh -NR 1234:localhost:22 a.b.c.d
登录到a.b.c.d机器，使用如下命令连接内网机器：
ssh -p 1234 localhost

本机运行

ssh -L <local host>:<local port>:<remote host>:<remote port> -C -N <SSH hostname>

ssh -NL a.b.c.d:1234:www.google.com:80 a.b.c.d
此时，在浏览器里键入：http://a.b.c.d:1234，就会看到Google的页面了。
```

172.23.0.100 -> 10.138.14.60 -> 10.100.100.100

-- Localhost--> ----Server----> -- Gitlab Server -

```sh
# start a ssh tunnel on Localhost
ssh -L 2222:10.100.100.100:22 automation@10.138.14.60

# git clone
git clone ssh://git@localhost:2222/your_username/your_repo_name.git

# or
ssh -NL localhost:22:10.138.196.26:22 automation@10.138.14.60
```


## openssl
https://blog.csdn.net/gengxiaoming7/article/details/78505107
```
caKey.pem私钥 
sudo openssl genrsa -out ./demoCA/private/cakey.pem 2048

证书请求


自签署 ca-cert.pem
sudo openssl ca -config /xxx/openssl.cnf \
-out ./fireboxcert.pem -infiles ./fireboxreq.pem

openssl req -new -config /xxx/openssl.cnf \
-x509 -days 365 -subj "%s" -key ./demoCA/private/cakey.pem -out ./demoCA/cacert.pem
```

#### How to create a self-signed PEM file:
```
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
```

#### Extract Cert
```sh
openssl s_client -showcerts -verify 5 -connect stackexchange.com:443 < /dev/null

openssl x509 -in example.crt -noout -text
```

## Create Root CA (Done once)

## Create Root Key

**Attention:** this is the key used to sign the certificate requests, anyone holding this can sign certificates on your behalf. So keep it in a safe place!

```bash
openssl genrsa -des3 -out rootCA.key 4096
```

If you want a non password protected key just remove the `-des3` option


## Create and self sign the Root Certificate

```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
```

Here we used our root key to create the root certificate that needs to be distributed in all the computers that have to trust us.


#### Create a certificate (Done for each server)

This procedure needs to be followed for each server/appliance that needs a trusted certificate from our CA

##### Create the certificate key

```
openssl genrsa -out mydomain.com.key 2048
```

##### Create the signing  (csr)

The certificate signing request is where you specify the details for the certificate you want to generate.
This request will be processed by the owner of the Root key (you in this case since you create it earlier) to generate the certificate.

**Important:** Please mind that while creating the signign request is important to specify the `Common Name` providing the IP address or domain name for the service, otherwise the certificate cannot be verified.

I will describe here two ways to gener

##### Method A (Interactive)

If you generate the csr in this way, openssl will ask you questions about the certificate to generate like the organization details and the `Common Name` (CN) that is the web address you are creating the certificate for, e.g `mydomain.com`.

```
openssl req -new -key mydomain.com.key -out mydomain.com.csr
```

##### Method B (One Liner)

This method generates the same output as Method A but it's suitable for use in your automation :) .

```
openssl req -new -sha256 -key mydomain.com.key -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com" -out mydomain.com.csr
```

If you need to pass additional config you can use the `-config` parameter, here for example I want to add alternative names to my certificate.

```
openssl req -new -sha256 \
    -key mydomain.com.key \
    -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=mydomain.com" \
    -reqexts SAN \
    -config <(cat /etc/ssl/openssl.cnf \
        <(printf "\n[SAN]\nsubjectAltName=DNS:mydomain.com,DNS:www.mydomain.com")) \
    -out mydomain.com.csr
```


##### Verify the csr's content

```
openssl req -in mydomain.com.csr -noout -text
```

##### Generate the certificate using the `mydomain` csr and key along with the CA Root key

```
openssl x509 -req -in mydomain.com.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out mydomain.com.crt -days 500 -sha256
```

##### Verify the certificate's content

```
openssl x509 -in mydomain.com.crt -text -noout
```

##### Convert to pfk
```
openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt
```

## add ssh fingerprint of a target
```
ssh-keyscan -H 192.168.1.162 >> ~/.ssh/known_hosts
```
## 免密登录 
```
automation ALL=(ALL) NOPASSWD: ALL
```

## snap proxy
```
automation@ubuntu-1804:~$ sudo snap set system proxy.ftp=http://10.138.110.1:3128
automation@ubuntu-1804:~$ sudo snap set system proxy.https=http://10.138.110.1:3128
automation@ubuntu-1804:~$ sudo snap set system proxy.http=http://10.138.110.1:3128
```
```
sudo systemctl edit snapd.service
#Add in the following:

[Service]
Environment=http_proxy=http://proxy:port
Environment=https_proxy=http://proxy:port
Save then reload:

sudo systemctl daemon-reload
sudo systemctl restart snapd.service
```

## bash cli proxy
```sh
export http_proxy=http://10.138.110.1:3128
export https_proxy=http://10.138.110.1:3128
exprot no_proxy=localhost, 127.0.0.1, *.my.lan
```
put in `~/.bash_rc`  for current user 
put in `/etc/environment` All Users

## apt proxy
```sh
sudo vi /etc/apt/apt.conf.d/proxy.conf

Acquire::http::Proxy "http://10.138.110.1:3128;
Acquire::https::Proxy "http://10.138.110.1:3128;
```

## docker daemon

```sh
sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf

[Service]
Environment="HTTP_PROXY=http://10.138.110.1:3128"
Environment="HTTPS_PROXY=http://10.138.110.1:3128"
Environment="NO_PROXY=localhost,127.0.0.1"

sudo systemctl daemon-reload
sudo systemctl restart docker
```

## docker build / docker-compose
```sh
vi ~/.docker/config.json

{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://10.138.111.2:3128",
     "httpsProxy": "http://10.138.111.2:3128",
     "noProxy": "localhost,127.0.0.1"
   }
 }
}
```

## git proxy
```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080

撤销代理：
git config --global --unset http.proxy
git config --global --unset https.proxy

如果要查当前的代理配置，分别是：
git config --global --get http.proxy
git config --global --get https.proxy
```

## git reset remote url
```sh
$ git remote -v
origin  git@gitlab.example.net:wats-ng/docs.wiki.git (fetch)
origin  git@gitlab.example.net:wats-ng/docs.wiki.git (push)

$ git remote set-url origin git@10.138.196.26:wats-ng/docs.wiki.git

$ git remote -v
origin  git@10.138.196.26:wats-ng/docs.wiki.git (fetch)
origin  git@10.138.196.26:wats-ng/docs.wiki.git (push)
```

## shell if
```sh
[ -e file ] file存在
[ -d file ] file存在且是一个目录
[ -f file ] file存在且是一个文件
[ -h file ] file存在且是一个符号连接
[ -S file ] file存在且是一个套接字

[ -r file ] 可读
[ -w file ] 可写
[ -x file ] 可执行
[ -s file ] 大小不为0

[file1 –nt file2] file1 has been changed more recently than file2
[file1 –ot file2] file1比file2要老

[ -z string ] string长度为0
[ -n string ] string长度不为0 
[ string ] string长度不为0 

[sting1==string2] 2个字符串相同
[sting1!=string2] 2个字符串不相同
[string1<string2] sorts by str

[arg1 OP arg2] “OP”is one of –eq,-ne,-lt,-le,-gt or –ge



```