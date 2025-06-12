# HackPark

### 1.进入后台
使用burp_suite爆破admin用户，得到密码<br>
[HackPark 1](./iamges/HackPark1.png)

### 2.获取shell
从后台管理页面的关于中得到CMS版本信息，搜索相关历史漏洞，发现有可远程执行代码的exp<br>
[HackPark 2](./iamges/HackPark2.png)<br>
下载exp，根据相关说明信息，将反向ip地址和端口修改成自己的ip和端口，接着按照其中所述流程，上传payload，nc监听，触发反向shell<br>
[HackPark 3](./iamges/HackPark3.png)<br>
[HackPark 4](./iamges/HackPark4.png)<br>
[HackPark 5](./iamges/HackPark5.png)<br>
[HackPark 6](./iamges/HackPark6.png)<br>
[HackPark 7](./iamges/HackPark7.png)<br>
获得shell后，由于ncshell不够稳定，使用msfvenom生成一个payload，使用powershell命令下载，获得msfshell<br>
[HackPark 8](./iamges/HackPark8.png)<br>
[HackPark 9](./iamges/HackPark9.png)<br>
[HackPark 10](./iamges/HackPark10.png)<br>
[HackPark 11](./iamges/HackPark11.png)<br>
[HackPark 12](./iamges/HackPark12.png)

### 3.权限提升
得到了稳定的msfshell，可以继续枚举系统信息，提升权限，使用winPEAS枚举脚本进行，同样首先进行上传<br>
[HackPark 13](./iamges/HackPark13.png)<br>
然后运行winPEAS，枚举系统信息，可以看到windows计划任务的目录可写<br>
[HackPark 14](./iamges/HackPark14.png)<br>
进入计划任务目录，查看相关log，看到Message.exe文件有计划任务运行<br>
[HackPark 15](./iamges/HackPark15.png)<br>
那么重新启动一个msf终端，生成一个反向shell，并上传到计划任务目录，替换Message.exe<br>
[HackPark 16](./iamges/HackPark16.png)<br>
[HackPark 17](./iamges/HackPark17.png)<br>
[HackPark 18](./iamges/HackPark18.png)<br>
[HackPark 19](./iamges/HackPark19.png)<br>
得到管理员权限<br>
[HackPark 20](./iamges/HackPark20.png)<br>