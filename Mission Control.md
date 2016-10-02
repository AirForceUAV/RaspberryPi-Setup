# RaspberryPi For Mission Control
Be Mission Control Panle

Hardware: Raspberry Pi 3 B

OS: [RASPBIAN JESSIE WITH PIXEL
 (2016-09-23)](https://www.raspberrypi.org/downloads/raspbian/)

Note: [INSTALLING OPERATING SYSTEM IMAGES](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

Recommend using windows to write SD card

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

```bash
sudo reboot
```

## Add Deploy Key to Github Repo

```bash
ssh-keygen -t rsa -C "yogurtshen@gmail.com"
cd ~/.ssh 
cat id_rsa.pub
```

## Electron
Download latest ARM version electron from 
[Electron Release Page](https://github.com/electron/electron/releases)

```bash
scp /Users/YogurtShen/Downloads/electron-v1.4.2-linux-arm.zip pi@192.168.1.16:~

unzip electron-v1.4.2-linux-arm.zip
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
./DataProxy mc
```