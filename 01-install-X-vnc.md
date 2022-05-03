#### Step 1 - Update System and Create User to run as VNC - Using Non GUI Ubuntu 20.04 and TightVNC
sudo apt update &&  apt upgrade -y

useradd -m -s /bin/bash vnc-user

passwd vnc-user

usermod -a -G sudo vnc-user

ssh vnc-user@ubuntu20.04

#### Step 2 – Install XFCE Desktop and TightVNC.  Linux has several desktop environments such as Gnome, Unity, KDE, LXDE, XFCE etc. For this tutorial, we will be using the XFCE desktop as our VNC desktop environment.
sudo apt install xfce4 xfce4-goodies

sudo apt install -y tightvncserver

#### Step 3 – VNC Configuration for "vnc-user" user
vncserver

ps -ef | grep Xtightvnc

vncserver -kill :1

mv /home/user/vnc-user/.vnc/xstartup ~/.vnc/xstartup.original

vim ~/.vnc/xstartup

  #!/bin/bash \
  xrdb $HOME/.Xresources \
  startxfce4 &

:wq!

chmod +x ~/.vnc/xstartup

vncserver

#### Step 4 – Running TightVNC as a Service
sudo su -

cd /etc/systemd/system

vim vncserver@.service

[Unit] 

 Description=Remote desktop service (VNC)

 After=syslog.target network.target

[Service]

  Type=forking

  User=vnc-user

  PIDFile=/home/vnc-user/.vnc/%H:%i.pid

  ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1

  ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 :%i

  ExecStop=/usr/bin/vncserver -kill :%i

[Install]

  WantedBy=multi-user.target

:wq!

systemctl daemon-reload

systemctl start vncserver@1.service

systemctl enable vncserver@1.service

systemctl status vncserver@1.service