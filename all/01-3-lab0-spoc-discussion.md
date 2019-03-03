# lec2：lab0 SPOC思考题

## **提前准备**
（请在上课前完成，option）

- 完成lec2的视频学习
- git pull ucore_os_lab, os_tutorial_lab, os_course_exercises  in github repos。这样可以在本机上完成课堂练习。
- 了解代码段，数据段，执行文件，执行文件格式，堆，栈，控制流，函数调用,函数参数传递，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在不同操作系统（如linux, ucore,etc.)与不同硬件（如 x86, riscv, v9-cpu,etc.)中是如何相互配合来体现的。
- 安装好ucore实验环境，能够编译运行ucore labs中的源码。
- 会使用linux中的shell命令:objdump，nm，file, strace，gdb等，了解这些命令的用途。
- 会编译，运行，使用v9-cpu的dis,xc, xem命令（包括启动参数），阅读v9-cpu中的v9\-computer.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
- 了解基于v9-cpu的执行文件的格式和内容，以及它是如何加载到v9-cpu的内存中的。
- 在piazza上就学习中不理解问题进行提问。

---

## 思考题

- 你理解的对于类似ucore这样需要进程/虚存/文件系统的操作系统，在硬件设计上至少需要有哪些直接的支持？至少应该提供哪些功能的特权指令？
  - 内存读写 CPU状态读取 CPU运算任务分配 硬盘读写
- 你理解的x86的实模式和保护模式有什么区别？你认为从实模式切换到保护模式需要注意那些方面？
  - 实模式类似开发者模式，能直接操作物理地址的内存，权限极高，但若不规范操作危险性也很大。而且能够访问的地址空间比较小，只有1MB。
  - 保护模式是正常使用模式，系统内核代码所在的内存空间被保护起来，一般应用程序无法访问。而且应用程序给出的内存访问需求会被操作系统进行虚实地址转化，而这一过程对应用程序透明。
  - 需要确认哪些内存需要保护，不允许一般程序读取/修改，并将其严格进行访问控制。
- 物理地址、线性地址、逻辑地址的含义分别是什么？它们之间有什么联系？
  - 物理地址是内存访问总线上的地址数据。
  - 逻辑地址是内存访问代码给出的地址
  - 线性地址是上述二者的中间层，逻辑地址加上段基址就是线性地址。
  - 若启用页机制，线性地址会通过页表查询得到物理地址。若不启用页机制，线性地址就是物理地址。
- 你理解的risc-v的特权模式有什么区别？不同 模式在地址访问方面有何特征？
- 理解ucore中list_entry双向链表数据结构及其4个基本操作函数和ucore中一些基于它的代码实现（此题不用填写内容）
- 对于如下的代码段，请说明":"后面的数字是什么含义
```
 /* Gate descriptors for interrupts and traps */
 struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
 };
```

每个数据的位数。

- 对于如下的代码段，

```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 1,2,3,0);
```
请问执行上述指令后， intr的值是多少？

gete之后32位分别是16位的3和16位的2，由于x86为小端模式，因此其值为2 * 2 ^ 16 + 3 =131075

### 课堂实践练习

#### 练习一

1. 请在ucore中找一段你认为难度适当的AT&T格式X86汇编代码，尝试解释其含义。

   ```c
   lab1_print_cur_status(void) {
       static int round = 0;
       uint16_t reg1, reg2, reg3, reg4;
       asm volatile (
               "mov %%cs, %0;"
               "mov %%ds, %1;"
               "mov %%es, %2;"
               "mov %%ss, %3;"
               : "=m"(reg1), "=m"(reg2), "=m"(reg3), "=m"(reg4));
       cprintf("%d: @ring %d\n", round, reg1 & 3);
       cprintf("%d:  cs = %x\n", round, reg1);
       cprintf("%d:  ds = %x\n", round, reg2);
       cprintf("%d:  es = %x\n", round, reg3);
       cprintf("%d:  ss = %x\n", round, reg4);
       round ++;
   }
   ```

   如图的汇编代码，表示将cs、ds、es、ss四个寄存器里的内容分别赋给reg1-reg4，并在之后的语言代码中将其输出到屏幕

2. (option)请在rcore中找一段你认为难度适当的RV汇编代码，尝试解释其含义。

#### 练习二

宏定义和引用在内核代码中很常用。请枚举ucore或rcore中宏定义的用途，并举例描述其含义。

1.定义变量

```c
#define SECTSIZE        512
```

2.声明空间

```c
#define ELFHDR          ((struct elfhdr *)0x10000)
```

3.声明函数并避免重复声明

```c
#ifndef __KERN_MM_PMM_H__
#define __KERN_MM_PMM_H__

void pmm_init(void);

#endif /* !__KERN_MM_PMM_H__ */
```

## 问答题

#### 在配置实验环境时，你遇到了那些问题，是如何解决的。

还没配。。

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)
