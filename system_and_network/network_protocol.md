

## TCP

![image-20200401115415396](images/image-20200401115415396.png)

TCP 是面向连接的，因而双方要维护连接的状态

**端口号**： 对应到 service/app

**序号**：包的编号

**确认序号**：发送出去的包

**状态位**：维护连接的状态

- SYN 发起一个连接
- ACK 回复
- RST 重新连接
- FIN 是结束连接

### 建立连接：三次握手

```text
C: hello, 我是C  (seq x            )  请求

S: hello C,我是S (seq y  , ack x+1)   应答

C: hello S      (seq x+1, ack y+1)    应答之应答
```

![image-20200401120033736](images/image-20200401120033736.png)

