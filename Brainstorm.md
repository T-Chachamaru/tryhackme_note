# Brainstorm

### 溢出FUZZ
由于扫全部端口太慢了，首先使用nmap进行端口扫描看看情况，发现了三个端口开放<br>
[Brainstorm 1](./iamges/Brainstorm0.png)<br>
nmap继续扫全端口，上午打的一个靶场刚好涉及到了ftp匿名访问，随便试了一下，发现连了上去<br>
[Brainstorm 2](./iamges/Brainstorm1.png)<br>
把里面的程序都下载下来，这是一个类似于BROP的靶场，因此尝试nc连接不知名的9999端口，发现攻击目标。顺带测试了一下，第二个输入有溢出<br>
[Brainstorm 3](./iamges/Brainstorm2.png)<br>
接下来将程序放到虚拟机中，debug调试运行<br>
[Brainstorm 4](./iamges/Brainstorm3.png)<br>
编写FUZZ脚本<br>
[Brainstorm 5](./iamges/Brainstorm4.png)<br>
FUZZ得到使目标崩溃的输入长度<br>
[Brainstorm 6](./iamges/Brainstorm5.png)

### 确认利用条件
首先编写一个基础的exp框架<br>
[Brainstorm 7](./iamges/Brainstorm6.png)<br>
生成唯一模式字符串，确定EIP的偏移量<br>
[Brainstorm 8](./iamges/Brainstorm7.png)<br>
[Brainstorm 9](./iamges/Brainstorm8.png)<br>
修改脚本，测试偏移量是否正确<br>
[Brainstorm 10](./iamges/Brainstorm9.png)<br>
[Brainstorm 11](./iamges/Brainstorm10.png)<br>
接下来寻找坏字节<br>
[Brainstorm 12](./iamges/Brainstorm11.png)<br>
[Brainstorm 13](./iamges/Brainstorm12.png)<br>
寻找jmp指令地址<br>
[Brainstorm 14](./iamges/Brainstorm13.png)

### 攻破
根据发现的坏字节数生成shellcode<br>
[Brainstorm 15](./iamges/Brainstorm14.png)<br>
确定最后的exp，运行，获得shell<br>
[Brainstorm 16](./iamges/Brainstorm15.png)<br>
[Brainstorm 17](./iamges/Brainstorm16.png)