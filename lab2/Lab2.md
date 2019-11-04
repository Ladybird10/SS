# 实验二

## 代码

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int hacked()
{
	printf("hacked!");
}

int sub(char* x)
{
	char y[10];
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

* 更改命令参数

<img src="lab2_pic\20191022112609.png" style="zoom:50%;" />

* 编译，看到崩溃界面

<img src="lab2_pic\20191022112733.png" style="zoom:50%;" />

* 根据ascii表，发现0x35303030是5000

<img src="lab2_pic\20191022112951.png" style="zoom:50%;" />

* 属性设置
  * 固定基地址

<img src="lab2_pic\1571715194111.png" alt="1571715194111" style="zoom:50%;" />

* 断点调试多次

<img src="lab2_pic\1571715368806.png" alt="1571715368806" style="zoom:50%;" />

<img src="lab2_pic\1571715407702.png" alt="1571715407702" style="zoom:50%;" />

每次的地址都相同，证明固定基址成功。

* hack函数的地址：004115C0

<img src="lab2_pic\1571715580080.png" alt="1571715580080" style="zoom:50%;" />

但00属于字符串截断，不能成功输入，故而更改基地址

<img src="lab2_pic\1571716032526.png" alt="1571716032526" style="zoom:50%;" />

得新地址

<img src="lab2_pic\1571716077547.png" alt="1571716077547" style="zoom:50%;" />

then，增加了新字符串并传入sub

<img src="lab2_pic\1571716186151.png" alt="1571716186151" style="zoom:50%;" />

出错点，字符8321

<img src="lab2_pic\1571716296426.png" alt="1571716296426" style="zoom:50%;" />

hack：

<img src="lab2_pic\1571716360258.png" alt="1571716360258" style="zoom:50%;" />

更改ov，然后发现要转义得全部转义，遂删除后面的

<img src="lab2_pic\1571716458882.png" alt="1571716458882" style="zoom:50%;" />

![1571716536405](lab2_pic\1571716536405.png)

地址变了

<img src="lab2_pic\1571716590177.png" alt="1571716590177" style="zoom:50%;" />

在sub执行后下断点，继续执行

<img src="lab2_pic\1571716659508.png" alt="1571716659508" style="zoom:50%;" />

虽然报错，但攻击成功了。

<img src="lab2_pic\1571716984740.png" alt="1571716984740" style="zoom:50%;" />