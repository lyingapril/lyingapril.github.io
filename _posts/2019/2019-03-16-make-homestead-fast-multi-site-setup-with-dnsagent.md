---
layout: post
title:  "借助 DnsAgent，让 HomeStead 快捷实现多站点设置"
date:   2019-03-16 10:21:38 +0800
categories: Post
tags: [homestead, dnsagent]
---
&emsp;&emsp;众所周知，HomeStead可以实现多站点的功能，但是每次都需要修改本机的hosts文件，实在是太不方便了，借助DnsAgent的域名解析功能，轻松实现多站点管理。

&emsp;&emsp;首先，先熟悉下HomeStead的配置文件。

## HomeStead配置

&emsp;&emsp;使用的[Homestead](https://github.com/laravel/homestead)版本号[v8.2.0](https://github.com/laravel/homestead/releases/tag/v8.2.0)

&emsp;&emsp;在执行了init.sh之后，得到了名为Homestead.yaml的配置文件

```yaml
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/code
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/public

databases:
    - homestead

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp

# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# zray:
#  If you've already freely registered Z-Ray, you can place the token here.
#     - email: foo@bar.com
#       token: foo
#  Don't forget to ensure that you have 'zray: "true"' for your site.

```

&emsp;&emsp;ip是虚拟机的ip地址，默认是192.168.10.10。memory是虚拟机的内存，默认是2048 (KB)。cpus是虚拟机的cpu核数，默认是单核。provider是虚拟机用什么来提供，默认是virtualbox，所以安装Homestead前要把virtualbox装好。authorize是公钥，keys是私钥，在Windows平台，可以使用git bash来[生成](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)。这里的私钥在虚拟机成功启动后，会复制虚拟机的~/.ssh/id_rsa目录下。

&emsp;&emsp;folders是主机与虚拟机之间的目录映射。map是主机的目录，这个短波浪线~在Windows平台是C:\Users\john，john是目前Windows登录账户的用户名。

&emsp;&emsp;sites是虚拟机nginx站点配置文件，每一个map-to对应一个/etc/nginx/sites-enabled/目录配置文件。map就是配置文件名，也是配置文件里的server_name项，to对应配置文件里的root项。

&emsp;&emsp;databases是虚拟机启动后自动生成的数据库，可以在下面添加多个数据库。

&emsp;&emsp;配置修改之后，执行

```sh
homestead provision
```

&emsp;&emsp;使配置生效。

## DnsAgent

&emsp;&emsp;使用的[DnsAgent](https://github.com/stackia/DNSAgent)版本号[v1.6.5781](https://github.com/stackia/DNSAgent/releases/tag/v1.6.5781)

&emsp;&emsp;下载releases页面的压缩包，解压后有这些文件

> ARSoft.Tools.Net.dll*  'Install as Service.bat'  'Uninstall Service.bat'   rules.cfg  DNSAgent.exe*  Newtonsoft.Json.dll*  options.cfg

&emsp;&emsp;主要是rules.cfg和options.cfg两个配置文件，接下来简单介绍下。

### rules.cfg

```json
[
    {
        "Pattern": "^(.*\\.mydomain\\.com)|((.*\\.)?(yourdomain|hisdomain)\\.com)$",
        "Address": "112.223.221.26"
    },
    {
        "Pattern": "^www\\.google\\.com\\.hk$",
        "Address": "www.google.com",
        "NameServer": "8.8.4.4",
        "CompressionMutation": true
    },
    {
        "Pattern": "^(.*)\\.mysuffix\\.com$",
        "Address": "{1}"
    },
    {
        "Pattern": "^www\\.google\\.com\\.tw$",
        "Address": "www.google.com",
        "NameServer": "127.0.0.1"
    },
    {
        "Pattern": "^www\\.google\\.co\\.jp$",
        "Address": "www.google.com"
    },
    {
        "Pattern": "^www\\.google\\.cn$",
        "NameServer": "114.114.114.114"
    },
    {
        "Pattern": "^.*\\.cn$",
        "NameServer": "119.29.29.29",
        "UseHttpQuery": true,
        "QueryTimeout": 1000
    }
]
```

&emsp;&emsp;Pattern是匹配的网址，支持使用[正则表达式](https://zh.wikipedia.org/zh-hans/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)。

&emsp;&emsp;Address是解析到的ip地址或其他网址。

&emsp;&emsp;NameServer是DNS服务器。

### options.cfg

```json
{
    "HideOnStart": false,
    "ListenOn": "127.0.0.1:53, [::1]",
    "DefaultNameServer": "119.29.29.29",
    "UseHttpQuery": false,
    "QueryTimeout": 4000,
    "CompressionMutation": false,
    "CacheResponse": true,
    "CacheAge": 86400,
    "NetworkWhitelist": null
}
```

&emsp;&emsp;HideOnStart是true时，软件启动之后的命令行窗口会隐藏。

&emsp;&emsp;ListenOn是监听的端口。

&emsp;&emsp;DefaultNameServer是默认DNS服务器地址。

&emsp;&emsp;CacheResponse是true时，会从缓存中读取解析记录

更多配置解释见[官方文档](https://github.com/stackia/DNSAgent/tree/v1.6.5781#configuration)。

&emsp;&emsp;在使用Homestead默认使用的域名是[http://homestead.test](http://homestead.test)，如果要是域名解析成功，需要修改电脑的hosts文件，增加一个站点就需要修改一次，很麻烦。为此，可以在rules.cfg配置文件中增加一条
```json
{
    "Pattern": "^.*\\.test$",
    "Address": "192.168.10.10"
}
```
&emsp;&emsp;这条配置可以使test结尾的顶级域名都解析到192.168.10.10上，避免了多个站点配置的麻烦。另外，双击Install as Service.bat还可以将DnsAgent加入系统服务，不用每次都启动DnsAgent，修改配置后，将服务重启下就可以了。

## 参考链接

[https://laravelacademy.org/post/19428.html](https://laravelacademy.org/post/19428.html)

