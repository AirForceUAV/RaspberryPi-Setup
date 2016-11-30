dnf update

systemctl enable sshd.service

service sshd start

su -c 'dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm'

sudo dnf install ftp://195.220.108.108/linux/rpmfusion/free/fedora/updates/24/x86_64/g/gstreamer1-libav-1.8.2-1.fc24.i686.rpm

sudo dnf update

sudo reboot

sudo dnf install -y util-linux-user zsh git vim tmux go nodejs libXScrnSaver python gstreamer gstreamer1-libav unrar

sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

chsh -s /bin/zsh

exit

sudo npm install -g cnpm --registry=https://registry.npm.taobao.org

sudo cnpm install -g electron

sudo ifdown wlp1s0

sudo route del default gw 192.168.10.1