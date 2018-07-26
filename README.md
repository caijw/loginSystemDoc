# 关于本文档

## 本文是登录系统的设计文档

登录系统项目代码地址    [https://github.com/caijw/loginSystem](https://github.com/caijw/loginSystem)

项目依赖了第三方库，git 下载的方式：

git clone --recursive [https://github.com/caijw/loginSystem.git](https://github.com/caijw/loginSystem.git)  
可以将依赖一次性下载

## 其他的环境依赖：

1、需要下载完需要手动安装grpc，grpc安装文档参照官方：

{% embed data="{\"url\":\"https://github.com/grpc/grpc/blob/v1.13.x/INSTALL.md\",\"type\":\"link\",\"title\":\"grpc/grpc\",\"description\":\"grpc - The C based gRPC \(C++, Python, Ruby, Objective-C, PHP, C\#\)\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/7802525?s=400&v=4\",\"width\":400,\"height\":400,\"aspectRatio\":1}}" %}

2、安装mongodb服务器：

{% embed data="{\"url\":\"https://www.mongodb.com/download-center\#atlas\",\"type\":\"link\",\"title\":\"MongoDB Download Center\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.mongodb.com/assets/images/global/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"http://s3.amazonaws.com/info-mongodb-com/\_com\_assets/cms/mongodb-for-giant-ideas-bbab5c3cf8.png\",\"width\":1024,\"height\":512,\"aspectRatio\":0.5}}" %}

代码里面写死了服务器的地址和端口号 [https://github.com/caijw/loginSystem/blob/master/src/server.cpp\#L326](https://github.com/caijw/loginSystem/blob/master/src/server.cpp#L326)

3、安装mongodb cpp驱动：

{% embed data="{\"url\":\"https://mongodb.github.io/mongo-cxx-driver/mongocxx-v3/installation/\",\"type\":\"link\",\"title\":\"Installing the mongocxx driver\",\"icon\":{\"type\":\"icon\",\"url\":\"https://mongodb.github.io/mongo-cxx-driver/img/favicon.png\",\"aspectRatio\":0}}" %}

4、编译server：

进入src目录，运行make，会在out目录生成server可执行文件，make过程可能会提示grpc的依赖没有安装，需要进行相关安装。

5、编译ios client：

进入djinni目录，运行 sh ./run\_djinni.sh  进行djinni的代码生成

进入login\_client\_ios目录,运行pod install

然后运行 open login\_client\_ios.xcworkspace 打开xcode进行编译和运行。



由于本项目依赖了太多的环境，配置环境是一个复杂的过程。



