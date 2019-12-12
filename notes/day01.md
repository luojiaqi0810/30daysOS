# day01

创建git仓库，名为`30daysOS`，clone到本地

创建`\30daysOS\code和\30daysOSnote`

创建`\30daysOS\code\day01\helloos0`

## 1.使用二进制编写镜像

根据书中的指示，Google搜索“Bz1621”，下载一个二进制编辑器，并进行我们的第一个镜像的制作。

对地址范围为000000~168000输入数据，除以下三个部分，其余地方均填0。

1.在000000到000080输入的数据：（直接复制粘贴可能会出问题，应该与编码有关，下同）


```assembly
EB 4E 90 48 45 4C 4C 4F 49 50 4C 00 02 01 01 00
02 E0 00 40 0B F0 09 00 12 00 02 00 00 00 00 00
40 0B 00 00 00 00 29 FF FF FF FF 48 45 4C 4C 4F
2D 4F 53 20 20 20 46 41 54 31 32 20 20 20 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
B8 00 00 8E D0 BC 00 7C 8E D8 8E C0 BE 74 7C 8A
04 83 C6 01 3C 00 74 09 B4 0E BB 0F 00 CD 10 EB
EE F4 EB FD 0A 0A 68 65 6C 6C 6F 2C 20 77 6F 72
6C 64 0A 00 00 00 00 00 00 00 00 00 00 00 00 00
```

2.在0001F0到000200输入的数据：

```assembly
00 00 00 00 00 00 00 00 00 00 00 00 00 00 55 AA
F0 FF 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```

3.在001400输入的数据：

```assembly
F0 FF FF 00 00 00 00 00 00 00 00 00 00 00 00 00
```

将镜像保存为`\30daysOS\code\day01\helloos0\helloos.img`

打开附带光盘文件夹，复制其中得`tolset`文件夹到`\30daysOS\code\`下（建议路径中不要有中文，不过我的路径有中文，也成功启动了），后续我们自己开发的软件也要放到这个文件夹里。



将`tolset\z_new_w`下的`!cons_9x.bat`和`!cons_nt.bat`复制到`\30daysOS\code\day01\helloos0\`

在helloos0中新建`run.bat`，书中此处的内容为：

```bash
copy helloos.img ../z_tools/qemu/fdimage0.bin
..￥/z_tools/make.exe -C ../z_tools/qemu
```

而光盘文件夹中`\OS\projects\01_day\helloos0\run.bat`的内容为：

```bash
copy helloos.img ..\z_tools\qemu\fdimage0.bin
..\z_tools\make.exe	-C ../z_tools/qemu
```

区别在于￥/和\，不知是日语输入的原因还是别的原因，看起来应该就是反斜杠的意思。

因为我们的所用的路径和书中稍有不一致，故将命令改为：

```
copy helloos.img ..\..\tolset\z_tools\qemu\fdimage0.bin
..\..\tolset\z_tools\make.exe	-C ..\..\tolset\z_tools\qemu
```



新建`install.bat`，书中此处的内容为：

```bash
.. ￥/z_tools/imgtol.com w a: helloos.img
```

而光盘文件夹中`\OS\projects\01_day\helloos0\install.bat`的内容为：

```bash
..\z_tools\imgtol.com w a: helloos.img
```

我们的所用的路径和书中稍有不一致，故将命令改为：

```
..\..\tolset\z_tools\imgtol.com w a: helloos.img
```





使用模拟器

双击`!cons_nt.bat`打开命令行，输入`run`，回车，会自动打开QEMU

（查看`!cons_nt.bat`发现内容就只是`cmd`，即打开命令行）

QEMU启动后会显示我们之前在`helloos.img`中想要显示的hello，world，如果失败，请检查镜像的二进制文件是否输入正确。



## 2.使用汇编编写镜像

### 2.1超长源代码

按书中所写，这个超长的源代码跟二进制编辑器输入的内容一模一样，只用到了DB指令（DB为data byte的缩写，意思是往文件里直接写入一个字节），这里我没有实践。

### 2.2正常长度源代码

使用RESB指令缩短代码长度，`RESB 10`表示从现在的地址开始空出10个字节，nask编译器会自动在这些空出来的地址是填上0x00，因为超长源代码中有很多0x00，所以使用RESB指令能把程序减少到22行。

由于找不到书中提到的nask编译器，也懒得找NASM，故直接复制光盘文件夹中的汇编文件

复制`\30天自制操作系统光盘\OS\projects\01_day\helloos1`到`\30daysOS\code\day01\helloos1`

双击`!cons_nt.bat`，输入：

```bash
..\..\tolset\z_tools\nask.exe helloos.nas helloos.img
```

这样就完成了镜像的编译，`\30daysOS\code\day01\helloos1`会出现`helloos.img`，镜像编译指令被作者写成了批处理文件`asm.bat`，但因为路径原因，我们将其内容替换为上面在命令行中输入的内容。

由于我们使用的路径和光盘中路径不同，所以将`helloos0`中的`install.bat`和`run.bat`复制到`helloos1`并覆盖。

双击`!cons_nt.bat`，输入`run`，不出意外的话就会自动打开QEMU并显示hello，world。

### 2.3加工润色

同样复制`\30天自制操作系统光盘\OS\projects\01_day\helloos2`到`\30daysOS\code\day01\helloos2`

由于我们使用的路径和光盘中路径不同，所以将`helloos1`中的`install.bat`和`run.bat`和`asm.bat`复制到`helloos2`并覆盖。

双击`asm.bat`完成编译，出现`helloos.img`

双击`!cons_nt.bat`，输入`run`，不出意外的话就会自动打开QEMU并显示hello，world。



作者对这部分的代码做了加工，加了一些注释将不同逻辑分开说明，使用了DW（data world，2个字节），DD（data double-word，4个字节）指令。

作者提到两点：

+ `DB`指令可以直接写字符串，`DB "hello, world"`

+ `RESB 0x1fe-$`，这里的`$`是一个变量，值为到当前行的字节数，源代码中到这一行，前面输出了132个字节，所以`$=132`，而`0x1fe=256+15*16+14=510`，故`0x1fe-$=510-132=378`，这条指令就是用于连续写入378个字节的0x00。

  使用`$`变量的好处是提高可扩展性，这条命令属于信息显示部分，我们是在部分里输入的要显示的`hello, world`，如果我们想要显示`this is a pen`，因为字符长度不一样，所以到`01fe`之前要填的0x00字节数也不一样，使用`$`就可以使填充0x00的字节数随之变化。

  在第1节中用二进制编写镜像时对0001F0到000200输入的数据做了特别指定，所以才会用`0x1fe`减去`$`。

具体这个指定内容有什么含义，作者给出了说明。

计算机读写软盘时，以512字节为一个单位进行读取，软盘的512字节称为一个扇区，一张软盘空间有1440KB，即1474560字节，共有1474560/512=2880个扇区。

第一个扇区称为启动区，计算机从启动区开始读软盘，然后检查启动区的最后两个字节的内容。如果最后两个字节不是`55 AA`（为什么是55 AA，是当初设计者随便定的），计算机会认为软盘上没有所需的启动程序，就会报错。如果计算机确认了启动区的最后两个字节是`55 AA`，它就认为这个扇区的开头是启动程序，并开始执行。



对于程序中出现的术语，作者提到：

启动区的名称设置：`DB  "HELLOIPL"`，ipl为initial program loader的缩写，启动程序加载器。这个名字必须为8字节，长度不够需要补空格。

