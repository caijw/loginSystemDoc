---
description: 关于登录系统的项目背景介绍
---

# 项目背景

## 设计一个登录系统，需要满足以下需求：

1. 具备登录注册功能
2. 用户只能在一个设备上登录，切换登录终端时，其他已经登录的终端会被踢出
3. 后台可以根据管理策略将已经登录的设备踢出

## 系统客户端和后台的设计要求：

1. 客户端登录模块需要采用C++实现ios和android的跨平台；终端app只要提供ios或android一种客户端实现即可
2. 后台采用grpc实现，用户信息存储上，可以采用sql和nosql实现
3. 设计时需要考虑敏感数据的安全性和传输安全
4. 项目设计实现一周左右
5. 交付物包括可运行的客户端以及后台项目代码，设计文档，可用github管理

## 参考资料：

1. C++实现ios和android跨平台：[https://github.com/dropbox/djinni](https://github.com/dropbox/djinni)
2. grpc文档：[https://grpc.io/](https://grpc.io/)
3. mongodb：[https://docs.mongodb.com/](https://docs.mongodb.com/)
4. bazel: [https://bazel.build/](https://bazel.build/)

## 项目代码：

github仓库 [https://github.com/caijw/loginSystem](https://github.com/caijw/loginSystem)

