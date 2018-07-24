# server模块

server简介：

server模块提供了AS的功能，主要用来提供：注册、登录和session ticket校验的功能。

server对外提供的接口：

1、注册接口

grpc接口

```cpp
rpc registerAccount (registerRequest) returns (registerResponse) {}
```

grpc请求结构体

```cpp
message registerRequest {
	string h1 = 1;
	string nickname = 2;
	string phone_num = 3;
}
```

h1是密码的MD5，即H1

nickname和phone\_num是一些用户信息

grpc响应结构体

```cpp
message registerResponse {
	int32 ret = 1;
	string msg = 2;
	string user_id = 3;
}
```

ret是请求返回码

msg是请求的返回message

user\_id是新注册用户的id

2、登录接口

grpc接口

```cpp
rpc loginAccount (loginRequest) returns (loginResponse) {}
```

grpc请求结构体

```cpp
message loginRequest {
	string user_id = 1;
	string s1 = 2;
}
```

user\_id是用户id，s1是本系统的

grpc响应结构体

```cpp
message loginResponse {
	int32 ret = 1;
	string msg = 2;
 	string st = 3;
}
```

ret是请求返回码

msg是请求的返回message

st是本文的ST，即session ticket

3、session ticket校验接口

grpc接口

```cpp
rpc verifyST (verifySTRequest) returns (verifySTResponse) {}
```

grpc请求结构体

```cpp
message verifySTRequest {
	string user_id = 1;
	string st = 2;
}
```

user\_id是用户id，st是 session ticket

grpc响应结构体

```cpp
message verifySTResponse {
	int32 ret = 1;
	string msg = 2;
}
```

ret是校验接口的返回码

msg是校验接口返回的消息

