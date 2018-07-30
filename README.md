# 关于本文档

## 本文是登录系统的设计文档

登录系统项目代码地址    [https://github.com/caijw/loginSystem](https://github.com/caijw/loginSystem)

##  运行项目代码：

{% hint style="info" %}
运行项目代码适用于macOS
{% endhint %}

### 1、下载代码：

git 下载的方式：

```text
git clone  https://github.com/caijw/loginSystem.git
```

### 2、安装依赖：

```text
cd loginSystem
sh ./install.sh
```

本项目有大量的依赖：mongo\_c\_driver、mongo\_cxx\_driver、grpc、protobuf 3、cocoapods还有其他编译相关的所需软件，因此安装依赖过程是一个漫长的过程，安装过程需要输入root密码。

### 3、生成项目执行程序:

```text
sh ./make.sh
```

生成程序后，会自动运行server，server所在的目录为loginSystem/out下的server。同时会打开客户端的xcode项目，可以在xcode中运行客户端软件。

如果后续只是想单独的运行server，可以运行：

```text
sh ./runServer.sh
```

或者到out目录下运行server程序。

如果后续只是想单独的打开客户端的xcode项目，可以运行：

```text
sh ./runClient.sh
```

