# 项目代码组织

### 代码仓库： [https://github.com/caijw/loginSystem](https://github.com/caijw/loginSystem)

项目依赖了第三方库，git 下载的方式：

git clone --recursive [https://github.com/caijw/loginSystem.git](https://github.com/caijw/loginSystem.git) 可以将依赖一次性下载

### deps目录

存放依赖的第三方库

djinni：跨平台的代码生成器

argon2: 进行密码哈希计算的argon2算法库

mongo-c-driver: mongodb c 驱动代码

mongo-cxx-driver mongodb cpp 驱动代码

### djinni目录

存放djinni配置和代码

### login\_client\_ios目录

存放ios客户端项目

### protos目录

存放protobuf协议

### src目录

存放项目的cpp代码，主要是server模块的代码和一些工具类

