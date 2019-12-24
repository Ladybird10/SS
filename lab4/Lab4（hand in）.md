# Lab4（hand in）

## 作业一

结合三个工具dumpbin、process exploer（下载失败）、depends，比较

### dumpbin

* 参考网址

> https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2008/1ez7dh12(v=vs.90)

<img src="D:\软件安全周期\lab4\pic\4.png" alt="4" style="zoom:80%;" />

调用sub，但程序中又没有sub

<img src="D:\软件安全周期\lab4\pic\5.png" alt="5" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\6.png" alt="6" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\7.png" alt="7" style="zoom:80%;" />

cl.exe /c x.c 编译

（已修改代码如下）
<img src="D:\软件安全周期\lab4\pic\9.png" alt="9" style="zoom:80%;" />

link a.obj b.obj /out:hehe.exe 链接，会发现失败

<img src="D:\软件安全周期\lab4\pic\8.png" alt="8" style="zoom:80%;" />

重链接，将包含messagebox的lib也包含进去，就会成功
<img src="D:\软件安全周期\lab4\pic\10.png" alt="10" style="zoom:80%;" />

dumpbin该exe文件，可以看到
<img src="D:\软件安全周期\lab4\pic\11.png" alt="11" style="zoom:80%;" />



## 作业二

新建项

<img src="D:\软件安全周期\lab4\pic\13.png" alt="13" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\14.png" alt="14" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\15.png" alt="15" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\16.png" alt="16" style="zoom:80%;" />

<img src="D:\软件安全周期\lab4\pic\17.png" alt="17" style="zoom:80%;" />

但此时在VS中编译函数不通过，故修改属性

<img src="D:\软件安全周期\lab4\pic\18-1.png" alt="18-1" style="zoom:80%;" />



<img src="D:\软件安全周期\lab4\pic\18-2.png" alt="18-2" style="zoom:80%;" />



<img src="D:\软件安全周期\lab4\pic\18-3.png" alt="18-3" style="zoom:80%;" />



<img src="D:\软件安全周期\lab4\pic\19.png" alt="19" style="zoom:80%;" />



<img src="D:\软件安全周期\lab4\pic\20.png" alt="20" style="zoom:80%;" />

运行exe文件，出现弹窗。

