---
title: "How to install Desktop Environments on CentOS 7?"
date: "2017-12-21"
categories: 
  - "get-something"
tags: 
  - "centos"
  - "cinnamon"
  - "gnome"
  - "how-to"
  - "kde"
  - "linux"
  - "mate"
  - "os"
  - "xfce"
---

### 1\. Installing GNOME-Desktop:

1. Install GNOME Desktop Environment on here.
    
    ```
    # yum -y groups install "GNOME Desktop" 
    ```
    
2. Input a command like below after finishing installation:
    
    ```
    # startx 
    ```
    
3. GNOME Desktop Environment will start. For first booting, initial setup runs and you have to configure it for first time.
    - Select System language first.
    - Select your keyboard type.
    - Add online accounts if you'd like to.
    - Finally click "Start using CentOS Linux".
4. GNOME Desktop Environments starts like follows.

\[caption id="attachment\_252" align="aligncenter" width="1025"\]![GNOME Desktop Environments](images/HVDB0.png) GNOME Desktop Environments\[/caption\]

### How to use GNOME Shell?

The default GNOME Desktop of CentOS 7 starts with _classic mode_ but if you'd like to use GNOME Shell, set like follows:

**Option A:** If you start GNOME with **_startx_**, set like follows.

```

# echo "exec gnome-session" >> ~/.xinitrc
# startx 
```

**Option B:** set the system graphical login [**_systemctl set-default graphical.target_**](http://www.server-world.info/en/note?os=CentOS_7&p=runlevel) and reboot the system. After system starts

1. Click the button which is located next to the "Sign In" button.
2. Select "GNOME" on the list. (The default is GNOME Classic)
3. Click "Sign In" and log in with GNOME Shell.

\[caption id="attachment\_253" align="aligncenter" width="1027"\]![log in with GNOME Shell](images/X7bhJ.png) log in with GNOME Shell\[/caption\]

4. GNOME shell starts like follows:

\[caption id="attachment\_254" align="aligncenter" width="1027"\]![GNOME shell](images/3mRsl.png) GNOME shell\[/caption\]

### 2\. Installing KDE-Desktop:

1. Install KDE Desktop Environment on here.
    
    ```
    # yum -y groups install "KDE Plasma Workspaces" 
    ```
    
2. Input a command like below after finishing installation:
    
    ```
    # echo "exec startkde" >> ~/.xinitrc
    # startx
    ```
    
3. KDE Desktop Environment starts like follows:

\[caption id="attachment\_255" align="aligncenter" width="1027"\]![KDE Desktop Environment](images/iTACp.png) KDE Desktop Environment\[/caption\]

### 3\. Installing Cinnamon Desktop Environment:

1. Install Cinnamon Desktop Environment on here.First Add the EPEL Repository (EPEL Repository which is provided from Fedora project.) [Extra Packages for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL#Extra_Packages_for_Enterprise_Linux_.28EPEL.29)
    - How to add EPEL Repository?
        
        ```
        # yum -y install epel-release
        
        # sed -i -e "s/\]$/\]\npriority=5/g" /etc/yum.repos.d/epel.repo # set [priority=5]
        # sed -i -e "s/enabled=1/enabled=0/g" /etc/yum.repos.d/epel.repo # for another way, change to [enabled=0] and use it only when needed
        # yum --enablerepo=epel install [Package] # if [enabled=0], input a command to use the repository
        ```
        
    - And now install the Cinnamon Desktop Environment from EPEL Repository:
        
        ```
        # yum --enablerepo=epel -y install cinnamon*
        ```
        
2. Input a command like below after finishing installation:
    
    ```
    # echo "exec /usr/bin/cinnamon-session" >> ~/.xinitrc
    # startx 
    ```
    
3. Cinnamon Desktop Environment will start. For first booting, initial setup runs and you have to configure it for first time.
    - Select System language first.
    - Select your keyboard type.
    - Add online accounts if you'd like to.
    - Finally click "Start using CentOS Linux".
4. Cinnamon Desktop Environment starts like follows.

\[caption id="attachment\_256" align="aligncenter" width="1027"\]![Cinnamon Desktop Environment](images/b94jQ.png) Cinnamon Desktop Environment\[/caption\]

### 4\. Installing MATE Desktop Environment:

1. Install MATE Desktop Environment on here.
    
    ```
    # yum --enablerepo=epel -y groups install "MATE Desktop"
    ```
    
2. Input a command like below after finishing installation:
    
    ```
    # echo "exec /usr/bin/mate-session" >> ~/.xinitrc 
    # startx
    ```
    
3. MATE Desktop Environment starts.

\[caption id="attachment\_257" align="aligncenter" width="1025"\]![MATE Desktop Environment](images/PEYSR.png) MATE Desktop Environment\[/caption\]

### 5\. Installing Xfce Desktop Environment:

1. Install Xfce Desktop Environment on here.
    
    ```
    # yum -y groupinstall X11
    # yum --enablerepo=epel -y groups install "Xfce" 
    ```
    
2. Input a command like below after finishing installation:
    
    ```
    # echo "exec /usr/bin/xfce4-session" >> ~/.xinitrc 
    # startx
    ```
    
3. Xfce Desktop Environment starts.

\[caption id="attachment\_258" align="aligncenter" width="1024"\]![Xfce Desktop Environment](images/hPjxx.png) Xfce Desktop Environment\[/caption\]

本文转自： [How to install Desktop Environments on CentOS 7?](https://unix.stackexchange.com/questions/181503/how-to-install-desktop-environments-on-centos-7)
