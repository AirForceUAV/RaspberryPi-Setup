# RaspberryPi For Mission Control
Be Mission Control Panle

## Electron

```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
sudo cnpm install -g electron
```

```bash
git clone git@github.com:AirForceUAV/MissionControl.git
electron ~/MissionControl
```

##Supervisor

```bash
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
sudo supervisord -c /etc/supervisord.conf
```

```bash
sudo vim /etc/rc.local
```

Add blow content before `exit 0`

```bash
sudo /usr/local/bin/supervisord -c /etc/supervisord.conf
```