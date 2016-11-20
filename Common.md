# RaspberryPi Common
For Both Flight Control & Mission Control

Hardware: Raspberry Pi 3 B

OS: [RASPBIAN JESSIE LITE (2016-09-23)](https://www.raspberrypi.org/downloads/raspbian/)

Note: [INSTALLING OPERATING SYSTEM IMAGES](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

Recommend using windows to write SD card for convenience

## Common

Using

```bash
sudo raspi-config
```

to change locale/time etc, a reboot is required.

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
sudo apt-get install rpi-update zsh git vim tmux openssh-server build-essential gstreamer1.0 gstreamer1.0-libav
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

## Config a Wi-Fi dongle(Deprecated)

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

## Add Deploy Key to Github Repo

```bash
ssh-keygen -t rsa -C "YOUR_EMAIL"
cd ~/.ssh 
cat id_rsa.pub
```

## Git command
```bash
git clone git@github.com:AirForceUAV/{$repo}.git

git checkout -b develop origin/develop    --关联本地分支与远程分支
git branch -a    

git add <file>
git commit -m 'note'
git push origin develop   --push 到远程分支

git pull            --从远程仓库pull最新代码

```

## Golang
Download latest ARM version golang from 
[Golang Release Page](https://golang.org/dl/)

```bash
scp /Users/YogurtShen/Downloads/go1.7.3.linux-armv6l.tar.gz pi@192.168.1.20:~

sudo tar -C /usr/local -xzf go1.7.3.linux-armv6l.tar.gz

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
go get github.com/mdp/qrterminal
go get github.com/satori/go.uuid
go get github.com/paypal/gatt
go get github.com/tarm/serial
go get github.com/Sirupsen/logrus
go get github.com/spf13/viper
go get github.com/eclipse/paho.mqtt.golang
go build
sudo ./DataProxy {$target}
```

if it shows below: 

```bash
src go get github.com/eclipse/paho.mqtt.golang

# cd .; git clone https://go.googlesource.com/net /home/pi/Go/src/golang.org/x/net
Cloning into '/home/pi/Go/src/golang.org/x/net'...
fatal: unable to access 'https://go.googlesource.com/net/': Could not resolve host: go.googlesource.com
package golang.org/x/net/websocket: exit status 128
```

Then

```bash
scp /Users/YogurtShen/Downloads/golang.org.x.net.tar.gz  pi@192.168.1.20:~/Go/src
rm -rf ~/Go/src/golang.org
tar zxvf golang.org.x.net.tar.gz
```

##Supervisor

```bash
sudo apt-get install python-setuptools python-dev build-essential
sudo easy_install supervisor
```
