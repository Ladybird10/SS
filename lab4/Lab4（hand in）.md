# Lab4（hand in）

## 作业一

结合三个工具dumpbin、process exploer、depends，比较

代码网址：

 https://docs.microsoft.com/zh-cn/windows/win32/toolhelp/taking-a-snapshot-and-viewing-processes 

注意：将.cpp改成.c/增加#include<stdio.h>/在工程上右键，设为启动项

代码修改

1. 

<img src="pic\微信截图_20191216184614.png" alt="1" style="zoom:80%;" />

2. 53行到77行（while前）注释掉

效果

<img src="pic\微信截图_20191216184425.png" alt="2" style="zoom:100%;" />

取消注释

<img src="pic\微信截图_20191216190434.png" alt="3" style="zoom:80%;" />

用process explore得到的结果也相同

<img src="pic\26.png" alt="26" style="zoom:80%;" />

## 作业二

* 参考网址

> https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2008/1ez7dh12(v=vs.90)

<img src="pic\4.png" alt="4" style="zoom:80%;" />



调用sub，但程序中又没有sub

<img src="pic\5.png" alt="5" style="zoom:80%;" />

<img src="pic\6.png" alt="6" style="zoom:80%;" />

<img src="pic\7.png" alt="7" style="zoom:80%;" />

cl.exe /c x.c 编译

（已修改代码如下）
<img src="pic\9.png" alt="9" style="zoom:80%;" />

link a.obj b.obj /out:hehe.exe 链接，会发现失败

<img src="pic\8.png" alt="8" style="zoom:80%;" />

重链接，将包含messagebox的lib也包含进去，就会成功
<img src="pic\10.png" alt="10" style="zoom:80%;" />

dumpbin该exe文件，可以看到
<img src="pic\11.png" alt="11" style="zoom:80%;" />


新建项

<img src="pic\13.png" alt="13" style="zoom:80%;" />

<img src="pic\14.png" alt="14" style="zoom:80%;" />

<img src="pic\15.png" alt="15" style="zoom:80%;" />

<img src="pic\16.png" alt="16" style="zoom:80%;" />

<img src="pic\17.png" alt="17" style="zoom:80%;" />

但此时在VS中编译函数不通过，故修改属性

<img src="pic\18-1.png" alt="18-1" style="zoom:80%;" />



<img src="pic\18-2.png" alt="18-2" style="zoom:80%;" />



<img src="pic\18-3.png" alt="18-3" style="zoom:80%;" />



<img src="pic\19.png" alt="19" style="zoom:80%;" />



<img src="pic\20.png" alt="20" style="zoom:80%;" />

运行exe文件，出现弹窗。



* 第二步调用方式称为load time 特点是exe文件导入表中会出先需要调用的dll文件名及函数名，并且在link 生成exe是，需明确输入lib文件。还有一种调用方式称为 run time。参考上面的链接，使用run time的方式，调用dll的导出函数。包括系统API和第一步自行生成的dll，都要能成功调用。

执行代码

```c
// A simple program that uses LoadLibrary and 
// GetProcAddress to access myPuts from Myputs.dll. 
 
#include <windows.h> 
#include <stdio.h> 
 
typedef int (__cdecl *MYPROC)(LPWSTR); 
 
int main( void ) 
{ 
    HINSTANCE hinstLib; 
    MYPROC ProcAdd; 
    BOOL fFreeResult, fRunTimeLinkSuccess = FALSE; 
 
    // Get a handle to the DLL module.
 
    hinstLib = LoadLibrary(TEXT("MyPuts.dll")); 
 
    // If the handle is valid, try to get the function address.
 
    if (hinstLib != NULL) 
    { 
        ProcAdd = (MYPROC) GetProcAddress(hinstLib, "myPuts"); 
 
        // If the function address is valid, call the function.
 
        if (NULL != ProcAdd) 
        {
            fRunTimeLinkSuccess = TRUE;
            (ProcAdd) (L"Message sent to the DLL function\n"); 
        }
        // Free the DLL module.
 
        fFreeResult = FreeLibrary(hinstLib); 
    } 

    // If unable to call the DLL function, use an alternative.
    if (! fRunTimeLinkSuccess) 
        printf("Message printed from executable\n"); 

    return 0;

}
```

 运行时链接可以编译通过并且执行，不会像加载时链接一样失败

<img src="pic\21.png" alt="21" style="zoom:80%;" />

通过编译链接方式进行

<img src="pic\23.png" alt="23" style="zoom:80%;" />

<img src="pic\24.png" alt="24" style="zoom:80%;" />

此处后续重新创建了dll，名为run.dll

<img src="pic\22.png" alt="22" style="zoom:80%;" />

重新运行，成功

<img src="pic\25.png" alt="25" style="zoom:80%;" />