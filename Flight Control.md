# RaspberryPi For Flight Control
Be Companion Computer for Pixhawk or ACE ONE

## For pixController

Install common package
```bash
sudo apt-get install python-dev python-pip
```
```python
sudo pip install paho-mqtt
sudo pip install pynmea2
sudo pip install apscheduler
```

Install package for pixhawk
```python
sudo pip install dronekit==2.5.0 dronekit-sitl==3.0.1
```
[dronekit](http://python.dronekit.io/about/index.html) [dronekit-sitl](https://github.com/dronekit/dronekit-sitl)
[paho-mqtt](https://pypi.python.org/pypi/paho-mqtt/1.1)
[pynmea2](https://github.com/Knio/pynmea2) [apscheduler](https://apscheduler.readthedocs.io/en/latest/userguide.html)

## Enable serial port on Pi3

### Enable "/dev/ttyAMA0"

Edit `config.txt`

```bash
sudo nano /boot/config.txt
```

and add the line(at the bottom)

`enable_uart=1`

`core_freq=250`

### Serial Aliases

To summarise the ports on a Raspberry Pi 3 and be crystal clear:

/dev/ttyAMA0 --> Bluetooth

/dev/ttyS0   --> GPIO pins 14(Tx) and 15(Rx).

```bash
ll /dev
```

and you will see something like this:   

serial0->ttyS0  

serial1->ttyAMA0

### Disable the Console

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
ll /dev
```

and you’ll see something like this:

serial0->ttyAMA0

serial1->ttyS0

## Rename serial port on Pi3

### Display device info
```bash
sudo udevadm info -a -p $(udevadm info -q path -n /dev/ttyUSB0)
```

```bash
sudo vim /etc/udev/rules.d/10-local.rules
```

### Add variation,Like:

SUBSYSTEMS=="usb-serial",DRIVERS=="cp210x",ATT{port_number}=="0",SYMLINK="GPS"

SUBSYSTEMS=="usb-serial",DRIVERS=="ftdi_sio",ATTRS{latency_timer}=="1",ATTRS{port_number}=="0",SYMLINK="compass"

SUBSYSTEMS=="usb-serial",DRIVERS=="ch341-uart",ATT{port_number}=="0",SYMLINK="IMU"

Take effect
```bash 
sudo service udev restartT
```
or  replug (recommand)

### Display whether is successful
```bash
ll /dev
```
You will see someting like:

GPS -> ttyUSB0

compass -> ttyUSB1

IMU -> ttyUSB2

## ProtoBuf

[Download ProtoBuf](https://github.com/google/protobuf/releases)

To build and install the Protocol Buffer compiler (protoc) execute the following:
```bash
./configure
make
make check
sudo make install
sudo ldconfig # refresh shared library cache.
protoc --version
```
To build and install the Protocol Buffer for python (protobuf):
```bash
cd python
sudo python setup.py install
pip show protobuf
```
Compile .proto ,ouput for python

protoc -I=$SRC_DIR --python_out=$DST_DIR $SRC_DIR/addressbook.proto

example:
```bash
protoc -I=./ --python_out=./ addressbook.proto
```
This generates addressbook_pb2.py in your specified destination directory.
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

Or use Raspberry Pi camera module(Recommended)

```bash
sudo modprobe bcm2835_v4l2
```

```bash
sudo raspivid -fl -t 0 -w 1280 -h 720 -b 1200000 -fps 15 \
    -pf baseline -o - | ffmpeg -f h264 -i - \
    -c copy -an -f flv -y rtmp://video.airforceuav.com:1935/live/livestream
```

[Online RTMP Player](https://www.hlsplayer.net/rtmp-player)
