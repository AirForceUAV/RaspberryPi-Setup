# RaspberryPi For Mission Control
Be Mission Control Panle

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
command=sudo /home/pi/Go/src/DataProxy/DataProxy mc
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
sudo vim /etc/supervisor/HTTPServer.conf
```

Add below content 

```bash
[program:HTTPServer]
directory=/home/pi/MissionControl
command=python -m SimpleHTTPServer
autostart=true
autorestart=true
startsecs=5
startretries=10
user=pi
stdout_logfile=/var/log/SimpleHTTPServer.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/var/log/SimpleHTTPServer.log
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