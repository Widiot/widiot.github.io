---
title: 'Ubuntu18.04美化'
layout: post
tags:
  - Ubuntu
category: OS
lang: zh
---

Ubuntu18.04使用GNOME取代了Unity，这里记录一下主题、终端、登陆、开机的美化过程

<!-- more -->

# 效果展示

桌面
![桌面](https://img-blog.csdnimg.cn/20190331145006906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

应用程序
![应用程序](https://img-blog.csdnimg.cn/20190331145106482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

文件夹
![文件夹](https://img-blog.csdnimg.cn/20190331145147903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

终端
![终端](https://img-blog.csdnimg.cn/2019033114545896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

锁屏和登陆界面不方便截图，所以用的官方示例图
![锁屏](https://img-blog.csdnimg.cn/20190331150125594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)
![登陆界面](https://img-blog.csdnimg.cn/20190331150133428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

# 安装GNOME Tweaks

安装GNOME Tweaks主题美化工具
```bash
sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-shell-extensions
sudo apt-get install  gnome-shell-extension-dashtodock
```

打开tweak，选择扩展，打开User themes选项
![tweak](https://img-blog.csdnimg.cn/20190331155100564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

然后选择外观，如果shell上有感叹号，关闭tweak，按Alt+F2，输入r，执行后重新打开tweak就没有感叹号了

# 配置主题和图标

下载[macOS High Sierra](https://www.gnome-look.org/s/Gnome/p/1013714/)主题，选择Files下载其中一个主题以及图标。解压之后，把主题放到`/usr/share/themes/`下，图标放到`/usr/share/icons/`下。我选择的主题和图标如下
![下载主题和图标](https://img-blog.csdnimg.cn/20190331161201775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

然后打开tweak，设置外观如下
![设置外观](https://img-blog.csdnimg.cn/20190331161534859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

# 设置Dock

打开tweak，选择扩展，打开Dash to dock，然后点击设置
![打开Dash to dock](https://img-blog.csdnimg.cn/20190331161935618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

这里可以按自己的喜好设置Dock
![设置Dock](https://img-blog.csdnimg.cn/20190331162146689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)
![设置Dock](https://img-blog.csdnimg.cn/20190331162205229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

# 设置开机动画

下载[Ubuntu Paw Plymouth](https://www.opendesktop.org/p/1154790/)主题，解压缩放在/usr/share/plymouth/themes/下

然后备份并编辑文件
```bash
sudo cp /etc/alternatives/default.plymouth /etc/alternatives/default.plymouth.bak
sudo vim /etc/alternatives/default.plymouth
```

最后两行修改为新的主题路径
```shell
[Plymouth Theme]
Name=Ubuntu Logo
Description=A theme that features a blank background with a logo.
ModuleName=script

[script]
ImageDir=/usr/share/plymouth/themes/Ubuntu-Paw
ScriptFile=/usr/share/plymouth/themes/Ubuntu-Paw/ubuntu-paw.script
```

# 设置登陆界面

下载[High Ubunterra ](https://www.opendesktop.org/s/Gnome/p/1207015/)主题，选择Files下载其中一个主题（根据其产品描述，带CC后缀的只能用于Ubuntu 18.10，我开始没注意，下载了带CC后缀的，结果安装不了），我选择的如下
![下载登陆界面主题](https://img-blog.csdnimg.cn/20190331163728236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

解压缩在终端运行
```bash
sudo chmod +x install.sh
./install.sh
```

更换锁屏壁纸只需要选择一张图片右键，然后在脚本中选择SetAsWallpaper

# 设置终端

Zsh是替代Bash的终端，还可以设置多种主题，在终端中安装
```bash
sudo apt-get install zsh
```

然后用Oh My Zsh美化Zsh终端
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

最后用Zsh替换Bash
```bash
chsh -s `which zsh`
```

其他的更改可以直接右键终端，选择配置文件首选项进行设置
![终端设置](https://img-blog.csdnimg.cn/20190331165313337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1doaXRlX0lkaW90,size_16,color_FFFFFF,t_70)

# 参考
* [Ubuntu18.04美化总结](https://www.jianshu.com/p/6ef16e3b0a3e)
* [Ubuntu18.04美化主题(mac主题)](https://blog.csdn.net/lishanleilixin/article/details/80453565)