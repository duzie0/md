# Web

## cookie和session

### cookie:

HTTP请求是无状态的，即第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。

cookie的出现就是为了解决这个问题。

第一次登录后，服务器返回一些数据（cookie）给浏览器，浏览器保存这些cookie信息。

当该用户第二次发送请求时，请求信息会携带cookie信息发送给服务器，服务器解析cookie信息就可以判断当前用户的身份了。

cookie存储数据量有限，一般不超过4KB，只能存储小量数据。

### session:

session和cookie作用相似，**都是为了存储用户相关信息**。

不同的是，cookie是存储在本地浏览器，而session存储在服务器。

存储在服务器的数据会更加的安全，不容易被窃取。

### cookie和session:

**cookie和session的两种结合方式：**

- **存储在服务端：** 通过cookie存储一个session_id，然后具体的数据则是保存在session中。如果用户已经登录，则服务器会在cookie中保存一个session_id，下次再次请求的时候，会把该session_id携带上来，服务器根据session_id在session库中获取用户的session数据。就能知道该用户到底是谁，以及之前保存的一些状态信息。这种专业术语叫做server side session。
- 将session数据加密，然后存储在cookie中。这种专业术语叫做client side session。flask采用的就是这种方式，但是也可以替换成其他形式。

## HTTP 与 HTTPS

### HTTP

### HTTPS

## TCP 与 UDP 

### TCP

### UDP