# relevant

### 信息收集
首先扫描端口，发现存在80、139、445、3389等端口<br>
[relevant 1](./iamges/relevant0.png)<br>
扫80端口的目录，什么都没扫出来<br>
[relevant 2](./iamges/relevant2.png)<br>
之后扫全部端口才发现49663端口也有个web服务<br>
[relevant 3](./iamges/relevant14.png)<br>
扫描发现挂在了共享目录下<br>
[relevant 4](./iamges/relevant5.png)

### smb错误配置
枚举smb用户<br>
[relevant 5](./iamges/relevant1.png)<br>
尝试登陆用户，密码默认为空，发现了一个密码文件，下载下来<br>
[relevant 6](./iamges/relevant3.png)<br>
密码文件中有base64编码，解码后得到两个用户名和密码<br>
[relevant 7](./iamges/relevant4.png)<br>
之前没有扫到第二个web服务，尝试使用得到的用户和密码远程登陆服务均无效，访问第二个web服务时才发现可以访问到共享目录，这代表可以上传webshell<br>
[relevant 8](./iamges/relevant6.png)<br>
[relevant 9](./iamges/relevant7.png)<br>
监听端口，msf生成payload，使用smb服务上传webshell<br>
[relevant 10](./iamges/relevant8.png)<br>
[relevant 11](./iamges/relevant9.png)<br>
geshell<br>
[relevant 12](./iamges/relevant10.png)

### 权限提升
惯例首先看拥有的权限，发现有SeImpersonatePrivilege<br>
[relevant 13](./iamges/relevant11.png)<br>
上传个利用程序<br>
[relevant 14](./iamges/relevant12.png)<br>
运行后获得系统权限<br>
[relevant 15](./iamges/relevant13.png)