---
title: "CentOS7 安装 ghost 博客记录"
date: "2018-11-25"
categories: 
  - "get-something"
  - "walking-cloud"
tags: 
  - "blog"
  - "centos"
  - "ghost"
---

1，更新系统到最新

```bash
yum update -y
```

2，安装 yarn

```
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

3，安装 node，直接下载可执行文件，不需要源码安装

```
tar xzf node-v10.13.0-linux-x64.tar.gz
mv node-v10.13.0-linux-x64 /usr/local/nodejs
#（安装 yarn 时会安装 node 0.6.4 版）
mv /usr/bin/node /usr/bin/node.bak
mv /usr/bin/npm /usr/bin/npm.bak
ln -s /usr/local/nodejs/bin/node /usr/bin/node
ln -s /usr/local/nodejs/bin/npm /usr/bin/npm
```

4，安装 ghost-cli

```
yarn global add ghost-cli@latest
```

5，添加一个有 sodu 权限的用户

```
adduser oops
passwd oops
usermod -aG wheel oops
```

6，切换用户

```
su - oops
```

7，新建目录及设置权限

```
sudo mkdir -p /home/wwwroot/ghost
sudo chown oops.oops -R /home/wwwroot/ghost
sudo chmod 775 /home/wwwroot/ghost
cd /home/wwwroot/ghost
```

8，安装 ghost

```
ghost install
```

9，配置 nginx

```
location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:2368;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

10，启动 ghost

```
#***换成你的域名，aaa.com 就是 aaa-com
systemctl start ghost_***
systemctl status ghost_***
systemctl enable ghost_***
```
