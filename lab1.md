# 实验报告一

## 实验目的

* 探索二进制缓冲区溢出漏洞

## 实验准备

* VS code

  * 基本配置

    建立.c文件，而不是.cpp文件

    所有选项->SDL检查：否

    代码生成->启用C++异常：否

    ​					安全检查：禁用

    ​					基本运行时检查：置空

    调试->命令行参数：输入长字符串

    配置文件：debug x86

  * 示例代码

  ```c
  #define _CRT_SECURE_NO_WARNINGS
  
  #include <stdlib.h>
  #include <stdio.h>
  #include <string.h>
  
  int sub(char* x)
  {
  	char y[10];//此处下断点调试
  	strcpy(y, x);
  	return 0;
  }
  
  int main(int argc, char** argv)
  {
  	if (argc > 1)
  		sub(argv[1]);
  	printf("exit");
  }
  ```

## 实验过程

1. 在第9行下断点，调试，进入调试界面后右键，在反汇编窗口中勾选显示地址、显示符号名

<img src="lab1_pic\1.png" alt="pausepoint" style="zoom:50%;" />

<img src="lab1_pic\2.png" alt="show1" style="zoom:50%;" />

2. 地址中分别输入EIP和006E18C9，发现EIP与该地址指向同一位置，可知EIP为指令指针寄存器

<img src="lab1_pic\3_address.png" alt="address" style="zoom:50%;" />

<img src="lab1_pic\3_EIP.png" alt="EIP" style="zoom:50%;" />

3. 单步调试，寄存器值出现变化

<img src="lab1_pic\4_move.png" alt="move1" style="zoom:50%;" />

EIP指向新命令的地址

EAX值是之前我们所输入的长字符串999999...，也代表着strcpy中的参数x

<img src="lab1_pic\4_EAX.png" alt="eax" style="zoom:50%;" />

4. 在主函数中下断点，转入反汇编，逐步执行

当前执行到call sub指令，逐语句可见jmp指令，而后跳转至sub函数

<img src="lab1_pic\5_main.png" alt="main_asb" style="zoom:50%;" />

<img src="lab1_pic\5_jmp.png" alt="jmp" style="zoom:50%;" />

<img src="lab1_pic\5_sub.png" alt="sub" style="zoom:50%;" />

5. esp值将自身值赋给了ebp

<img src="lab1_pic\6_ebpesp.png" alt="espandebp" style="zoom:50%;" />

> 1）ESP：栈指针寄存器(extended stack pointer)，其内存放着一个指针，该指针永远指向系统栈最上面一个栈帧的栈顶。
>
> 2）EBP：基址指针寄存器(extended base pointer)，其内存放着一个指针，该指针永远指向系统栈最上面一个栈帧的底部。
> 根据上述的定义,在通常情况下ESP是可变的,随着栈的生产而逐渐变小,而ESB寄存器是固定的,只有当函数的调用后,发生入栈操作而改变。
>
> 而ebp寄存器的出现则是为了另一个目标，通过固定的地址与偏移量来寻找在栈参数与变量。而这个固定值者存放在ebp寄存器中，。但是这个值会在函数的调用过程发生改变。而在函数执行结束之后需要还原，因此，在函数的出栈入栈过程中进行保存。
>
> ————————————————
> 原文链接：https://blog.csdn.net/u011822516/article/details/20001765

6. 调用strcpy函数

<img src="lab1_pic\7_strcpy.png" alt="callstrcpy" style="zoom:50%;" />

<img src="lab1_pic\7_strcpy2.png" alt="strcpy2" style="zoom:50%;" />

7. 结束调用

<img src="lab1_pic\8_strcpyend.png" alt="strcpyend" style="zoom:50%;" />

<img src="lab1_pic\8_strcpyend2.png" alt="strcpyend2" style="zoom:50%;" />

ebp将地址归还给esp，以便esp找回之前的位置。

8. 此时ebp的值出现了改变，变成了之前我们输入的长字符串

<img src="lab1_pic\9_end.png" alt="end1" style="zoom:50%;" />

程序报错

<img src="lab1_pic\9_end2.png" alt="end2" style="zoom:50%;" />

此处因为在该地址找不到有效的指令而报错，但如果攻击者输入的地址真正指向一段可执行脚本，那么程序将被攻击者利用。

## 扩展：跨站脚本攻击

> 存储型XSS：存储型XSS，持久化，代码是存储在服务器中的，如在个人信息或发表文章等地方，插入代码，如果没有过滤或过滤不严，那么这些代码将储存到服务器中，用户访问该页面的时候触发代码执行。这种XSS比较危险，容易造成蠕虫，盗窃cookie
>
> 反射型XSS：非持久化，需要欺骗用户自己去点击链接才能触发XSS代码（服务器中没有这样的页面和内容），一般容易出现在搜索页面。反射型XSS大多数是用来盗取用户的Cookie信息。
>
> DOM型XSS：不经过后端，DOM-XSS漏洞是基于文档对象模型(Document Objeet Model,DOM)的一种漏洞，DOM-XSS是通过url传入参数去控制触发的，其实也属于反射型XSS
> ————————————————
> 原文链接：https://blog.csdn.net/qq_36119192/article/details/82469035