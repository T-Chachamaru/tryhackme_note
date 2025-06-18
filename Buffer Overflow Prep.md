# Buffer Overflow Prep

### 1.溢出FUZZ
首先打开调试工具，放入程序并运行<br>
[Buffer Overflow Prep 1](./iamges/BufferOverflowPrep0.png)<br>
使用nc连到程序开启的端口，观察程序逻辑，有10个用来测试的入口点<br>
[Buffer Overflow Prep 2](./iamges/BufferOverflowPrep1.png)<br>
!mona config -set workingfolder c:\mona\%p创建mona脚本的工作区<br>
[Buffer Overflow Prep 3](./iamges/BufferOverflowPrep2.png)<br>
编写FUZZ脚本，每次增长100byte，直到溢出后，程序崩溃，输出让程序崩溃时的bytes<br>
[Buffer Overflow Prep 4](./iamges/BufferOverflowPrep3.png)<br>
到2200bytes后程序崩溃<br>
[Buffer Overflow Prep 5](./iamges/BufferOverflowPrep4.png)

### 2.确定利用条件
编写出来一个标准的exp栈溢出利用脚本的框架<br>
[Buffer Overflow Prep 6](./iamges/BufferOverflowPrep5.png)<br>
运行kali中的pattern_create.rb脚本，生成一个唯一模式字符串，用来定位EIP的偏移量<br>
[Buffer Overflow Prep 7](./iamges/BufferOverflowPrep6.png)<br>
写入到exp的payload中，重启应用程序，再运行脚本<br>
[Buffer Overflow Prep 8](./iamges/BufferOverflowPrep7.png)<br>
[Buffer Overflow Prep 9](./iamges/BufferOverflowPrep8.png)<br>
!mona findmsp -distance 2600使用mona查找EIP的偏移量<br>
[Buffer Overflow Prep 10](./iamges/BufferOverflowPrep10.png)<br>
测验偏移量是否正确，修改exp的offset与retn并运行，发现EIP被覆盖<br>
[Buffer Overflow Prep 11](./iamges/BufferOverflowPrep11.png)<br>
[Buffer Overflow Prep 12](./iamges/BufferOverflowPrep12.png)<br>
寻找坏字节，用于下步生成正确的shellcode<br>
[Buffer Overflow Prep 13](./iamges/BufferOverflowPrep14.png)<br>
[Buffer Overflow Prep 14](./iamges/BufferOverflowPrep13.png)<br>
[Buffer Overflow Prep 15](./iamges/BufferOverflowPrep15.png)<br>
[Buffer Overflow Prep 16](./iamges/BufferOverflowPrep16.png)<br>
继续寻找坏字节，直到mona脚本找不到坏字节了<br>
[Buffer Overflow Prep 17](./iamges/BufferOverflowPrep17.png)<br>
[Buffer Overflow Prep 18](./iamges/BufferOverflowPrep18.png)<br>
使用mona脚本过滤掉坏字节，寻找jmp指令地址<br>
[Buffer Overflow Prep 19](./iamges/BufferOverflowPrep19.png)

### 3.攻破
把搜索到的jmp地址赋给retn，注意使用小端序<br>
[Buffer Overflow Prep 20](./iamges/BufferOverflowPrep20.png)<br>
msf生成shellcode<br>
[Buffer Overflow Prep 21](./iamges/BufferOverflowPrep21.png)<br>
padding填充，预留解包空间，给exp设好offset、retn、payload<br>
[Buffer Overflow Prep 22](./iamges/BufferOverflowPrep22.png)<br>
运行exp，得到shell<br>
[Buffer Overflow Prep 23](./iamges/BufferOverflowPrep23.png)