# RaspberryPi Common
For Both Flight Control & Mission Control

Hardware: Raspberry Pi 3 B

OS: [RASPBIAN JESSIE LITE (2016-11-25)](https://www.raspberrypi.org/downloads/raspbian/)

Note: [INSTALLING OPERATING SYSTEM IMAGES](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

Recommend using windows to write SD card for convenience

## Common

Using `sudo raspi-config` to change locale/time & enable ssh etc, a reboot is required.

Using `sudo nano /etc/apt/sources.list` to replace the original address with USTC mirror, list below:

```bash
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
```

Update the OS

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt full-upgrade
```

Install some packages

```bash
sudo apt-get install rpi-update git vim tmux gstreamer1.0 gstreamer1.0-libav zsh
```
//beanstalkd openssh-server build-essential

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Then enter password to change to zsh or you need

```bash
chsh -s /bin/zsh
sudo reboot
``` 

Update the firmware

```bash
sudo rpi-update
sudo reboot
sudo apt-get autoremove
```

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
scp /Users/YogurtShen/Downloads/go1.7.4.linux-armv6l.tar.gz pi@192.168.1.20:~

sudo tar -C /usr/local -xzf go1.7.4.linux-armv6l.tar.gz

sudo vim .zshrc
```

Add two lines at bottom

```bash
export GOPATH=$HOME/Go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
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
git checkout develop
go build
beanstalkd -V -l 127.0.0.1 -p port(fc:2222 or mc:3333)
sudo ./DataProxy start -t (fc or mc)
```

```bash
go get github.com/kadekcipta/beanwalker
cd github.com/kadekcipta/beanwalker
go install
beanwalker -h localhost -p port(fc:2222 or mc:3333)
```

##Supervisor

```bash
sudo apt-get install python-setuptools python-dev build-essential
sudo easy_install supervisor
```
