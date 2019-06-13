---
title: linux下webpack热替换无效
date: 2019-03-13 23:24:50
tags:
---
#### 一、问题可能出现的原因

Linux中Inotify监控文件最大数量过少，导致程序不知道修改了文件。

`Inotify 是一个 Linux内核特性，它监控文件系统，并且及时向专门的应用程序发出相关的事件警告，比如删除、读、写和卸载操作等`

#### 二、查看Inotify监控文件的最大数量

我们可以通过下面的shell命令来查看Inotify监控文件的最大数量:

```
sysctl -a | grep inotify
```

<font color=#0099ff>fs.inotify.max_queued_events:</font> 表示调用inotify_init时分配给inotify,Instance中可排队的event的数目的最大值，超出这个值的事件被丢弃，但会触发IN_Q_OVERFLOW事件。

<font color=#0099ff>fs.inotify.max_user_instances:</font>
表示每一个real user ID可创建的inotify instatnces的数量上限，默认128.

<font color=#0099ff>fs.inotify.max_user_watches:</font>
表示同一用户同时可以添加的watch数目（watch一般是针对目录，决定了同时同一用户可以监控的目录数量）

`可能是fs.inotify.max_user_watches太少，导致文件修改后不能监控到`

#### 三、解决办法

修改系统的<font color=#0099ff>fs.inotify.max_user_watches</font>的值，可以使用下列两种办法来修改

##### 方法1 使用Shell命令直接修改
 
```
sudo sysctl fs.inotify.max_user_watches=524288
```

##### 方法２ 修改sysctl.conf配置文件

```
sudo vim /etc/sysctl.conf
```

在文件的末尾加上

```
fs.inotify.max_user_watches=524288
```

重新从配置文件“/etc/sysctl.conf”加载内核参数设置

```
sudo sysctl -p /etc/sysctl.conf
```

查看修改后的结果

```
sysctl -a | grep inotify
```