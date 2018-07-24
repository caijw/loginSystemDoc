# 本系统的登录设计方案

## 借鉴kerberos协议的思想，设计一套安全的登录系统

### 名词定义

AS: Authentication Server，认证服务器

SS: 业务服务器

PWD: 用户的密码明文

H1: PWD的一次哈希，此处哈希算法采用MD5，即 H1 = MD5\(PWD\)

S1: H1的加盐哈希，即 S1 = MD5\(H1 + SALT\)

K\(AS, SS\): AS 和 SS之间的共享对称加密密钥

ST: session ticket，作为appClient访问SS的session ticket，生成规则为用K\(AS, SS\)作为密钥加密userId和时间戳 等数据，ST = K\(AS, SS\)\(userId + timestamp + ...\) 。

### 登录系统交互流程

![&#x767B;&#x5F55;&#x7CFB;&#x7EDF;&#x4EA4;&#x4E92;&#x56FE;](../.gitbook/assets/deng-lu-xi-tong-jia-gou%20%282%29.png)

#### appClient向AS申请ST:

appClient用S1作为对称加密密钥，加密userId和H1，发送给AS

AS收到appClient的请求后，根据userId在数据库中查找S1，解出 userID + H1，判断userID没有被篡改，生成TS，用S1作为密钥加密后发给appClient

#### appClient请求业务服务器服务:

在请求数据中带上请求业务参数数据、userID、ST等

业务服务器收到后，验证ST的有效性，如果ST有效，则返回业务响应数据给appClient。

#### SS如何验证ST:

SS用K\(AS, SS\)解密ST，得到userId，timestamp等，判断userId是否跟用户请求带的userId是否相同，并且判断时间戳是否过期。

### 登录系统安全性

考虑最坏情况下，存储userId 和 S1的数据库被拖库，黑客知道了S1，该设计方案的安全性。

#### 伪造客户端和重放：

黑客即使知道S1，黑客不知道H1，无法算出S1\(H1 + userId\)，因此无法伪造成客户端请求AS。黑客不能通过重放伪造客户端，userId + S1\(userId + H1 + timestamp\)中带了时间戳，过期的登录请求会被拒绝。黑客可以通过窃听ST，用ST直接伪造成客户端请求SS。所以是黑客是可以伪造客户端请求SS的。

#### 中间人攻击：

黑客不知道K\(AS, SS\)，无法伪造ST，所以无法伪造AS服务器，中间人攻击无法施行。

#### 窃听和篡改：

黑客知道了S1，在S1\(ST\)中，可以篡改和窃听ST。

因此拖库造成了一定的安全风险: 黑客可以伪造客户端请求SS，黑客可以窃听和篡改ST。

## 解决本系统的安全风险

为了解决拖库问题，需要在对称加密体系中，引入非对称加密。

### SSL/TLS协议

是一种非对称加密和对称加密结合起来的安全协议，被广泛应用于网络应用中，比如HTTPS，就是在http协议之上再构建一个SSL/TLS协议。

SSL/TLS wiki: [https://en.wikipedia.org/wiki/Transport\_Layer\_Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)

在三次TCP握手建立TCP通道后，SSL/TLS协议会进行4次握手，建立安全加密通道。在四次握手中，协商对称加密算法，交互各自的证书，并且验证证书完整性，最终用非对称加密算法协商完对称加密的秘钥，四次握手结束，安全通道建立，通信双方后续用对称加密算法进行通信。

### 改造后的设计方案

因此，本设计方案只需要进行适当的改造，即可预防拖库造成的风险。

![&#x767B;&#x5F55;&#x7CFB;&#x7EDF;&#x5728;&#x5B89;&#x5168;&#x901A;&#x9053;&#x4E2D;&#x4EA4;&#x4E92;](../.gitbook/assets/deng-lu-xi-tong-an-quan-tong-dao.png)

SSL/TLS协议已经被证明是高效、可靠和安全的协议，HTTPS的广泛应用可以证明这个事实。

加入SSL/TLS后，即使被拖库：

黑客无法伪造客户端与SS通信，即使有S1，黑客也无法窃听SSL/TLS安全通道。

黑客无法窃听和篡改TS，因为用户无法窃听和篡改SSL/TLS安全通道。

## 注册功能设计

注册功能实现了将S1和userId的绑定，注册流程同样是通过SSL/TLS通道进行的，流程如下：



客户端提交H1，AS生成新的userId，用H1+SALT生成H1的加盐哈希，将S1和userId保存为新的用户，将userId返回给用户。

如果没有SSL/TLS通道的保证，这一步是很脆弱的，因为将H1明文在网上上传输很容易被黑客拿到，黑客可以通过查表的方式破解H1得到PWD。

## 单一设备登录设计

用户只能在一个设备上登录，切换登录终端时，其他已经登录的终端会被踢出。为了实现该功能，需要在用户在其他终端登录后，将上一个ST失效。

### 表的改造

在AS的登录验证中，维护了一个数据库表tb\_login\_info，tb\_login\_info表字段大概如下：

| userID | S1 |
| --- | --- |
| kingweicai | abcdefg |

为了保证新的登录生成的ST可以把原来的ST失效，需要给ST绑定一个id，并且将当前登录有效的ST的id保存在tb\_login\_info表中：

| userID | S1 | seq |
| --- | --- |
| kingweicai | abcdefg | 1 |

![&#x5355;&#x4E00;&#x767B;&#x5F55;&#x4EA4;&#x4E92;&#x56FE;](../.gitbook/assets/dan-yi-deng-lu-jiao-hu%20%281%29.png)

### 算法的改造

ST的生成算法需要优化为：ST = K\(AS, SS\)\(userId + timestamp + seq + ...\)

当有新的登录请求，AS在生成ST时，查询tb\_login\_info，知道当前的seq，将seq+1的值作为新的ST生成算法中的seq，并且更新tb\_login\_info中该userId行的seq为seq+1。

SS验证ST时，需要请求验证tb\_login\_info中该userId行的seq值，如果ST解密出来里面的seq比tb\_login\_info中的seq小的，则该ST是失效的ST，返回登录失效的响应，如果相等则是当前有效的ST正常响应用户请求。

当tb\_login\_info中的seq值增长到很大的时候，需要重新从0开始计数。



## 根据配置策略将已经登录的设备提出

读取配置的策略，在tb\_login\_info表中，将命中策略的userId的那一行的值设置为无穷大的值（该值依赖相关数据库的实现），这样设置导致当前所有登录的设备都处于登录失效的状态。

用户重新登录，AS查询tb\_login\_info表中，seq的值为无穷大，则新的seq需要重新从0开始计数，保证新的登录有效。



