# RaspberryPi For Mission Control
Be Mission Control Panle

Hardware: Raspberry Pi 3 B

OS: [RASPBIAN JESSIE WITH PIXEL
 (2016-09-23)](https://www.raspberrypi.org/downloads/raspbian/)

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
sudo apt-get install rpi-update zsh git vim tmux
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
ssh-keygen -t rsa -C "YOUR_EMAIL"
cd ~/.ssh 
cat id_rsa.pub
```

## Golang
Download latest ARM version golang from 
[Golang Release Page](https://golang.org/dl/)

```bash
scp /Users/YogurtShen/Downloads/go1.7.1.linux-armv6l.tar.gz pi@192.168.1.22:~

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
go get github.com/mdp/qrterminal
go get github.com/satori/go.uuid
go build
./DataProxy mc
```

## Electron
Download latest ARM version electron from 
[Electron Release Page](https://github.com/electron/electron/releases)

```bash
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs
```

```bash
scp /Users/YogurtShen/Downloads/electron-v1.4.3-linux-arm.zip pi@192.168.1.22:~

unzip electron-v1.4.3-linux-arm.zip

./electron
```

```bash
git clone git@github.com:AirForceUAV/MissionControl.git
~/electron ~/MissionControl
```

##Supervisor

```bash
sudo apt-get install python-setuptools python-dev build-essential
sudo easy_install supervisor
sudo -i
echo_supervisord_conf > /etc/supervisord.conf
su pi
sudo mkdir /etc/supervisor
sudo vim /etc/supervisord.conf
```

Add two lines at top

```bash
[include]
files = /etc/supervisor/*.conf
```

```bash
sudo vim /etc/supervisor/DataProxy.conf
```

Add below content

```
[program:DataProxy]
command=/home/pi/Go/src/DataProxy/DataProxy mc
autostart=true
autorestart=true
startsecs=5
startretries=10
user=pi
stdout_logfile=/var/log/DataProxy.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/var/log/DataProxy.log
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
environment=HOME="/home/pi", USER="pi"
```

```bash
sudo supervisord -c /etc/supervisord.conf
```

```bash
sudo vim /etc/rc.local
```

Add blow content before `exit 0`

```bash
sudo /usr/local/bin/supervisord -c /etc/supervisord.conf
```