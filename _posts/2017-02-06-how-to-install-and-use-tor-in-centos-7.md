---
title: "How To install and use tor in centos 7?"
date: "2017-02-06"
categories: 
  - "get-something"
tags: 
  - "centos"
  - "linux"
  - "os"
  - "security"
  - "新技能get"
---

Today, I’ll help you guys to install and use tor in your centos 7 machine. This method works with other versions of centos or fedora too.

Here’s how you will do it. We have to add the repository to install tor. For that, we need to create a new repo file in the yum.repos.d folder. For that, issue the following command (Please note that you need to be root to edit this file) `#gedit /etc/yum.repos.d/torproject.repo` Now, paste the following text into that file and save it 

```
[tor] name=Tor repo enabled=1 baseurl=http://deb.torproject.org/torproject.org/rpm/el/7/$basearch/ gpgcheck=1 gpgkey=http://deb.torproject.org/torproject.org/rpm/RPM-GPG-KEY-torproject.org.asc [tor-source] name=Tor source repo enabled=1 autorefresh=0 baseurl=http://deb.torproject.org/torproject.org/rpm/el/7/SRPMS gpgcheck=1 gpgkey=http://deb.torproject.org/torproject.org/rpm/RPM-GPG-KEY-torproject.org.asc
``` 

**If you want to install tor in your other versions of fedora or centos or redhat, check the following table.** 

fedora 19 – fc/19 fedora 20 – fc/20 centos 6 – el/6 And reaplace “el/7” in the repo file with the corresponding one. (If you have Centos 7, then you don’t need to replace it – obviously ) 

**Important Note:** 

If you have epel.repo enabled in your system, then you should open that file( /etc/yum.repos.d/epel.repo) and edit it to add the following line to it. Exclude=torSave the file.

Once the repo file is in position, we can simply install it by issuing `#yum install tor` Once the installation is finished, we can start the service by typing this command,

`# service tor start` 

**Configuring Your Firefox browser:**

It is highly recommended to use the Tor browser bundle, but if you want to use firefox with tor, once you have started the service, open firefox settings >> Preferences >> advanced >> network >> settings and select manual proxy configuration. Refer to the following image and configure accordingly.

![Configuring Your Firefox browser](/assets/images/Screenshot-from-2014-10-11-233955.png) 

Configuring Your Firefox browser

You're done. Go to [https://check.torproject.org/](https://check.torproject.org/) to check if it is working properly or not.

本文转自： [https://digitz.org/blog/how-to-install-and-use-tor-in-centos-7-2/](https://digitz.org/blog/how-to-install-and-use-tor-in-centos-7-2/)
