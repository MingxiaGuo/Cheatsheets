# Linux

scp /path/to/file user@server:/path/to/destination # Copy file from local to server

df -h # Check the amount of free space

sudo ufw status # Check status
sudo ufw allow from remote_IP_address to any port 3306 # Allow external ip to access port

scp user@remote_host:remote_file local_file # download: remote -> local
scp local_file user@remote_host:remote_file # upload: local -> remote

sudo -s # Log as root

cat /proc/<process_id>/maps   # Show the current virtual memory usage of a Linux process

ip r # Display ip of the server

lsof -i :9000 # List process running on port 9000

journalctl -u minio.service -n 100 --no-pager # List last 100 logs for specific service

sudo resize2fs /dev/disk/by-id/scsi-0DO_example # Resize volume

ps -ax | grep myprocessname # Search processes
kill -9 PROCESS_ID # Kill process PID

## basic

```sh
passwd   # let you change your password
useradd gmx # create a user account called gmx, and add the account to the group，通过命令创建的用户在/etc/passwd文件里的， 组的信息我们放在/etc/group文件中
passwd gmx   # change password for account gmx
useradd -g <>
cat <path/file_name>  # output context of file
cd                    # change directory
cd .                  #  切换到当前目录
cd ..                 #切换到上一级目录
ls                    # list files and 
ls -l                 # 用列表的方式列出文件
ls -la                # 才能看到，a就是all
chmod                 #改变权限 change mode
chown                 # 改变所属用户 change owner
chgrp                 # 改变所属组 change group
rpm -i jdk-XXX_linux-x64_bin.rpm # Install software on CentOS , -i 表示Install
dpkg -i jdk-XXX_linux-x64_bin.deb # Install software on Ubuntu , 
rpm -qa       # 查看软件列表在CentOS -q:query , -a:all
dpkg -l       # 查看软件列表在Ubuntu      -l:list
rpm -qa | grep jdk    # | 是管道，用于连接两个程序，grep支持正则表达式
dpkg -l | grep jdk
rpm -qa | more #将很长的结果分页展示出来。这样你就可以一个个来找了，more是分页后只能往后翻页，翻到最后一页自动结束返回命令行，需要输入q返回命令行，q就是quit
rpm -qa | less #将很长的结果分页展示出来。这样你就可以一个个来找了，less是往前往后都能翻页，需要输入q返回命令行，q就是quit
dpkg --remove # 卸载软件
yum search jdk # 搜索想要安装的软件 CentOS下的软件管家yum
apt-cache search jdk# 搜索想要安装的软件 Ubuntu下的软件管家apt
yum install java-11-openjdk.x86_64 # 安装软件
apt-get install openjdk-9-jdk  # 安装软件
yum erase java-11-openjdk.x86_64 # 卸载软件
apt-get purge openjdk-9-jdk # 卸载软件
wget <link> # 从网上下载文件
./filename  #运行这个程序。如果放在PATH里设置的路径下面，就不用./了，直接输入文件名就可以运行，Linux会帮你找
nohup command >out.file 2>&1 & # 后台运行
ps -ef |grep 关键字  |awk '{print $2}'|xargs kill -9  #关闭进程
mkdir upper lower merged work # 同时创建四个文件 upper lower merged work
scp
ssh
ps -ef # 列出所有正在运行的程序
tar xvzf jdk-XXX_linux-x64_bin.tar.gz #解压文件jdk-XXX_linux-x64_bin.tar.gz 

export JAVA_HOME=/root/jdk-XXX_linux-x64 # 配置环境变量，export命令仅在当前命令行的会话中管用，一旦退出重新登录进来，就不管用 
export PATH=$JAVA_HOME/bin:$PATH
```

```sh
[root@deployer ~]# useradd -h
Usage: useradd [options] LOGIN
       useradd -D
       useradd -D [options]


Options:
  -g, --gid GROUP               name or ID of the primary group of the new account
```

通过命令创建的用户在**/etc/passwd**文件里的，组的信息我们放在**/etc/group**文件中

首先是用户名，x的地方是密码，接下来是用户ID和组ID。/root和/home/cliu8分别是root用户和cliu8用户的主目录。/bin/bash的位置是用于配置登录后的默认交互命令行

```sh
# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
......
cliu8:x:1000:1000::/home/cliu8:/bin/bash

# cat /etc/group
root:x:0:
......
cliu8:x:1000:
```

第一个字段的第一个字符是**文件类型**，如果是“-”，表示普通文件；如果是d，就表示目录。第一个字段剩下的9个字符是**模式**，其实就是**权限位**（access permission bits）。3个一组，每一组rwx表示“读（read）”“写（write）”“执行（execute）”。如果是字母，就说明有这个权限；如果是横线，就是没有这个权限。这三组分别表示文件所属的用户权限、文件所属的组权限以及其他用户的权限。如果想改变权限，可以使用命令chmod 711 hosts。第二个字段是**硬链接**（hard link）**数目**，这个比较复杂，讲文件的时候我会详细说。第三个字段是**所属用户**，第四个字段是**所属组**。第五个字段是文件的大小，第六个字段是**文件被修改的日期**，最后是**文件名**。你可以通过命令chown改变所属用户，chgrp改变所属组。

```
# ls -l
drwxr-xr-x 6 root root    4096 Oct 20  2017 apt
-rw-r--r-- 1 root root     211 Oct 20  2017 hosts
```

Windows上的软件管家会有一个统一的服务端，来保存这些软件，但是我们不知道服务端在哪里。而Linux允许我们配置从哪里下载这些软件的，地点就在配置文件里面。

对于CentOS来讲，配置文件在/etc/yum.repos.d/CentOS-Base.repo里。

```
[base]
name=CentOS-$releasever - Base - 163.com
baseurl=http://mirrors.163.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7
```

对于Ubuntu来讲，配置文件在/etc/apt/sources.list里。

```
deb http://mirrors.163.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ xenial-backports main restricted universe multiverse
```

**其实无论是先下载再安装，还是通过软件管家进行安装，都是下载一些文件，然后将这些文件放在某个路径下，然后在相应的配置文件中配置一下。**例如，在Windows里面，最终会变成C:\Program Files下面的一个文件夹以及注册表里面的一些配置。对应Linux里面会放的更散一点。例如，主执行文件会放在/usr/bin或者/usr/sbin下面，其他的库文件会放在/var下面，配置文件会放在/etc下面。

export命令仅在当前命令行的会话中管用，一旦退出重新登录进来，就不管用了，有没有一个地方可以像Windows里面可以配置永远管用呢？

在当前用户的默认工作目录，例如/root或者/home/cliu8下面，有一个.bashrc文件，这个文件是以点开头的，这个文件默认看不到，需要ls -la才能看到，a就是all。每次登录的时候，这个文件都会运行，因而把它放在这里。这样登录进来就会自动执行。当然也可以通过source .bashrc手动执行。

要编辑.bashrc文件，可以使用文本编辑器vi，也可以使用更加友好的vim。如果默认没有安装，可以通过yum install vim及apt-get install vim进行安装。

Linux不是根据后缀名来执行的。它的执行条件是这样的：只要文件有x执行权限，都能到文件所在的目录下，通过./filename运行这个程序。当然，如果放在PATH里设置的路径下面，就不用./了，直接输入文件名就可以运行了，Linux会帮你找。

这是**Linux执行程序最常用的一种方式，通过shell在交互命令行里面运行**。

这样执行的程序可能需要和用户进行交互，例如允许让用户输入，然后输出结果也打印到交互命令行上。这种方式比较适合运行一些简单的命令，例如通过date获取当前时间。这种模式的缺点是，一旦当前的交互命令行退出，程序就停止运行了。

这样显然不能用来运行那些需要“永远“在线的程序。

**Linux运行程序的第二种方式，后台运行**。

这个时候，我们往往使用nohup命令。这个命令的意思是no hang up（不挂起），也就是说，当前交互命令行退出的时候，程序还要在。

当然这个时候，程序不能霸占交互命令行，而是应该在后台运行。最后加一个&，就表示后台运行。

另外一个要处理的就是输出，原来什么都打印在交互命令行里，现在在后台运行了，输出到哪里呢？输出到文件是最好的。

最终命令的一般形式为nohup command >out.file 2>&1 &。这里面，“1”表示文件描述符1，表示标准输出，“2”表示文件描述符2，意思是标准错误输出，“2>&1”表示标准输出和错误输出合并了。合并到哪里去呢？到out.file里。

那这个进程如何关闭呢？我们假设启动的程序包含某个关键字，那就可以使用下面的命令。

```
ps -ef |grep 关键字  |awk '{print $2}'|xargs kill -9
```

从这个命令中，我们多少能看出shell的灵活性和精巧组合。

其中ps -ef可以单独执行，列出所有正在运行的程序，grep上面我们介绍过了，通过关键字找到咱们刚才启动的程序。

awk工具可以很灵活地对文本进行处理，这里的awk '{print $2}'是指第二列的内容，是运行的程序ID。我们可以通过xargs传递给kill -9，也就是发给这个运行的程序一个信号，让它关闭。如果你已经知道运行的程序ID，可以直接使用kill关闭运行的程序。

Linux也有相应的服务，这就是**程序运行的第三种方式，以服务的方式运行**。例如常用的数据库MySQL，就可以使用这种方式运行。

例如在Ubuntu中，我们可以通过apt-get install mysql-server的方式安装MySQL，然后通过命令systemctl start mysql启动MySQL，通过systemctl enable mysql设置开机启动。之所以成为服务并且能够开机启动，是因为在**/lib/systemd/system**目录下会创建一个**XXX.service**的配置文件，里面定义了如何启动、如何关闭。

在CentOS里有些特殊，MySQL被Oracle收购后，因为担心授权问题，改为使用MariaDB，它是MySQL的一个分支。通过命令yum install mariadb-server mariadb进行安装，命令systemctl start mariadb启动，命令systemctl enable mariadb设置开机启动。同理，会在/usr/lib/systemd/system目录下，创建一个XXX.service的配置文件，从而成为一个服务。

systemd的机制十分复杂，这里咱们不讨论。如果有兴趣，你可以自己查看相关文档。

## network

```
sudo ifconfig utun3 mtu 1354
```

## systemctl

systemctl may be used to introspect and control the state of the "systemd" system and service manager.

systemctl cheatsheet: https://access.redhat.com/sites/default/files/attachments/12052018_systemd_6.pdf

```bash
systemctl status service   # See if service is running/enabled
systemctl stop service     # Stop a running service
systemctl start service    # Start a service， systemctl start mysql
systemctl restart service  # Restart a running service
systemctl enable service   # Enable a service to start on boot， systemctl enable mysql
systemctl disable service  # Disable service--won’t start at boot
```

## journalctl

VIEWING LOG MESSAGES

```bash
journalctl Show all collected log messages
journalctl -u network.service See network service messages
journalctl -f Follow messages as they appear
journalctl -k Show only kernel messages
```

## Change or Set Password

```bash
root@test-gmx-stunnel:~# passwd
New password:
Retype new password:
passwd: password updated successfully
root@test-gmx-stunnel:~#
```

## Vi

### 快速移动光标

移动光标至行首：shift + 6；Home键=Fn+左方向

移动光标至行尾: shift + 4；End键=Fn+右方向

向前移动一个单词:

向后移动一个单词

## tmux

```.sh
tmux 

tmux new -s  <session-name>

tmux attach 

tmux attach -t<session-number>/<session-name>

tmux kill-session -t <session-number>/<session-name>

Ctrl+b d：分离当前会话 / tmux detach
```
