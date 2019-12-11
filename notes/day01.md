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

将镜像保存为helloos.img



打开附带光盘文件夹，复制其中得tolset文件夹到任意位置，后续我们自己开发的软件也要放到这个文件夹里。



在tolset中新建helloos0文件夹，将helloos.img复制进来，并将tolset/z_new_w下的!cons_9x.bat和!cons_nt.bat复制进来。



在helloOS0中新建run.bat，内容为：

```bash
copy helloos.img ../z_tools/qemu/fdimage0.bin
..￥/z_tools/make.exe -C ../z_tools/qemu
```

```bash
copy helloos.img ..\z_tools\qemu\fdimage0.bin
..\z_tools\make.exe	-C ../z_tools/qemu
```



新建install.bat，内容为：

```bash
.. ￥/z_tools/imgtol.com w a: helloos.img
```

```bash
..\z_tools\imgtol.com w a: helloos.img
```

