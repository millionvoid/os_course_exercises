# lec 3 SPOC Discussion

## **提前准备**
（请在上课前完成）


 - 完成lec3的视频学习和提交对应的在线练习
 - git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises  　in github repos。这样可以在本机上完成课堂练习。
 - 仔细观察自己使用的计算机的启动过程和linux/ucore操作系统运行后的情况。搜索“80386　开机　启动”
 - 了解控制流，异常控制流，函数调用,中断，异常(故障)，系统调用（陷阱）,切换，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在linux, ucore, v9-cpu中的os*.c中是如何具体体现的。
 - 思考为什么操作系统需要处理中断，异常，系统调用。这些是必须要有的吗？有哪些好处？有哪些不好的地方？
 - 了解在PC机上有啥中断和异常。搜索“80386　中断　异常”
 - 安装好ucore实验环境，能够编译运行lab8的answer
 - 了解Linux和ucore有哪些系统调用。搜索“linux 系统调用", 搜索lab8中的syscall关键字相关内容。在linux下执行命令: ```man syscalls```
 - 会使用linux中的命令:objdump，nm，file, strace，man, 了解这些命令的用途。
 - 了解如何OS是如何实现中断，异常，或系统调用的。会使用v9-cpu的dis,xc, xem命令（包括启动参数），分析v9-cpu中的os0.c, os2.c，了解与异常，中断，系统调用相关的os设计实现。阅读v9-cpu中的cpu.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
 - 在piazza上就lec3学习中不理解问题进行提问。

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
-  请描述在“计算机组成原理课”上，同学们做的MIPS CPU是从按复位键开始到可以接收按键输入之间的启动过程。
   -  初始化PC,CPU状态,地址空间
   -  初始化寄存器
   -  内存控制器初始化
   -  加载程序或者OS并跳转到程序或者OS的入口处
-  x86中BIOS从磁盘读入的第一个扇区是是什么内容？为什么没有直接读入操作系统内核映像？
   -  加载程序
   -  太大
-  比较UEFI和BIOS的区别。
   -  BIOS固化在计算机主板上，针对特定的计算机
   -  UEFI则接口标准，对所有平台都提供一致的操作系统启动服务
-  理解rcore中的Berkeley BootLoader (BBL)的功能。

## 3.2 系统启动流程

- x86中分区引导扇区的结束标志是什么？
  - 55AA
- x86中在UEFI中的可信启动有什么作用？
  - 方便从不同硬件上进行启动，并通过数据签名检查其安全性
- RV中BBL的启动过程大致包括哪些内容？

## 3.3 中断、异常和系统调用比较
- 什么是中断、异常和系统调用？
  - 中断：外设对CPU发出信号，使CPU暂时停下当前正在做的工作，跳转到内核态去处理该外设所请求的任务，处理完毕后恢复原任务。
  - 异常：CPU运行过程中发生意想不到的情况，导致CPU必须暂停当前工作，跳转到内核态处理该情况。
  - 系统调用：CPU运行用户程序过程中，需要用到系统特权指令，跳转到内核态运行该指令之后返回到用户态。
- 中断、异常和系统调用的处理流程有什么异同？
  - 异：
    - 源头不同：外设、运行过程中意想不到的情况、运行过程中对系统内核功能的调用
    - 响应方式不同：异步、同步、异步
    - 处理机制不同：对应用程序透明、杀死程序或重新执行出现异常的指令、等待
  - 同：
    - 都会引起CPU从用户态向内核态的跳转
- 以ucore/rcore lab8的answer为例，ucore的系统调用有哪些？大致的功能分类有哪些？
  - sys_exit
    sys_fork
    sys_wait
    sys_exec
    sys_yield
    sys_kill
    sys_getpid
    sys_putc
    sys_pgdir
    sys_gettime
    sys_lab6_set_priority
    sys_sleep
    sys_open
    sys_close
    sys_read
    sys_write
    sys_seek
    sys_fstat
    sys_fsync
    sys_getcwd
    sys_getdirentry
    sys_dup
  - 系统操作、外设操作、进程操作、文件操作

## 3.4 linux系统调用分析
- 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(仅实践，不用回答)
- 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(仅实践，不用回答)


## 3.5 ucore/rcore系统调用分析 （扩展练习，可选）
-  基于实验八的代码分析ucore的系统调用实现，说明指定系统调用的参数和返回值的传递方式和存放位置信息，以及内核中的系统调用功能实现函数。
- 以ucore/rcore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
- 以ucore/rcore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。


## 3.6 请分析函数调用和系统调用的区别
- 系统调用与函数调用的区别是什么？
  - 系统调用的实现必然包含内核功能，而函数调用则不必
- 通过分析x86中函数调用规范以及`int`、`iret`、`call`和`ret`的指令准确功能和调用代码，比较x86中函数调用与系统调用的堆栈操作有什么不同？
  - 函数调用内外均在用户态，没有堆栈切换
  - 系统调用中，用户态和内核态使用的是不同的堆栈，需要进行切换和反切换
- 通过分析RV中函数调用规范以及`ecall`、`eret`、`jal`和`jalr`的指令准确功能和调用代码，比较x86中函数调用与系统调用的堆栈操作有什么不同？


## 课堂实践 （在课堂上根据老师安排完成，课后不用做）
### 练习一
通过静态代码分析，举例描述ucore/rcore键盘输入中断的响应过程。

### 练习二
通过静态代码分析，举例描述ucore/rcore系统调用过程，及调用参数和返回值的传递方法。
