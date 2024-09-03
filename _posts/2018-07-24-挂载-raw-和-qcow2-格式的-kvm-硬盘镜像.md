---
title: "挂载 raw 和 qcow2 格式的 KVM 硬盘镜像"
date: "2018-07-24"
categories: 
  - "get-something"
tags: 
  - "linux"
  - "lvm"
  - "qcow2"
  - "raw"
  - "新技能get"
  - "漫步云端"
---

> from:https://lazyhack.net/mount-raw-and-qcow2-kvm-disk-images/ 

## raw格式 

对于未分区镜像文件直接使用loop： 
```bash
mount -o loop image.img /mnt/image
``` 

已分区的镜像文件： 如果已知分区的起始位置 `mount -o loop,offset=32256 image.img /mnt/image` 或者使用losetup + kpartx 

```bash
losetup /dev/loop0 image.img 
kpartx -a /dev/loop0 
mount /dev/mapper/loop0p1 /mnt/image
```
kpartx命令的作用，是让Linux内核读取一个设备上的分区表，然后生成代表相应分区的设备。 `kpartx -l imagefile` 可以查看一个映像文件中的分区，使用 `kpartx -a imagefile` 命令后，就可以通过 `/dev/mapper/loop0pX` （其中X是 分区号）来访问映像。 

## qcow2格式 

对于qcow2格式需要使用`qemu-nbd`这个工具 

```bash
modprobe nbd max_part=63 
qemu-nbd -c /dev/nbd0 image.img 
mount /dev/nbd0p1 /mnt/image 
```

如果是LVM格式的镜像： 

```bash
vgscan vgchange -ay 
mount /dev/VolGroupName/LogVolName /mnt/image
```

最后使用结束需释放资源： 

```bash
umount /mnt/image 
vgchange -an VolGroupName 
killall qemu-nbd 
kpartx -d /dev/loop0 
losetup -d /dev/loop0
```

转自： https://krystism.is-programmer.com/posts/47074.html
