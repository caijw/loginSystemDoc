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

注册接口给用户分配一个userId，将userId和用户的S1保存起来，作为已经注册的用户标志。

2、登录接口

grpc接口

```cpp
rpc loginAccount (loginRequest) returns (loginResponse) {}
```

grpc请求结构体

```cpp
message loginRequest {
	string user_id = 1;
	string data = 2;
}
```

user\_id是用户id，data是本系统的S1\(userId+timestamp\)，即以S1为公钥加密数据，server可以用S1解开后验证请求的的有效性

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

登录接口首先会用S1解开data数据，对比解密的userId是否和请求的userId一致，验证请求的有效性，然后会给登录请求下发一个session ticket，服务器从数据库中查询，st使用K\_AS\_SS作为公钥加密\(userId+timestamp+seq\)得到的session ticket，

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

验证session ticket，首先用K\_AS\_SS解密st，得到解密后的userId、timestamp、seq。对比userId是否和和请求的用户的userId相同，不同则说明请求无效，然后从数据库查询该用户的seq值，对比数据库的seq和解密后的seq，如果数据库seq不等于解密后的seq，说明该session ticket 已经是无效的了。最后如果验证都通过，则返回验证成功。这里verifyST接口可以部署在业务服务器，但是要维护业务服务器和AS之间的K\_AS\_SS。

