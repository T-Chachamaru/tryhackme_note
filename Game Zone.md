# Game Zone

### 1.进入后台
sql注入万能密码绕过登陆<br>
[Game Zone 1](./iamges/Game_Zone1.png)

### 2.获取shell
发现查询页面，测试sql注入后使用sqlmap拖库，顺便再扫下目录和端口<br>
[Game Zone 2](./iamges/Game_Zone2.png)<br>
[Game Zone 3](./iamges/Game_Zone3.png)<br>
[Game Zone 4](./iamges/Game_Zone4.png)

### 3.破解密码哈希
丢去在线网站分析一下加密格式，使用john爆破，得到明文密码<br>
[Game Zone 5](./iamges/Game_Zone5.png)<br>
尝试使用用户账号密码连接ssh，成功<br>
[Game Zone 6](./iamges/Game_Zone6.png)

### 4.ssh端口转发
翻web站时能看到该web站的部分资料是从127.0.0.1本地地址接收了，因此看一下本地web服务<br>
[Game Zone 7](./iamges/Game_Zone7.png)<br>
10000端口有web服务，使用SSL端口转发<br>
[Game Zone 8](./iamges/Game_Zone8.png)<br>
[Game Zone 9](./iamges/Game_Zone9.png)

### 5.提升权限
10000端口的服务是root开启的，搜索一下是否有相关漏洞，使用msf获取shell，得到root权限