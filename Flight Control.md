=========================================================
# RaspberryPi For Flight Control
=========================================================
[![Build Status](https://travis-ci.org/google/protobuf.svg?branch=master)](https://github.com/AirForceUAV/Unicorn)

Be Companion Computer for FlightController
## Hardware

UAV:HEX4,HEX6,Copter-550,Copter-600,AF-25B

Computer:Pi3 and MCU

FlightController:pixhawk, ACE_ONE, Naza-m v2, MicroBeast

[IMU](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.sSd9zC&id=45727369640&_u=n9i6v3oce6d):MPU9250 AHRS

[Compass](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.sSd9zC&id=44445395508&_u=n9i6v3ob5ab):HCM365B

[GPS](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.sSd9zC&id=17669339623&_u=n9i6v3o34c9):NEO-M8N

[Baro](https://detail.tmall.com/item.htm?id=41281679152&spm=a1z09.2.0.0.zyoHfd&_u=n9i6v3o6702):GY-63 MS5611-01BA03

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
[dronekit](http://python.dronekit.io/about/index.html)
[dronekit-sitl](https://github.com/dronekit/dronekit-sitl)
[paho-mqtt](https://pypi.python.org/pypi/paho-mqtt/1.1)
[pynmea2](https://github.com/Knio/pynmea2)
[apscheduler](https://apscheduler.readthedocs.io/en/latest/userguide.html)

## Enable serial port on Pi3

### Enable /dev/ttyAMA0

Edit `config.txt`

```bash
sudo nano /boot/config.txt
```

and add the line(at the bottom)

`enable_uart=1`

`core_freq=250`

### Serial Aliases

To summarise the ports on a Raspberry Pi 3 and be crystal clear:
```
/dev/ttyAMA0 --> Bluetooth
/dev/ttyS0   --> GPIO pins 14(Tx) and 15(Rx).
```
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
Edit *.rules
```bash
sudo vim /etc/udev/rules.d/10-local.rules
```

### Add variation,Like:

    SUBSYSTEMS=="usb-serial",DRIVERS=="cp210x",ATT{port_number}=="0",SYMLINK="compass"

    SUBSYSTEMS=="usb-serial",DRIVERS=="ftdi_sio",ATTRS{latency_timer}=="1",ATTRS{port_number}=="0",SYMLINK="sbus"

    SUBSYSTEMS=="usb-serial",DRIVERS=="ch341-uart",ATT{port_number}=="0",SYMLINK="IMU"

Take effect
```bash 
sudo service udev restart
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
./configure --prefix=/usr/local/protobuf
make
make check
sudo make install
sudo ldconfig # refresh shared library cache.
protoc --version
```
Add envionment variables,and edit /etc/profile：
```bash
sudo vim /etc/profile
```
add:

    export PATH=$PATH:/usr/local/protobuf/bin

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib

To build and install the Protocol Buffer for python (protobuf):
```bash
cd python
sudo python setup.py install
pip show protobuf
```
Compile *.proto ,ouput for python

protoc -I=$SRC_DIR --python_out=$DST_DIR $SRC_DIR/FlightLog.proto

example:
```bash
protoc -I=. --python_out=. FlightLog.proto
```
This generates addressbook_pb2.py in your specified destination directory.
