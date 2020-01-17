# 中文编程语言
## 概述
本项目包含了一个中文编程语言的实现，包括：编译器、解析器、字节码虚拟机
其中字节码虚拟机 支持 Windows Linux 操作系统及 arm、arm32 、x86、x64 CPU

本项目原本为公司内部分享课程《计算机语言编译理》配套源码。经完善后，开源发布出来，使用本项目全部及部分请遵守 MIT 协议。

## 源码组成：
#### / (项目根目录)
    由C#实现的编译器、解析器源代码
#### /bin
    项目的二进制生成目录（由IDE自动创建）

#### /DEMO程序
    中文程序语言样例

#### /lib.import.def
    存储有目标字节码虚拟机宿主函数导入表文件，如果虚拟增加了宿主函数需要修改该文件。

#### /Loader.Linux
    Linux平台加载器实现 (C++)

#### /Loader.WIN32
    Windows平台加载器实现 (C++)

#### /VM
    字节码虚拟机源代码，各平台上的加载器都是引用该源码 (C++)

#### /release.bin.zip
    已编译完成的二进制文件包，如果不想编译源代码，可以直接将该包解压出来直接使用，使用说明，请参考cnpl语言介绍。

## 源码编译：
目前所有代码使用Visual Studio 2017进行编译，其中Linux平台使用Visual Studio 2017中的交叉编译工具进行编译。

**Linux X86** 平台 **工具链为**：g++ (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0  
**Linux arm** 平台 **工具链为**：arm-linux-gnueabihf-g++ (Linaro GCC 7.3-2018.05)  


## 字节码虚拟机实现
虚拟机采用堆栈机,内部维护一个**计算栈**和**数据栈**外加一个**IP**寄存器.  
**计算栈**:虚拟机所有指令运行都是基于计算栈.  
**数据栈**:所有临时计算结果和函数临时变量数据都存储在数据栈  
**IP寄存器**:用于指向指令当前执行位置  

## 其它说明
目前该项目仅仅简单演示一个计算机程序到 CPU 执行的过程，而且为了更易于理解和实现方便，许多地方并未完全按照编译原理的理论来实现。

## CNPL 编译器命令行说明
    -S      [编译选项][必选]，输入的源程序路径。
    -RUN    [运行选项][可选]，使用该选项后，源程序将不会被编译（忽略编译参数），直接解析运行源程序。
    -ARCH   [编译选项][可选][默认值：x86]，选择目标运行平台的机器架构，可用架构系统：
                arm32
                arm64
                x86
                x86_64
    -OS     [编译选项][可选][默认值：windows]，选择目标运行平台的操作系统，可用系统名称：
                linux
                windows
    -I      [链接选项][可选][默认值：根据选择的OS和ARCH自动选择]，选择函数导入表文件，可用名称：
                import.arm32.linux.def
                import.arm32.windows.def
                import.arm64.linux.def
                import.arm64.windows.def
                import.x86.linux.def
                import.x86.windows.def
                import.x86_64.linux.def
                import.x86_64.windows.def
    -T      [链接选项][可选][默认值：EXE]，选择输出文件类型，可用类型：
                ASM     字节码汇编文件。
                BIN     字节码文件。
                EXE     可执行文件。
    -O      [链接选项][可选][默认值：output]，选择输出文件路径。
    -help   [帮助选项][可选]，显示本页信息。
    
    例如需要将DEMO程序 “d:\demo\样例2-排序算法测试.程序” 编译成64 位的Linux 平台上的可执行程序，可输入下列命令：
    cnpl.exe -S d:\demo\样例2-排序算法测试.程序 -ARCH x86_64 -OS linux -T EXE -O 排序算法测试.elf
    如果编译过程没有出错，将得到d:\demo\排序算法测试.elf文件，将该文件复制到64位的Linux系统上可直接运行。
    如果需要编译为windows 平台上运行的EXE程序，则以上命令的OS参数对应更改为windows即可
    
    注：如果无法正确运行，可能是以下问题：
        1、由于默认的加载器是采用 C++11 编译，在windows上依赖vc2017的运行库，linux上依赖libc++6,所以请注意运行库是否已安装
        2、注意目标运行平台的CPU位数，windows上64位可以运行32位程序，反之则不行，linux上则64位只能运行64位程序，32位只能运行32程序。
        3、不排除某些BUG造成无法运行，可以尝试多运行几次，如果还是无法运行，则只能从源码将BUG解决后再试，当然也希望能将解决BUG后的代码提交到本项目。


## CNPL语法说明
    该项目灵感来自于LingDong大神，他创造了文言文编程语言，于是本项目创造了一门白话文编程语言，语法上与他的文言文编程语言有些类似
#### 语言大体规则
    1、名称符号（变量名函数名）采用 [] 或 【】括起来
    2、函数调用使用<>或《》将函数名称 括起来
    3、每行结尾可以用 ;或；结束，但不是必须的，可以没有。
    4、每一个语言块必须使用句号(.或。)结束，比如，循环语句块必须使用、条件跳转语句也必须使用。
    5、函数调用时，如果有多个参数使用逗号（,或，）分开
    
#### 变量定义
    有一个数字0，取名为【甲】；
    有一个数字一百零三，取名为【乙】；
    有一句话:"你好"，取名为【ABC】；
    有一个逻辑量 【否】，取名为【是否结束】；
    有一个阵列：20行 35列，取名为【地图】；
    
#### 函数定义
    有一种方法 接受输入：[n]，取名为 【斐波那契数列顺序求法】：
        ...
        ...
    。
#### 变量赋值
    设【甲】的值为：【甲】、1相减的结果；
    设【甲】的值为：100；
    设【测试数列】的第【i】行0列 的值为：1000;

#### 循环
    下列操作执行【N】次：
        ...
        ...
    。
    
    当【i】小于【n】,执行下列操作：
        ...
        ...
    。
    
    循环可使用 跳出循环 语句跳出
    
#### 条件分支
    如果【n】小于1，
    则：
        ...
        ...
    。
    否则：
        ...
        ...
    。
#### 表达式
##### 变量取值
    【ABC】
    【数列】的第【y】行【x】列
##### 常量
    数字常量 可使用 阿拉伯数字表示的自然数， 也可使用中文表示法
    字符串常量 使用 "" 或“” 括起来中间全部是字符串
    【是】或【真】逻辑真常量
    【否】或【假】 逻辑假常量
##### 四则运算
    【size】、1相加
    【size】、2相减
    【乙】、【甲】相乘
    【size】、5相除
    【乙】、2求余
    以上操作可连续进行如：
    【size】、1相加的结果、5相乘的结果、3求余
##### 逻辑运算
     【i】小于【size】
     【i】大于【size】
     【i】等于【size】
     【i】不等于【size】
     【i】不等于【size】 并且 【乙】不等于【甲】
     【i】不等于【size】 或者 【乙】不等于【甲】
##### 函数调用
    《输出》： “你好”，“大家好”
    《随机数》：20, 100000
    
## 样例说明
在DEMO目录中给出了一些基本样例程序:

#### 样例0-基础测试.程序
    基本语法测试

#### 样例1-斐波那契数列.程序
    斐波那契数列顺序求法测试

#### 样例2-排序算法测试.程序
    排序算法性能测试程序

#### 样例3-贪吃蛇.程序
    典型应用样例
    
![贪吃蛇截图](https://github.com/Zhou-zhi-peng/cnpl/blob/master/DEMO程序/样例3-贪吃蛇.程序.png "贪吃蛇")


