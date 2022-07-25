<img width="870" alt="image" src="https://user-images.githubusercontent.com/637919/180819867-04e81762-5fdb-4607-b778-3e3ba775beef.png">

<img width="1440" alt="image" src="https://user-images.githubusercontent.com/637919/180819909-9ed8ee7e-2909-4f7d-885f-69d976bfd339.png">


### 先拿到一个bash

* bash

### 更新源并且更新ca

* apt update
* apt upgrade
* apt install ca-certificates

### 安装vim

* apt install vim

否则不方便搞事情


### 修改源

https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/


	# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
	deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

	# 预发布软件源，不建议启用
	# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
	# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse


### 镜像正常化一下

* unminimize


### 安装桌面大环境

参考资料：https://www.makeuseof.com/install-desktop-environment-gui-ubuntu-server/

* apt install ubuntu-desktop

### 本地与docker之间的X11链接化

	docker run -d --name wechat --ipc=host -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY 

	docker run -it --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix:rw bestwu/wechat

	docker run -it --env="DISPLAY=host.docker.internal:0" bestwu/wechat
	docker run -it --env="DISPLAY=host.docker.internal:0" bestwu/qq:latest
	docker run -it --env="DISPLAY=host.docker.internal:0" jess/firefox

	docker run -it --device /dev/kvm -p 50922:10022 -v /tmp/.X11-unix:/tmp/.X11-unix -e "DISPLAY=host.docker.internal:0" sickcodes/docker-osx:latest


	sudo docker run --rm  -it --shm-size=512m -p 6901:6901 -e VNC_PW=password kasmweb/desktop

	docker run -it --env="DISPLAY=host.docker.internal:0" centminmod/docker-ubuntu-vnc-desktop

参考资料：http://mamykin.com/posts/running-x-apps-on-mac-with-docker/

其实这种方式不好，也不安全，但是简单


### 试一下

export DISPLAY=host.docker.internal:0


### 安装flatpak

add-apt-repository ppa:flatpak/stable
apt update
apt install flatpak

### 安装插件

apt install gnome-software-plugin-flatpak

### 添加好源
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

### 重启整个docker

### 安装bottles
https://flathub.org/apps/details/com.usebottles.bottles

add-apt-repository ppa:mozillateam/ppa


apt-get install software-properties-common

### How can I launch gnome-terminal remotely on my headless server? (fails to launch over X11 forwarding)

https://unix.stackexchange.com/questions/407831/how-can-i-launch-gnome-terminal-remotely-on-my-headless-server-fails-to-launch

docker run -it --rm --env="DISPLAY=host.docker.internal:0" quizzical_feistel xeyes
docker run -it --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix:rw 6f57225a5494b11b67208be594c9a35333399087121f91bb263cbb532ada824f gnome-calculator


### 容器镜像化

6f57225a5494

docker ps -a

docker commit -m "ubuntu desktop" -a "lemonhall" 6f57225a5494 lemonhall/ubuntu_desktop

docker run -it --rm --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix lemonhall/ubuntu_desktop Xsession

这就是可以的

docker run -it --rm --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix lemonhall/ubuntu_desktop gnome-calculator

docker run -it --rm --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix lemonhall/ubuntu_desktop bash
### 修改gdm3

	root@c03f7c87135b:/etc/gdm3# cat custom.conf
	# GDM configuration storage
	#
	# See /usr/share/gdm/gdm.schemas for a list of available options.

	[daemon]
	# Uncomment the line below to force the login screen to use Xorg
	WaylandEnable=false

	# Enabling automatic login
	AutomaticLoginEnable = true
	AutomaticLogin = root

	# Enabling timed login
	#  TimedLoginEnable = true
	#  TimedLogin = user1
	#  TimedLoginDelay = 10

	[security]
	DisallowTCP=false

	[xdmcp]
	Enable = true

	[chooser]

	[debug]
	# Uncomment the line below to turn on debugging
	# More verbose logs
	# Additionally lets the X server dump core if it crashes
	#Enable=true

### 解决dbus报错的第一阶段

dbus-launch
export $(dbus-launch)

### 解决gnome-session 报错的第二阶段

https://gist.github.com/BoQsc/d2191f4711cdc55a1c3787ffc06c3240

mkdir -p /run/dbus
service dbus start
dbus-daemon --system

启动dbus

export XDG_SESSION_TYPE=x11
export GDK_BACKEND=x11
gnome-session

这样会报错

输出两个系统变量

startx /usr/bin/gnome-session

   17  mkdir -p /run/dbus
   18  service dbus start
   19  dbus-launch
   20  export $(dbus-launch)
   21  dbus-daemon --system
   22  export XDG_SESSION_TYPE=x11
   23  export GDK_BACKEND=x11

docker run -it --rm --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix lemonhall/ubuntu_desktop bash

###  解决报错
bwrap: Creating new namespace failed: Operation not permitted
error: ldconfig failed, exit status 256

https://github.com/ocaml/opam/issues/4327

--privileged

docker run -it --rm --privileged --env="DISPLAY=host.docker.internal:0" -v /tmp/.X11-unix:/tmp/.X11-unix lemonhall/ubuntu_desktop bash

### 解决目录问题
flatpak override com.usebottles.bottles --filesystem=/root/
flatpak override com.usebottles.bottles --nofilesystem=/root/

