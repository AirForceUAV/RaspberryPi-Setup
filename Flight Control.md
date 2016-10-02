# RaspberryPi For Flight Control
Be Companion Computer for Pixhawk or ACE ONE

Hardware: Raspberry Pi 3 B

OS: [RASPBIAN JESSIE LITE (2016-09-23)](https://www.raspberrypi.org/downloads/raspbian/)

Note: [INSTALLING OPERATING SYSTEM IMAGES](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

Recommend using windows to write SD card for convenience

## Common

Using

```bash
sudo raspi-config
```

to change locale/time etc.

```bash
sudo nano /etc/apt/sources.list
```

Replace the original address with USTC mirror

```bash
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free rpi
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free rpi
```

```bash
sudo apt-get update
```

```bash
sudo apt-get upgrade
```

```bash
sudo apt-get install rpi-update zsh git vim uuid-runtime tmux
```

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Then enter password to change to zsh or you need

```bash
chsh -s /bin/zsh
``` 

and reboot later

```bash
sudo rpi-update
```

```
sudo reboot
```

## Config a Wi-Fi dongle

```bash
sudo nano /etc/network/interfaces
```

modify content like below:

```bash
auto lo
iface lo inet loopback
iface eth0 inet dhcp
auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid “YOUR SSID”
wpa-psk “YOUR PASSWORD”
```

```bash
sudo /etc/init.d/networking restart
```

Then

```bash
ifconfig
```

to check wlan0's IP address

## For pixController
```bash
sudo apt-get install python-dev python-pip git vim
```

```
sudo pip install paho-mqtt pymavlink mavproxy dronekit dronekit-sitl threadpool
sudo pip install dronekit-sitl -UI
sudo pip install --pre azure
```
[Azure python SDK](https://github.com/Azure/azure-sdk-for-python)

## Add Deploy Key to Github Repo

```bash
ssh-keygen -t rsa -C "mengxz188@qq.com"
cd ~/.ssh 
cat id_rsa.pub
```

##Git
```bash
git clone git@github.com:AirForceUAV/pixController.git
git clone git@github.com:AirForceUAV/ObstacleAvoidance.git

git branch -a       --查看所有分支(本地分支 和 远程分支)
git checkout -b develop origin/develop    --关联本地分支与远程分支
git checkout develop     --切换分支

git add <file>
git commit -m 'note'
git push origin develop   --push 到远程分支

git pull            --从远程仓库pull最新代码

```

## Golang
Download latest ARM version golang from 
[Golang Release Page](https://golang.org/dl/)

```bash
scp /Users/YogurtShen/Downloads/go1.7.1.linux-armv6l.tar.gz pi@192.168.1.16:~

sudo tar -C /usr/local -xzf go1.7.1.linux-armv6l.tar.gz

sudo vim .zshrc
```

Add two lines at bottom

```bash
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/Go
```

```bash
source .zshrc
```

Use below command to test

```bash
go version
echo $GOPATH
```

Then create corresponding dirs

```bash
cd
mkdir Go
cd Go/
mkdir src pkg bin
cd src
git clone git@github.com:AirForceUAV/DataProxy.git
cd DataProxy
git checkout -b develop origin/develop
go get github.com/eclipse/paho.mqtt.golang
go build
./DataProxy fc
```
