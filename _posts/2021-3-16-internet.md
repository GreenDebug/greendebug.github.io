# 2

## 网络端点

创建网络端点

```c
int sockfd=socket(AF_INET,
		SOCK_STREAM,
		0);
if(sockfd==-1){		
	printf("can't create socket\n");		
	exit(1);
}
```

int socket (int family, int type, int protocol)
family

![image-20210316113230428](https://gitee.com/csjuesz/image/raw/master/20210316113237.png)

type

![image-20210316113315758](https://gitee.com/csjuesz/image/raw/master/20210316113315.png)

protocl通常是0，表示默认协议

AF_INET SOCK_STREAM默认TCP

AF_INET SOCK_DGRAM默认UDP

SOCK_STREAM意识不到报文界限，也许不会返回所有字节，需要若干次调用

SOCK_SEQPACKET 与STREAM类似，基于报文

SOCK_RAW直接访问网络层

![image-20210316113859016](https://gitee.com/csjuesz/image/raw/master/20210316113859.png)

可以采用shutdown来禁止套接字上的输入/输出

int shutdown(int sockfd, int how);

how SHUT_RD关闭读，SHUT_WR关闭写，SHUT_RDWR同时关闭

shutdown允许套接字处于不活跃状态，close只有最后一个活动引用关闭才释放

TCP/IP采用大端字节序，Linux小端字节序

```c
uint32_t htonl(uint32_t hostint32);//网络字节序 32位整数
uint16_t htons(uint16_t hostint16);//网络字节序 16位整数
uint32_t ntohl(uint32_t netint32); //主机字节序 32位整数
uint16_t ntohs(uint16_t netint16);
```

h表示host n表示network l表示long s表示short

地址格式

地址强制转换为sockaddr表示

```c
struct sockaddr {
    sa_family_t sa_family;
    char sa_data[];
}
```

