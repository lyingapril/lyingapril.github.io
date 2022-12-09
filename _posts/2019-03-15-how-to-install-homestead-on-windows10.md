---
layout: post
title:  "在 Windows10 系统中安装 Homestead 本地开发环境 （小白填坑版）"
date:   2019-03-15 10:21:38 +0800
categories: post
---

　　刚入门学习laravel总会遇到各种问题，比如安装homestead，本文记录安装homestead的整个过程，记录时间2019/03/15（刚好315消协日）

## 准备工作
- VirtualBox
- Vagrant

　　这里选择的版本都是截止写本篇记录的最新版本，附上下载页面和下载的版本的下载链接
- [VirtualBox下载页](https://www.virtualbox.org/wiki/Downloads)
- [VirtualBox v6.0.4](https://download.virtualbox.org/virtualbox/6.0.4/VirtualBox-6.0.4-128413-Win.exe)
- [Vagrant下载页](https://www.vagrantup.com/downloads.html)
- [Vagrant v2.2.4 64-bit](https://releases.hashicorp.com/vagrant/2.2.4/vagrant_2.2.4_x86_64.msi)

　　下载好了安装过程就不记录了，尽量不要装在系统盘

## 开始填坑

　　Vagrant安装好了之后，按win+R键，在运行对话框里输入cmd，打开cmd，切换目录到其他非系统盘，切换到E:\homestead，接着初始化目录，并下载box
```sh
cd /d E:\homestead
vagrant init laravel/homestead
vagrant up
```

　　在命令行里看到了这些
> vagrant up
<br />Bringing machine 'default' up with 'virtualbox' provider...
<br />==> default: Box 'laravel/homestead' could not be found. Attempting to find and install...
<br />default: Box Provider: virtualbox
<br />default: Box Version: >= 0
<br />==> default: Loading metadata for box 'laravel/homestead'
<br />default: URL: https://vagrantcloud.com/laravel/homestead
<br />==> default: Adding box 'laravel/homestead' (v7.1.0) for provider: virtualbox
<br />default: Downloading: https://vagrantcloud.com/laravel/boxes/homestead/versions/7.1.0/providers/virtualbox.box
<br />default: Download redirected to host: vagrantcloud-files-production.s3.amazonaws.com
<br />default: Progress: 9% (Rate: 460k/s, Estimated time remaining: 0:59:10)

　　下载速度不快不慢。。。耐心等待
　　在等待了一会儿之后发现报错了，错误如下
> default:
> An error occurred while downloading the remote file. The error
> message, if any, is reproduced below. Please fix this error and try
> again.
> 
> OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054

　　所以直接用IDM直接下载前面命令行里的链接 [https://vagrantcloud.com/laravel/boxes/homestead/versions/7.1.0/providers/virtualbox.box](https://vagrantcloud.com/laravel/boxes/homestead/versions/7.1.0/providers/virtualbox.box) ，10分钟后，50M的宽带终于把它下载好了，将下载好的virtualbox.box移动到E:\homestead，移动的时候才发现需要把它的乱码名字重命名为virtualbox.box，接着运行
```sh
vagrant box add laravel/homestead virtualbox.box
```

　　在命令行里看到了这些
> ==> box: Box file was not detected as metadata. Adding it directly...
> 
> ==> box: Adding box 'laravel/homestead' (v0) for provider:
> 
>   box: Unpacking necessary files from: file://E:/homestead/virtualbox.box
> 
>   box: Progress: 100% (Rate: 83.1M/s, Estimated time remaining: --:--:--)
> 
> ==> box: Successfully added box 'laravel/homestead' (v0) for 'virtualbox'!

　　好了box已经成功的添加，接着运行
```sh
vagrant box list
```

　　可以看到laravel/homestead (virtualbox, 0)已经在列表里面了

## 安装homestead
```sh
git clone https://github.com/laravel/homestead.git Homestead
cd ./Homestead
init.bat
vagrant up
```

　　但是没有成功，提示又要下载box，显示如下错误
> Bringing machine 'homestead-7' up with 'virtualbox' provider...
> 
> ==> homestead-7: Box 'laravel/homestead' could not be found. Attempting to find and install...
> 
>    homestead-7: Box Provider: virtualbox
> 
>    homestead-7: Box Version: >= 7.0.0
> 
> ==> homestead-7: Loading metadata for box 'laravel/homestead'
> 
>    homestead-7: URL: https://vagrantcloud.com/laravel/homestead
> 
> ==> homestead-7: Adding box 'laravel/homestead' (v7.1.0) for provider: virtualbox
> 
>    homestead-7: Downloading: https://vagrantcloud.com/laravel/boxes/homestead/versions/7.1.0/providers/virtualbox.box
> 
> ==> homestead-7: Box download is resuming from prior download progress
> 
>    homestead-7: Progress: 0% (Rate: 0*/s, Estimated time remaining: --:--:--)==> homestead-7: Waiting for cleanup before exiting...
> 
>    homestead-7:
> 
> ==> homestead-7: Box download was interrupted. Exiting.

　　为什么呢，因为之前执行
```sh
vagrant box add laravel/homestead virtualbox.box
```

　　的时候，成功安装的box版本是V0,而homestead对box版本有要求，怎么解决呢？先把旧的删除，再重新安装，需要借助一个json文件，新建一个homestead.json文件，内容如下
```json
{
	"name": "laravel/homestead",
	"versions": [{
		"version": "7.1.0",
		"providers": [{
			"name": "virtualbox",
			"url": "file:///E:/homestead/virtualbox.box"
		}]
	}]
}
```

　　接着，执行
```sh
vagrant box remove laravel/homestead
vagrant box add homestead.json
```

　　运行之后的提示
> ==> box: Loading metadata for box 'homestead.json'
> 
>    box: URL: file://E:/homestead/homestead.json
> 
> ==> box: Adding box 'laravel/homestead' (v7.1.0) for provider: virtualbox
> 
>    box: Unpacking necessary files from: file://E:/homestead/virtualbox.box
> 
>    box: Progress: 100% (Rate: 100M/s, Estimated time remaining: --:--:--)
> 
> ==> box: Successfully added box 'laravel/homestead' (v7.1.0) for 'virtualbox'!

　　到这一步，就全部安装完成了，运行
```sh
　　vagrant up
```
　　启动虚拟机就可以了，第一次启动速度比较满，耐心

## 参考链接
[https://learnku.com/docs/laravel/5.7/homestead/2245](https://learnku.com/docs/laravel/5.7/homestead/2245)

[https://learnku.com/articles/16664/install-homestead-local-development-environment-in-windows10-system](https://learnku.com/articles/16664/install-homestead-local-development-environment-in-windows10-system)

[https://www.jianshu.com/p/ae9d1261bbd8](https://www.jianshu.com/p/ae9d1261bbd8)
