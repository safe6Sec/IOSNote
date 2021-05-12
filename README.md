# ios逆向笔记



## 基础知识

ios安装包后缀ipa

ios应用安装后和mac的程序一样，和安卓的安装不一样

ISO ipa安装目录

/private/var/containers/Bundle/Application

/private/var/mobile/Containers/Data/Application

安卓apk 的安装目录：

system/app 系统自带的应用程序

data/app 用户程序安装的目录

hook使用frida

ios越狱cydia

cyida添加源安装对应服务端

https://www.jianshu.com/p/71587d8b39f4

开发工具xcode

编程语言oc(objective-c)

静态分析ida，hopper

连接iPhone设备使用openssh(也可以用usb连接) 

默认密码alpine

https://www.jianshu.com/p/d5fbacb1bf5c



## 逆向分析

IOS下逆向常规操作判断该APP是否加壳(加密),有壳就砸壳，但是一般下载过来的ipa都是有壳的，我们直接砸壳提取出ipa，然后利用IDA或者Hopper对脱壳后的**Mach-O**文件进行分析。



## 砸壳

可以先把安装目录下的二进制Mach-O提取出来，判断是否有壳

```
otool -l 可执行文件 | grep crypt
```

cryptid 0 = 无壳

查看 cryptid 即可（有壳为1，已脱壳为0）

如果无壳就拖入ida直接开始分析



如有壳直接用[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)砸壳



安装方法：

```
git clone https://github.com/AloneMonkey/frida-ios-dump
cd frida-ios-dump
pip install -r requirements.txt --upgrade
```

安装完成所需的依赖后打开dump.py进行配置。

如使用openssh需要更改下面的配置，如使用的usb，可以使用下面的默认配置

```
User = 'root'
Password = 'alpine'#默认的密码
Host = 'localhost'
Port = 2222 #根据自己的端口进行修改
KeyFileName = None
```

使用方法:

```
python3 dump.py APP名字
或者
python3 dump.py Bundle identifier
```

将手机与电脑连接，打开APP，执行`frida-ps -Ua`,查看APP名字和Identifier。

```
frida-ps -Ua
 PID  Name    Identifier
----  ------  --------------------
7312  APP名字  com.xxxx.tomatodo
```

然后进行砸壳

python3 dump.py APP名字，或者Identifier



随后会在**frida-ios-dump**的根目录生成ipa安装

提取二进制文件

把安装解压

unzip xxxx.ipa

得到payload文件夹

进入提取对应的Mach-O二进制文件



## 二进制分析



学oc

学汇编



二进制分析



## 抓包

工具：Charles，burp

抓包http，iPhone设置代理



抓包tcp

工具：tcpdump，rvictl



1使用rvictl创建虚拟网卡

rvictl -s -l -x
`rvictl -s <UDID> //开启虚拟网卡`
在wireshark中选中rv0网卡
`rvictl -x <UDID> //关闭虚拟网卡`



2.使用tcpdump

在cydia搜索安装tcpdump，

tcpdump -w ghost.cap开始抓包









