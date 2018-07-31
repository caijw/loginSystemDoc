# 关于本文档

## 本文是登录系统的设计文档

登录系统项目代码地址    [https://github.com/caijw/loginSystem](https://github.com/caijw/loginSystem)

##  运行项目代码：

### 1、下载代码：

git 下载的方式：加 recurisve参数可以将submodule都下载下来

```text
git clone --recursive  https://github.com/caijw/loginSystem.git
```

### 2、前置依赖：

安装mongodb：

{% embed data="{\"url\":\"https://www.mongodb.com/download-center\#community\\n\",\"type\":\"link\",\"title\":\"MongoDB Download Center\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.mongodb.com/assets/images/global/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"http://s3.amazonaws.com/info-mongodb-com/\_com\_assets/cms/mongodb-for-giant-ideas-bbab5c3cf8.png\",\"width\":1024,\"height\":512,\"aspectRatio\":0.5}}" %}

安装项目依赖：

```text
cd loginSystem
sh ./deps_install.sh
```

依赖的有mongo-c-driver驱动、mongo-cxx-driver驱动

编译server：

```text
bazel build //src:server
```

打开ios客户端的xcode项目：

```text
sh ./runClient.sh
```

### 3、运行程序

运行server：

```text
bazel run //src:server
```

运行ios客户端：

xcode中运行项目即可

