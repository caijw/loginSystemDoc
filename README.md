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

[https://www.mongodb.com/download-center\#community](https://www.mongodb.com/download-center#community)

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

编译cpp的client（可选）:

```text
bazel  build  //src:client
```

### 3、运行程序

db配置初始化：

数据库需要建一个login\_system的db，里面有一个user\_id和user\_info的表，可以导入仓库上的初始化配置：[https://github.com/caijw/loginSystem/tree/master/dbdirectory/login\_system](https://github.com/caijw/loginSystem/tree/master/dbdirectory/login_system)

```text
mongorestore -h 127.0.0.1:27017  --dir dbdirectory/
```

运行mongodb：

--dbpath可以指定你的database所在的目录，不指定该参数的话，默认是/data/db

```text
sudo   mongod   --dbpath  you_database_path
```

运行server：

```text
bazel run //src:server
```

运行ios客户端：

xcode中运行项目即可

运行cpp的client：

```text
bazel run //src:client
```

