# Daily Bugle

### 1.信息收集
nmap扫描，看到开启了ssh、http、mysql服务，其中检测出了是joomla-cms<br>
![Daily Bugle 1](./iamges/DailyBugle0.png)<br>
dirsearch扫描目录发现了cms配置文件和administrator目录<br>
![Daily Bugle 2](./iamges/DailyBugle1.png)<br>
web.config文件里还有一些rewrite配置<br>
![Daily Bugle 3](./iamges/DailyBugle2.png)<br>
在自述文件里能发现CMS版本是3.7<br>
![Daily Bugle 4](./iamges/DailyBugle3.png)

### 2.利用sqli
搜索历史漏洞，发现存在sql注入漏洞<br>
![Daily Bugle 5](./iamges/DailyBugle4.png)<br>
去网上找一个利用脚本，注出了后台管理员的账号密码<br>
![Daily Bugle 6](./iamges/DailyBugle5.png)<br>
搜索一下加密特征，也可以直接搜索这个cms的3.7.0版本用的什么密码加密<br>
![Daily Bugle 7](./iamges/DailyBugle6.png)<br>
john爆破密码<br>
![Daily Bugle 8](./iamges/DailyBugle7.png)<br>
登陆后台，直奔templates选项修改模板，把index.php重写为反向shell<br>
![Daily Bugle 9](./iamges/DailyBugle8.png)<br>
![Daily Bugle 10](./iamges/DailyBugle9.png)<br>
访问后getshell<br>
![Daily Bugle 11](./iamges/DailyBugle10.png)

### 3.提升权限
首先是去web目录下查看cms配置文件，看看连接数据库的账号密码是什么<br>
![Daily Bugle 12](./iamges/DailyBugle11.png)<br>
尝试了su为root，但发现不成功，看一下home下的其他用户，之后su成功了。这样就可以ssh连上去<br>
![Daily Bugle 13](./iamges/DailyBugle12.png)<br>
![Daily Bugle 14](./iamges/DailyBugle13.png)<br>
开始枚举提权向量，在sudo中发现该用户可以运行yum<br>
![Daily Bugle 15](./iamges/DailyBugle14.png)<br>
尝试提权，成功<br>
![Daily Bugle 16](./iamges/DailyBugle15.png)