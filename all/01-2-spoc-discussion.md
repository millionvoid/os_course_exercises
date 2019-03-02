# lec1: 操作系统概述

2016011258 计62 贾明麟

---

## **提前准备**

（请在上课前完成）

* 完成lec1的视频学习和提交对应的在线练习
* git pull ucore\_os\_lab, ucore\_os\_docs, os\_tutorial\_lab, os\_course\_exercises in github repos。这样可以在本机上完成课堂练习。
* 知道OS课程的入口网址，会使用在线视频平台，在线练习/实验平台，在线提问平台\(piazza\)
  * [http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring](http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring)


* 会使用linux shell命令，如ls, rm, mkdir, cat, less, more, gcc等，也会使用linux系统的基本操作。
* 在piazza上就学习中不理解问题进行提问。



# 思考题

## 填空题

* 当前常见的操作系统主要用__C,汇编__编程语言编写。
* "Operating system"这个单词起源于__Oprator__ 。
* 在计算机系统中，控制和管理__各种资源__ 、有效地组织 __多道程序__运行的系统软件称作__操作系统__ 。
* 允许多用户将若干个作业提交给计算机系统集中处理的操作系统称为 __批处理 __操作系统
* 你了解的当前世界上使用最多的操作系统是__Windows__。
* 应用程序通过__系统调用__接口获得操作系统的服务。
* 现代操作系统的特征包括__并行性__ ， __虚拟性__ ， __共享性__ ，__异步性__ 。
* 操作系统内核的架构包括__宏内核__ ， __微内核__ ， __外内核__ 。


## 问答题

- 请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
  - 通用
    - 应该能在不同硬件环境下运行
  - 鲁棒
    - 应该允许用户的各种操作，并对非法操作给出警示而非直接崩溃
    - 应该在各种外部环境或者内部运算出错时拥有错误恢复机制，而不产生丢失用户数据等严重后果
  - 安全
    - 应该能够防范一般性的非法攻击
  - 高效
    - 应该具有高效的内存管理算法等操作系统内部算法，让整个操作系统能够高效处理一般性的工作
  - 易用
    - 应该具有用户友好的操作方式，如果稍显复杂应该有完善的新手引导教程


- 为什么现在的操作系统基本上用C语言来实现？为什么没有人用python，java来实现操作系统？

  - 操作系统是最底层的软件，是其他所有软件运行的基石。而python需要python解释器软件、java需要java虚拟机软件来运行，而这些软件又需要在操作系统之上才能运行，因此操作系统的开发不太可能使用python或者java。而C语言则可以直接编译成机器（汇编）语言，机器语言可以在硬件上直接运行，性能较高而且不需要其他软件的协助。

---

## 可选练习题

---

- 请分析并理解[v9\-computer](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)以及模拟v9\-computer的em.c。理解：在v9\-computer中如何实现时钟中断的；v9 computer的CPU指令，关键变量描述有误或不全的情况；在v9\-computer中的跳转相关操作是如何实现的；在v9\-computer中如何设计相应指令，可有效实现函数调用与返回；OS程序被加载到内存的哪个位置,其堆栈是如何设置的；在v9\-computer中如何完成一次内存地址的读写的；在v9\-computer中如何实现分页机制。


- 请编写一个小程序，在v9-cpu下，能够输出字符


- 输入的字符并输出你输入的字符


- 请编写一个小程序，在v9-cpu下，能够产生各种异常/中断


- 请编写一个小程序，在v9-cpu下，能够统计并显示内存大小



- 请分析并理解[RISC-V CPU](http://www.riscvbook.com/chinese/)以及会使用模拟RISC\-V(简称RV)的qemu工具。理解：RV的特权指令，CSR寄存器和在RV中如何实现时钟中断和IO操作；OS程序如何被加载运行的；在RV中如何实现分页机制。
  - 请编写一个小程序，在RV下，能够输出字符
  - 输入的字符并输出你输入的字符
  - 请编写一个小程序，在RV下，能够产生各种异常/中断
  - 请编写一个小程序，在RV下，能够统计并显示内存大小

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)
