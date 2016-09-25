# RaspberryPi-Setup
Be Companion Computer for Pixhawk

Hardware:Raspberry Pi 3

OS:[RASPBIAN JESSIE LITE (2016-05-27)](https://www.raspberrypi.org/downloads/raspbian/)

Note:[INSTALLING OPERATING SYSTEM IMAGES](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)

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
sudo apt-get install rpi-update zsh git vim
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

##将公钥添加到GitHub上
```bash
ssh-keygen -t rsa -C "mengxz188@qq.com"     --在.ssh文件下产生公钥id_rsa.,pub 和私钥id_rsa
cd ~/.ssh 
ls -a            --可以在用户目录下(/home/mengxz)看到隐藏文件 .ssh 文件
cat id_rsa.pub     --复制公钥到github上
```
##Git
```bash
git clone git@github.com:AirForceUAV/pixController.git
git clone git@github.com:AirForceUAV/ObstacleAvoidance.git

git branch -a       --查看所有分支(本地分支 和 远程分支)
git checkout -b dev origin/dev    --关联本地分支与远程分支
git checkout dev     --切换分支

git add <file>
git commit -m 'note'
git push origin dev   --push 到远程分支

git pull            --从远程仓库pull最新代码

```

