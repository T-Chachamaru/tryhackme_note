# Internal

### 信息收集
访问web服务，是apache的默认站点<br>
[Internal 1](./iamges/Internal0.png)<br>
nmap扫描，没扫描出什么<br>
[Internal 2](./iamges/Internal1.png)<br>
扫目录，发现web服务是个wordpress站点<br>
[Internal 3](./iamges/Internal2.png)<br>
版本是5.4.2，但这个版本找不到可利用的漏洞<br>
[Internal 4](./iamges/Internal3.png)<br>
[Internal 5](./iamges/Internal4.png)

### 暴力破解
首先尝试一下暴力破解后台，用户是admin，得到密码<br>
[Internal 6](./iamges/Internal5.png)<br>
[Internal 7](./iamges/Internal6.png)<br>
直奔后台写shell<br>
[Internal 8](./iamges/Internal7.png)<br>
getshell<br>
[Internal 9](./iamges/Internal8.png)

### 权限提升
找了脚本枚举了一下，www-data的权限太低，没发现有用的信息，最后竟然在opt目录下直接发现了凭证<br>
[Internal 10](./iamges/Internal9.png)<br>
提权后来到用户目录，发现了一个文本，表示在内网还有一个jenkins服务<br>
[Internal 11](./iamges/Internal10.png)<br>
使用ssh端口转发到本地<br>
[Internal 12](./iamges/Internal11.png)<br>
继续暴力破解jenkins后台，得到密码<br>
[Internal 13](./iamges/Internal12.png)<br>
[Internal 14](./iamges/Internal13.png)<br>
进入后台后进入Script Console写反向shell<br>
[Internal 15](./iamges/Internal14.png)<br>
获得172.17.0.2这台主机的shell<br>
[Internal 16](./iamges/Internal15.png)<br>
又上传了枚举脚本，尝试提权，最后竟然又在opt目录下发现了凭证<br>
[Internal 17](./iamges/Internal16.png)<br>
ssh远程连接，获得root权限<br>
[Internal 18](./iamges/Internal17.png)