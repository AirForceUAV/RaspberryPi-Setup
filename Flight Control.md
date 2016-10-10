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
sudo apt-get install python-dev python-pip minicom
```

```python
sudo pip install dronekit==2.5.0 dronekit-sitl==3.0.1
sudo pip install paho-mqtt
sudo pip install threadpool
sudo pip install pynmea2
sudo pip install apscheduler
```
[dronekit](http://python.dronekit.io/about/index.html) [dronekit-sitl](https://github.com/dronekit/dronekit-sitl)
[paho-mqtt](https://pypi.python.org/pypi/paho-mqtt/1.1) [threadpool](https://pypi.python.org/pypi/threadpool/1.2.6)
[pynmea2](https://github.com/Knio/pynmea2) [apscheduler](https://apscheduler.readthedocs.io/en/latest/userguide.html)

## Add Deploy Key to Github Repo

```bash
ssh-keygen -t rsa -C "YOUR_EMAIL"
cd ~/.ssh 
cat id_rsa.pub
```

## Enable serial port on Pi3

### Enable

Edit `config.txt`

```bash
sudo nano /boot/config.txt
```

and add the line(at the bottom)

`enable_uart=1`

`core_freq=250`

### Serial Aliases

To summarise the ports on a Raspberry Pi 3 and be crystal clear:

/dev/ttyAMA0 -> Bluetooth

/dev/ttyS0   -> GPIO pins 14(Tx) and 15(Rx).

```bash
ls -l /dev
```

and you will see something like this:   

serial0->ttyS0  

serial1->ttyAMA0

### Disabling the Console

If you are using the serial port for anything other than the console you need to disable it.

```bash
sudo systemctl stop serial-getty@ttyS0.service

sudo systemctl disable serial-getty@ttyS0.service

```

You also need to remove the console from the cmdline.txt. If you edit this with:

```bash
sudo nano /boot/cmdline.txt
```

then remove the line: `console=serial0,115200` and save and reboot for changes to take effect.

### Swapping the Serial Ports on Pi3

To use add the following line to the /boot/config.txt

```bash
sudo nano /boot/config.txt
```

and add:

`dtoverlay=pi3-miniuart-bt`

Finally,you can check that it has worked by:

```bash
ls -l /dev
```

and you’ll see something like this:

serial0->ttyAMA0

serial1->ttyS0



## Git command
```bash
git clone git@github.com:AirForceUAV/{$resposity}.git

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
scp /Users/YogurtShen/Downloads/go1.7.1.linux-armv6l.tar.gz pi@192.168.1.20:~

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
./DataProxy fc
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
src tar zxvf golang.org.x.net.tar.gz
```

##Supervisor

```bash
sudo apt-get install python-setuptools python-dev build-essential
sudo easy_install supervisor
```


## FFMPEG

```bash
sudo apt-get install libx264-142 libx264-dev
git clone --depth=1 git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg
./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree
make -j4
sudo make install
```

Plug in the USB camera, Create below shell script, named `Video_USB.sh` as below

```bash
# !/bin/bash

ffmpeg \
-f v4l2 -framerate 25 \
-video_size 1280x720 -i /dev/video0 \
-f flv \
rtmp://video.airforceuav.com:1935/live/livestream
```

```bash
tmux
bash video.sh
Ctrl+B D
```

Or use Raspberry Pi camera module

```bash
sudo modprobe bcm2835_v4l2
```

```bash
sudo raspivid -fl -t 0 -w 1280 -h 720 -b 1200000 -fps 15 \
    -pf baseline -o - | ffmpeg -f h264 -i - \
    -c copy -an -f flv -y rtmp://video.airforceuav.com:1935/live/livestream
```
