# Skynet

### 1.信息收集
nmap扫描显示目标主机smb服务有配置错误<br>
![skynet 1](./iamges/skynet0.png)<br>
dirsearch扫描目录发现mail目录<br>
![skynet 2](./iamges/skynet1.png)

### 2.利用smb服务配置错误
使用smbclient枚举用户<br>
![skynet 3](./iamges/skynet2.png)<br>
连接用户，只有anonymous用户可以无密码连接<br>
![skynet 4](./iamges/skynet3.png)<br>
下载共享目录下的所有敏感文件，查看发现可用的密码字典<br>
![skynet 5](./iamges/skynet4.png)<br>
使用hydra爆破web网站的mail登录页面，得到密码<br>
![skynet 6](./iamges/skynet5.png)<br>
![skynet 7](./iamges/skynet6.png)<br>
登陆后看到有一个密码重置邮件，点进去查看，发现了milesdyson的smb密码<br>
![skynet 8](./iamges/skynet7.png)<br>
![skynet 9](./iamges/skynet8.png)<br>
连上去，查看所有敏感文件，里面提到了一个CMS目录<br>
![skynet 10](./iamges/skynet9.png)<br>
![skynet 11](./iamges/skynet10.png)

### 3.使用历史漏洞getshell
隐藏目录是一个个人博客CMS，首先目录扫描一下<br>
![skynet 12](./iamges/skynet11.png)<br>
![skynet 13](./iamges/skynet12.png)<br>
有个后台登录页面，访问看到cms的名字与版本，去搜索相关exp<br>
![skynet 14](./iamges/skynet13.png)<br>
有远程文件包含的漏洞可利用，因此使用php的反向shell，python创建一个简易的http服务，远程包含getshell<br>
![skynet 15](./iamges/skynet14.png)<br>
![skynet 16](./iamges/skynet15.png)<br>
![skynet 17](./iamges/skynet16.png)<br>
得到flag<br>
![skynet 18](./iamges/skynet17.png)

### 4.提升权限
首先检查一下计划任务，看到了backup.sh这个脚本，root启动的<br>
![skynet 19](./iamges/skynet18.png)<br>
查看脚本，发现其中是在归档备份html目录下的文件，去查看一下tar有哪些提权姿势<br>
![skynet 20](./iamges/skynet19.png)<br>
![skynet 21](./iamges/skynet20.png)<br>
tar命令有一个–-checkpoint参数，该参数与--checkpoint-action参数一起使用，当检查点被触发时，会执行指定的命令。因此，我们可以使用这个参数来创建SUID文件，执行提权命令。<br>
![skynet 22](./iamges/skynet21.png)<br>
等待一分钟后，脚本执行完成，/tmp目录下出现bash的副本，并且SUID位被设置，直接-p执行它，提权到root<br>
![skynet 23](./iamges/skynet22.png)