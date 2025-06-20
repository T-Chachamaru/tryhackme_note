# Gatekeeper

### 1.溢出FUZZ
nmap扫到了很多端口，使用-A选项对这些扫描的端口检测一遍<br>
[Gatekeeper 1](./iamges/Gatekeeper1.png)<br>
smb有配置错误，使用了guest账号，级别为user<br>
[Gatekeeper 2](./iamges/Gatekeeper2.png)<br>
smbclient枚举<br>
[Gatekeeper 3](./iamges/Gatekeeper3.png)<br>
连上Users账号，发现了一个二进制程序，初步推断就是靶机的31337端口运行的程序<br>
[Gatekeeper 4](./iamges/Gatekeeper4.png)<br>
在Win10虚拟机中调试运行程序，连上去观察程序逻辑<br>
[Gatekeeper 5](./iamges/Gatekeeper5.png)<br>
[Gatekeeper 6](./iamges/Gatekeeper6.png)<br>
修盖FUZZ脚本<br>
[Gatekeeper 7](./iamges/Gatekeeper7.png)<br>
爆破得到溢出长度<br>
[Gatekeeper 8](./iamges/Gatekeeper8.png)

### 2.确定利用条件
编写一个基本的栈溢出exp框架，生成唯一模式字符串，运行后确定EIP的偏移量<br>
[Gatekeeper 9](./iamges/Gatekeeper9.png)<br>
[Gatekeeper 10](./iamges/Gatekeeper10.png)<br>
[Gatekeeper 11](./iamges/Gatekeeper11.png)<br>
修改exp的offset与retn并运行，发现EIP被覆盖<br>
[Gatekeeper 12](./iamges/Gatekeeper12.png)<br>
mona脚本创建一个坏字节脚本，攻击机修改exp的payload，检测坏字节<br>
[Gatekeeper 13](./iamges/Gatekeeper13.png)<br>
[Gatekeeper 14](./iamges/Gatekeeper14.png)<br>
[Gatekeeper 15](./iamges/Gatekeeper15.png)<br>
mona脚本寻找jmp指令地址<br>
[Gatekeeper 16](./iamges/Gatekeeper16.png)

### 3.攻破
修改exp，启动msfshell并监听，运行exp，得到shell<br>
[Gatekeeper 17](./iamges/Gatekeeper17.png)<br>
[Gatekeeper 18](./iamges/Gatekeeper18.png)<br>
[Gatekeeper 19](./iamges/Gatekeeper19.png)

### 4.提权
在寻找flag的途中就看到了firefox.lnk<br>
[Gatekeeper 20](./iamges/Gatekeeper20.png)<br>
那么将当前的msfshell挂入后台，运行msf的内置脚本寻找firefox缓存<br>
[Gatekeeper 21](./iamges/Gatekeeper21.png)<br>
网上搜索一个解密脚本<br>
[Gatekeeper 22](./iamges/Gatekeeper22.png)<br>
运行脚本，得到新的用户凭证<br>
[Gatekeeper 23](./iamges/Gatekeeper23.png)<br>
用新的用户凭证，远程桌面连上去，提权成功<br>
[Gatekeeper 24](./iamges/Gatekeeper24.png)