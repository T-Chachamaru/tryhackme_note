# Retro

### 信息收集
nmap扫描开启了3389端口与80端口<br>
[Retro 1](./iamges/Retro1.png)<br>
扫目录看到了站点目录<br>
[Retro 2](./iamges/Retro2.png)

### 敏感信息泄露
该网站是wordpress，用wpscan扫描、爆破后台账号浪费了挺多时间，后面发现网站上暴露了账号密码，不是后台的<br>
[Retro 3](./iamges/Retro3.png)<br>
3389远程连接<br>
[Retro 4](./iamges/Retro4.png)

### 权限提升
这台主机有两种提权方式，一种是UAC提权，一种是内核漏洞，但UAC提权比较麻烦<br>
systeminfo看补丁信息，能看到这台2016只打了一个补丁<br>
[Retro 5](./iamges/Retro5.png)<br>
用CVE-2017-0213提权<br>
[Retro 6](./iamges/Retro6.png)