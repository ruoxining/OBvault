# x86汇编

# Lab记录

!!! failure 
    
    Lab已经全部换掉，这部分作业介绍无法参考了。

- 作业1: 字符串转换
  
    输入一个字符串，小写字母全部转换为大写字母，删除空格后输出。

    记得他有字符串读写的源代码。

- 作业2: 打印ASCII码表
   
    进入图形模式，输出红的ascii码和绿的相应十六进制编号。

    个人感觉先把位移写出来再填里面的输出比较好弄。

    可以参照他的数字用循环左移输出成十六进制代码。

- 作业3: 简易计算器
  
    整体思路：80386语法写，内容都存在内存变量里

    加法思路：高位加高位，低位加低位

    乘法思路：分别乘低位和高位，高位结果加进位

    除法思路：分别除高位和低位，。。后面忘了

    输出十进制思路：高位低位

- 作业4: C代码文件查看器翻译成汇编
  
    他会给一个c源代码，用c和汇编夹心调试把c翻译成汇编。

    逻辑的思路：
    
    寄存器状态保存 + 初始状态 + 判断终止 + 操作 + 储存 + 循环条件 + 跳转 + 寄存器状态恢复


### 6种寻址方式与其作用

| 说明 | 示例 | 作用 | 
| --- | -- | -- |
| 立即寻址 | mov eax,56H | 通常用于赋值 |
| 直接寻址 | mov eax,[1255887H] |	通常用于处理变量 |
| 寄存器寻址 |	mov eax,[edi] |	地址在寄存器中 |
| 寄存器相对寻址 |	mov eax,[edi+20H] |	常用于访问数组和结构 |
| 基址加变址寻址 |	mov eax,[EBP+ESI] |	常用于访问数组 |
| 相对基址加变址寻址 |	mov eax,[EBX+EDI-10H]	| 常用于访问结构 |


## obj dump

源代码文件名mytest.c

```bash
gcc -c -g -o mytest mytest.c
objdump -s -d main.o > main.o.txt
```

目标文件反汇编，同时显示源代码

```bash
gcc -g -c -o main.o main.c
objdump -S -d main.o > main.o.txt
```

显示源代码的同时显示行号

```bash
objdump -j .text -ld -C -S main.o > main.o.txt
```

可执行文件反汇编

```bash
gcc -o main main.c
objdump -s -d main > main.txt
```

同时显示源代码

```bash
gcc -g -o main main.c
objdump -S -d main > main.txt
```


## 期末考试

1. 是非题(10个，每题1分，共10分)
2. 填空(15个，每空2分，共30分)：
3. 程序填空题(3题，每题10分，共30分)
    
    一般都会用stack压入参数
    会给出c语言的原型（？，参数的压入顺序从右到左，caller清理
    pascal，从左到右，callee清理
    stdcall，从右到左，caller清理
    都用ax返回参数
    一般两个空不可以交换。。。
    先自己写一遍再填
    （一般20几行的程序）
    
4. 程序阅读(2题，每题5分，共10分)
    会问运行结果和中间结果（#）（如果有循环，每次循环到都要写，但是不会太多）
    

不会有直接手写一整个程序的题

重点：
函数参数传递，如何构造一个堆栈框架，ebp。。
需要看懂是不是递归，
有一个程序填空会出单步调试，边解密边加密那个。。
不会考保护模式。

## 复习

### Intel 8086/80386 CPU 功能结构

#### 工作方式

1. 从存储器中取一条指令
2. 分析指令的操作码
3. 从存储器中读取操作数
4. 执行指令
5. 写入结果集
6. 回到1

![](./asset/instcycle.png)


运算器进行信息处理，寄存器进行信息存储，控制器控制各种器件工作，总线连接各种器件。

### 16位和32位的80x86的区别 - 操作系统view

- 16位操作系统中的中断调用相当于32位操作系统中的API调用。16位操作系统中的段地址和便宜地址在32位中消失了，在32位操作系统中统一采用平坦的内存地址模式寻址。
- 16位操作系统中的程序运行在RING0级，也就是说普通程序和操作系统运行在同一个级别并拥有最高权限，而32位操作系统中的程序一般只拥有RING3级运行权限，程序的所有操作都受到操作系统控制，若程序要获得RING0操作特权，只能通过驱动程序实现。
- 16位操作系统的可执行文件格式和32位操作系统的可执行文件格式不同，在32位的windows操作系统中，可执行文件的格式叫PE格式，32位的windows操作系统运行在CPU的保护模式之上，而16位的系统则运行在CPU的实模式上。

#### 逻辑地址与物理地址转换：
1234h:0058h 转化成物理地址=12340h+0058h=12398h
补码

#### 标志位
状态标志：CF ZF SF OF AF PF
控制标志：DF(direction flags) TF(trace/trap flag) IF(interrupt flag)

#### 数据在内存中的存放规律：
小端格式。低字节在前，高字节在后。
设ds=1000h, bx=2000h, ax=1234h
Mov ds:[bx], ax
执行后1000:2001指向的字节=12h

### 寄存器
总结

| 寄存器 | 类别 | 用途 |
| --- | --- | --- |
| AX | 数据寄存器 | 算术运算中的主要寄存器，在乘除运算中用来制定被除数，也是乘除运算后结果的默认存储单元。另外I/O指令均使用该寄存器与I/O设备传送信息。 |
| BX | 数据寄存器 | 指令寻址时常用做基址寄存器，存入偏移量或偏移量的构成成分 |
| CX | 数据寄存器 | 在循环指令操作或串处理指令中隐含计数 |
| DX | 数据寄存器 | 在双字节长运算中与AX构成32位操作数，DX为高16位。在某些I/O指令中，DX被用来存放端口地址 |
| SP | 指针及变址寄存器 | 始终是栈顶的位置，与SS寄存器一起构成栈顶数据的物理地址 |
| BP | 指针及变址寄存器 | 系统默认其指向堆栈中某一单元，即提供栈中该单元的偏移量。加段前缀后，BP可作为非堆栈段的地址指针 |
| SI | 指针及变址寄存器 | 与DS联用，指示数据段中某操作的偏移量。在做串处理时，SI指示源操作数地址，并有自动增量和自动减量的功能。变址寻址时，SI与某一位移量共同构成操作数的偏移量 |
| DI | 指针及变址寄存器 | 与DS联用，指示数据段中某操作数的偏移量，或与某一位移量共同构成操作数的偏移量，串处理操作时，DI指示附加段中目的地址，并有自动增量和减量的功能。 |
| CS | 段寄存器 | 存放当前程序的指示代码 |
| DS | 段寄存器 | 存放程序所设计的源数据或结果 |
| SS | 段寄存器 | 以“先入后出”为原则的数据区 |
| ES | 段寄存器 | 辅助数据区，存放串或其它数据 |
| IP | 控制寄存器 | 它始终指向当前将要执行指令在代码段中的偏移量 |
| FR | 控制寄存器 | 控制标志位 |

![reg1](./asset/reg1.png)

![reg2](./asset/reg2.png)

#### 通用寄存器

IA-32架构中一共有4个32位寄存器，用于保存临时数据，这4个通用寄存器可以当作16位用，也可以作8位用。

AX BX CX DX：数据寄存器，每个数据寄存器都可以拆成两个 8 位寄存器独立使用，如 AX 可拆分为 AH 和 AL，BX 拆分为 BH 和 BL 等。H 和 L 分别表示高 8 位和低 8 位。

AX(accumulator)：累加器。在乘除法运算、串运算、 I/O 指令中都作为专用寄存器；
BX (base)：基址寄存器，常用于存档内存地址。

CX (count)：计数寄存器。常用于存放循环语句的循环次数，字符串操作中也常用。

DX (data)：数据寄存器。常常和EAX一起使用。

#### 变址寄存器

存放在变动的内存地址

ESI(source index): 源变址寄存器，通常存放要处理的数据的内存地址。

EDI(destination index)：目的变址寄存器，通常存放处理后的数据的内存地址。

ESI和EDI常用来配合使用完成数据的赋值操作

```nasm
rep movs dword ptr[edi], dword ptr[esi];
```

这句的意思是把ESI指向的内存地址中的内容复制到EDI所指向的内存中，数据长度在ECX中指定。

#### 指针寄存器

ESP（stack pointer）：堆栈指针寄存器。SS：SP指向堆栈的栈顶，因此虽然是通用寄存器，但不应随便改变SP的值。不可以作为通用寄存器使用，ESP存放当前堆栈栈顶的地址，一般情况下，ESP和EBP联合使用来访问函数中的参数和局部变量。
EBP（base pointer）：基址指针寄存器。可以作为通用寄存器用于存放操作数，常用来代替堆栈指针访问堆栈的数据。
EIP：指令指针寄存器，总是指向下一条要执行的指令的地址。
常见的访问堆栈指令：

```nasm
push ebp
mov ebp, esp
sub esp, 78
push esi
push edi
cmp dword ptr [ebp+8], 0
```

ss栈段寄存器
sp栈顶指针寄存器
bp默认的栈寻址寄存器

#### 标志寄存器

标志寄存器EFLAGS一共有32位，在这32位中大部分是保留给编写操作系统的人用的。

IP (instruction pointer)：指令指针寄存器。代码段寄存器 CS 和指令指针寄存器 IP 是 8086CPU 中最关键的两个寄存器。它们分别用来提供当前指令的段地址和偏移地址。即任意时刻，8086CPU 将 CS:IP 指向的内容当做命令执行。每条指令进入指令缓冲器后、执行前，IP += 所读取指令的长度，从而指向下一条指令。用户不能直接访问 IP 寄存器。

FL (flags)：标志寄存器。与其他寄存器一样，标志寄存器也有 16 位，但是标志寄存器只用到其中的 9 位。这 9 位包括 6 个状态标志和 3 个控制标志，参见下面的“标志位”。

OF（Overflow Flag）:溢出标志，溢出时为1，否则置0。两个正数相加变负，或两个负数相加变正会溢出。#

DF （Direction Flag）:方向标志，在串处理指令中控制信息的方向。0:正方向，1：反方向。cld，std。#

IF (Interrupt Flag) :中断标志。禁止中断0，允许中断1。cli，sti。#

AF (Auxiliary carry Flag) :辅助进位标志，有进位时置1，否则置0。

ZF (Zero Flag) :零标志，运算结构为0时ZF位位置1，否则置0。

SF (Sign Flag):符号标志，结果为负时置1，否则置0。#

CF (Carry Flag): 进位标志，进位时置1，否则置0。配套的clc，stc两条设置指令：清除和置1。#

PF (Parity Flag): 奇偶标志。结果操作数中1的个数为偶数时置1，否则置0。

TF：单步调试要用。#


![](./asset/flag.png)

![https://img-blog.csdnimg.cn/20200426105235850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Nhcmxvc1g=,size_16,color_FFFFFF,t_70](https://img-blog.csdnimg.cn/20200426105235850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Nhcmxvc1g=,size_16,color_FFFFFF,t_70)

EFLAGS是实现条件判断和逻辑判断的一种机制，在汇编语言中一般不直接访问EFLAGS寄存器，而是通过指令的操作隐含访问EFLAGS寄存器。

```nasm
cmp dword ptr [ebp+8], 0. // 影响标志位CF，ZF，SF，OF，AF和PF
Jz 99495898 //如果ZF等于1，则跳转到00405898 
```

### 指令

总结

| 指令 | 作用 | 参数 | 改变标志位 |
| --- | --- | --- | --- |
| mov | 赋值 | 被赋值寄存器，【寄存器，内存，值】 | no |
| xchg | 数据交换 | 【寄存器，内存】，【寄存器，内存】 | no |
| push | 进栈 | 源操作数【寄存器】 |  |
| pop | 出栈 | 目的操作数【寄存器】 |  |
| pushf | 标志位进栈 | 无 |  |
| popf | 标志位出栈 | 无 |  |
| lea | Load effect address，寻址，取偏移地址 |  |  |
| lds | 当指令指定的是16位寄存器时，把源操作数存储单元中存放的十六位偏移地址取出存放在寄存器中，然后把源操作数+2的十六位数装入指令指定的段寄存器。当指令指定的是32位寄存器时 把源操作数存储单元中存放的32位偏移地址装入寄存器 然后把 源操作数+4 的16位数装入段寄存器。mem指向的地址,高位存放在DS中,低位存放在reg中. | LDS reg,mem |  |
| les | 把内存中指定位置的双字操作数的低位字装入指令中指定的寄存器、高位字装入ES寄存器。 |  |  |
| cbw | 8位数扩展为16位数，有符号扩充 |  | no |
| cwd | 字(16位)扩展为双字(32位)，有符号？ |  | no |
| add | 加 | OPRDS，OPRDD |  |
| adc | 带进位加（结果含标志位CF的值，=OPRDS＋OPRDD＋CF） | OPRDS，OPRDD |  |
| sub | 减 | OPRDD，OPRDS |  |
| sbb | 带进位减（结果含标志位CF的值，=OPRDD－OPRDS－CF） | OPRDD，OPRDS |  |
| inc | 自增1 | 寄存器 |  |
| dec | 自减1 | 寄存器 |  |
| mul | 32位：被乘数默认为EAX，那么乘积将存放在EDX：EAX中 | 32位乘数 |  |
|  | 16位：被乘数默认为AX那么乘积将放在DX：AX中i | 16位乘数 |  |
|  | 8位：被乘数默认为AL，那么乘积将放在AX | 8位乘数 |  |
| div | 32位：被除数将是EDX：EAX， 最终的商将存放在EAX， 余数将存放在EDX中 | 32位乘数 |  |
|  | 16位：被除数为EAX，最终得到的商放在AX，余数放在EAX的高16位 | 16位乘数 |  |
|  | 8位：被除数是16位，最终得到的商将放在AL中，余数放在AH中 | 8位乘数 |  |
| imul | 无符号乘 |  |  |
| idiv | 无符号除 |  |  |
| xlat | 换码指令，以bx为首地址的，偏移地址为al的内容送给al。 |  |  |
| in | 端口读写指令 | IN AL,21H；表示从21H端口读取一字节数据到AL |  |
| out | 端口读写指令 |  |  |
| and | 按位与 |  |  |
| or | 按位或 |  |  |
| xor | 按位异或 |  |  |
| not | 操作数按位取反 |  |  |
| neg | 操作数按位取反加一 |  |  |
| test | 对两个操作数进行按位与操作。与and不同，不影响目标操作数的值。 |  |  |
| shl | 逻辑左移，将一个寄存器中的值或单元中的数据向左移位，将最后移出的一位写入cf中。最低位用0补充。 |  |  |
| shr | 逻辑右移，将一个寄存器中的值或单元中的数据向左移位，将最后移出的一位写入cf中。最高位用0补充。 |  |  |
| sal | 算术左移，与shl一样，补0 |  |  |
| sar | 算术右移，与shr不一样，算术右移补最高位 |  |  |
| rol | 循环左移 |  |  |
| ror | 循环右移 |  |  |
| rcl | 带进位循环左移，左移的时候移出去的会放在cf？ |  |  |
| rcr | 带进位循环右移 |  |  |
| cmp | 比较 |  |  |
| ja | jump if above |  |  |
| jb | Jump if below |  |  |
| jae | Jump if above or equal |  |  |
| jbe | Jump if below or equal |  |  |
| jg | jump if greater，有符号大于跳转 |  |  |
| jl | jump less，有符号小于跳转 |  |  |
| jge | jump if greater or equal |  |  |
| jle | Jump if less or equal |  |  |
| jc | jump if with carry, CF = 1 |  |  |
| jnc | jump if not with carry, CF = 0 |  |  |
| je = jz | jump if equal, ZF = 1 |  |  |
| jne = jnz | jump if not equal, ZF = 0 |  |  |
| jz | jump if zero, ZF = 1 |  |  |
| jnz | jump if not zero, ZF = 0 |  |  |
| jcxz | jump if cx equals zero |  |  |
| js | SF = 1 |  |  |
| jns | SF = 0 |  |  |
| jo | Jump if overflow, OF = 1 |  |  |
| jno | jump if not overflow, OF = 0 |  |  |
| loop | 循环 | 代码段（？）名 |  |
| clc | clear carry flag，将cf位清零 |  |  |
| stc | set carry flag，CF置1 |  |  |
| cli | clear interrupt endable flag，IF清零，关闭中断 |  |  |
| sti | set interrupt endable flag，IF置位1，打开中断 |  |  |
| CMC | complement carry flag，CF取反 |  |  |
| CLD | clear direction flag，DF清零 |  |  |
| STD | set interrupt endable flag，DF置1 |  |  |
| call | 近调用 |  |  |
| ret | 近返回 |  |  |
| call far ptr | 远调用。三个push一个jmp。push f，push cs，push ip，jump |  |  |
| retf | 远返回。三个pop。指令⽤栈中的数据，修改CS和IP的内容，从⽽实现远转移 |  |  |
| int | 中断指令 |  |  |
| iret | 中断返回 |  |  |
| jmp short | 段内短转移，短是指要跳⾄的⽬标地址与当前地址前后相差不超过128字节 |  |  |
| jmp near ptr | 段内近转移。近是指跳转的⽬标地址与当前地址在⽤⼀个段内，即CS的值不变，只改变EIP的值 |  |  |
| jmp far ptr | 段间转移，远指跳到另⼀个代码段去执⾏，CS/EIP都要改变 |  |  |
| Jmp dword ptr | 段间转移，以内存地址单元处的双字来修改指令，⾼地址内容修改CS，低地址内容 修改IP，内存地址可以以任何合法的⽅式给出 |  |  |
| repe/renpe scasb | 字符串扫描指令。cmp al, es:[di] di++; 当DF=1时，为di-- repne:当ECX!=0并且ZF==0时 重复 repe: cx != 0且zf != 0重复 |  |  |
| repe/renpe cmpsb | 字符串比较指令。⽐较byte ptr ds:[si]与byte ptr es:[di] 当DF=0时，SI++，DI++ 当DF=1时，SI--，DI-- repne:当ECX!=0并且ZF==0时 重复 repe: cx != 0且zf != 0重复 |  |  |
| rep movsb | 字符串移动指令。其中rep表示repeat，s表示string，b表示byte 在执⾏此指令前要做以下准备⼯作： ①ds:si |  |  |
| lodsb | 块装入指令，把SI指向的存储单元读入累加器，lodsb就读入ax，lodsw就读入ax，然后si自动增加或减小1或2 |  |  |
| stosb/stosw/stosd | SI指向的[🔗](https://baike.baidu.com/item/%E5%AD%98%E5%82%A8%E5%8D%95%E5%85%83读入https://baike.baidu.com/item/%E7%B4%AF%E5%8A%A0%E5%99%A8),其中LODSB是读入AL, LODSW是读入AX中, 然后SI自动增加或减小1或2位.当方向标志位DF=0时，则SI自动增加；DF=1时，SI自动减小。 |  |  |
| rep stosb |  |  |  |
| lodsb |  |  |  |

#### 数据传送指令

数据传送指令是为了实现CPU和内存，输入和输出端口之间的数据传送。

mov

```nasm
mov eax, 56 // 将56H传送到eax寄存器
mov esi, dword ptr [eax * 2 + 1]  // 将内存地址为eax*2+1的4字节数据传送到esi寄存器
mov ah, byte ptr [esi * 2 + eax]  // 将内存地址为esi*+eax处的8位数据传送到AH寄存器
```

xchg

寄存器和内存的数据交换，交换的数据可以是8字节、16字节或32字节，必须格式相同

```nasm
xchg eax, edx; // 将edx寄存器的值和eax寄存器的值交换
xchg [esp-55], edi; // 将edi寄存器的值和堆栈地址为[esp-55]处的值交换
```

push pop

push和pop：称为压入堆栈指令和弹出堆栈指令，格式是push src(源操作数)和pop dst(目的操作数)，push指令和pop指令需要匹配出现，否则堆栈会不平衡。push指令将原操作数src压入堆栈，同时esp-4（栈顶指针减一个4位），而pop反之，从堆栈的顶部弹出4字节的数值然后放入dst。在32位的操作系统上，push和pop的操作是以4字节为单位的，push和pop指令常用于向函数传参。

```nasm
push eax // 将eax寄存器的值以4字节压入堆栈，同时esp-4
push dword ptr [12FF8589] // 将堆栈顶部的4字节弹出到内存地址为12FF8589所指地方，同时esp+4
-----------------------------------------------------------------------------
pop dword ptr [12FF8589] // 将堆栈顶部的4字节弹出到内存地址为12FF8589所指的地方，同时esp+4
pop eax // 将堆栈顶部的4字节弹出到eax寄存器，同时esp+4
```

#### 地址传送指令

x86有3条地址传送指令，分别是LEA，LDS和LES。其实LDS和LES指令和段寄存器有关，在32位的windows操作系统上，一般的程序员都不需要管理段寄存器，所以相对而言，LDS和LES寄存器使用得比较少，一般情况下常见的只有LEA指令。

LEA

称为地址传送指令，格式是“LEA DST, ADDR”。LEA将ADDR地址加载到DST，其中ADDR可以是内存，也可以是寄存器，而DST必须是一个通用寄存器。

```nasm
lea eax, [12345678]; // 指令执行后eax寄存器的值为12345678H
mov eax, [12345678]; // 而mov eax, [12345678] 指令执行后eax寄存器的值为内存地址12345678指向的那个数值

// LEA指令可用于算法运算
lea ecx, [ecx + eax*4];  // ecx = ecx + eax * 4
// 相当于计算出ecx+eax*4的数值，在[]里是一个地址，lea取地址后就取到了这个数值
```

#### 算数运算指令

80x86提供了8条加减法指令，4条乘除法指令。

ADD：加法指令

```nasm
add eax, esi; // 将eax寄存器的值加上esi寄存器的值，得出的结果保存在eax的寄存器中
add ebx, dword ptr[12345678] // 将ebx寄存器的值加上内存地址为12345678所在的4字节值，得出的结果保存在ebx寄存器中
// 不同的平台和编译器中，dword占用的字节数不同，在32位的windows中一个word占16字节，dword占32字节
// 64位中一个word占32字节，dword占64字节
```

sub 减法指令

```nasm
sub ecx, 4H; // 将ecx寄存器的值减去4H，得出的结果保存在eax寄存器中
sub byte ptr[eax], ch; // 将内存地址为eax所指向的数据结构按字节为单位和ch寄存器相减，得出的结果按字节为单位保存在eax所指向的位置
```

inc加1指令

```nasm
inc eax; // 将eax寄存器的值加1，得出的结果存放在原来的地方
```

dec减1指令

```nasm
dec edx; // 将dec寄存器的值减1，得出的结果存放在原来的地方
```

cmp比较指令

称比较指令格式是”cmp oper1, oper2”

cmp指令将oper1减去oper2，得出的结果不保存，只是相应地设置寄存器eflags的cf，pf，zf，af，sf和of。也就是说可以通过测试寄存器eflags相关的标志值得知cmp执行后的结果

```nasm
cmp eax, 56H; // 将eax寄存器的值减去56H，得出的结果不保存，并且设置寄存器eflags相关的标志位
```

neg

neg：取补指令，格式是neg oper

neg指令将oper操作数取反，简而言之就是将0减去oper操作数，得出的结果存在oper自身中。

```nasm
neg eax; 
```

mul imul

无符号乘法指令和有符号乘法指令。mul指令隐含了一个参加运算的操作数eax寄存器，将eax寄存器里的值乘oper，结果保存在eax中。如果结果超过32位则高32位使用edx寄存器保存，eax寄存器保存低32位。

```nasm
mul edx; // 将eax寄存器的值乘以edx寄存器的值，得出的结果保存在eax寄存器中
```

div idiv

除法指令和有符号除法指令。

```nasm
div ecx; // 将eax寄存器的值按4字节为单位除以ecx寄存器的值，得出的结果商保存在eax寄存器中，余数保存在edx寄存器中。
div word ptr [esp+36]; // 将eax寄存器的值按word为单位除以堆栈地址为esp+36所指向的数据，得出的结果商保存在eax寄存器中，余数保存在edx寄存器中。
```

### 高级语言中的数据结构与80386间接寻址

![](./asset/segment.png)

BX BP SI DI

BX：

BP：

SI：

DI：

间接寻址：bx，bp，si，di，可以放在方括号内

缺省段址：ds和ss，如果方括号内有bp，一定是ss，bx一定是ds

CS (code segment): 代码段寄存器，用来存储代码段的段地址。

DS (data segment)：数据段寄存器，用来存储数据段的段地址。

SS (stack segment)：堆栈段寄存器，用来存储堆栈段的段地址。

ES (extra segment)：附加数据段寄存器，用来存放附加段的段地址。有时，一个数据段不够用时，我们可以声明一个附加段来存放更多的数据。例如，我们可以声明 2 个数据段，分别用 DS 和 ES 指向。

程序开始运行时，DOS 会把 ds 和 es 赋值为 psp(program segment prefix) 段地址。psp 段位于程序首个段的前面，长度为 100h 字节，其用途是保存当前 exe 相关的一些信息，如 psp:80h 开始存放了 exe 的命令行参数。

间接寻址： 可以⽤作间接寻址的寄存器只有四个：bx, bp, si, di [bx], [bp], [si], [di]是最简单的间接寻址 [bx + si], [bp + si], [bx + di], [bp + di]注意前⾯必须是bx/bp，后⾯必须是di/si [bx+2] [bp-2] [si+1] [di-1] [bx+si+2] [bx+di-2]

[bp+si+1] [bp+di-1] tips：两个寄存器相加的间接寻址⽅式中, bx或bp通常⽤来表示数组的⾸地址, ⽽si或di则⽤来表示下 标。

缺省段址：不含bp的源操作数⼀般都省略的段地址ds，含有bp的源操作数省略了ss，⽽这个默认的段地址是 可以被改变的
![](./asset/segment2.png)

用堆栈传递参数时，如何用[bp+]实现对参数的引用？

bp + 多少就是栈里的多少

王爽《汇编语⾔》第四版 附录4:⽤栈传递参数

```
difcube:
	mov bp, sp
	mov ax, [bp+4]	;a的值送入ax中
	sub ax, [bp+6]	;减栈中b的值
	mov bp, ax
	mul bp
	mul bp
	pop bp
	ret 4

```

#### 其它的笔记

#### x86：

Intel从16位微处理器8086开始的整个CPU芯片系列，系列中的每种型号都保持与以前的各种型号兼容，主要有8086，8088（16位CPU），80186，80286（这两个是过渡产品）， 80386，80486以及以后各种型号的Pentium芯片（32位CPU），通常所说的x86都是指32位CPU

80386: 32位汇编。

80836寄存器

通用寄存器(EAX EBX ECX EDX,ESP,EBP,ESI,EDI)

通用寄存器与8086的寄存器相比,由16位变为了32位

ESP:栈顶

EBP:栈底

EAX，EBX，ECX，EDX通用寄存器

EAX：累加器（乘法的时候存低位）

EBX：基址（［EBX＋100Ｈ］）

ECX：计数（循环的时候计数）

EDX：数据（默认EDX＊10H＋．．．；乘法的时候存高位）

ESI，EDI：变址寄存器

ESI：源变址寄存器

EDI：目的变址寄存器　与EBX基址搭配使用

#### 参考文献

asm_sum.doc

xxjj的《汇编语言考试总结》 [https://www.yuque.com/xianyuxuan/coding/mkte6u](https://www.yuque.com/xianyuxuan/coding/mkte6u)

[[80386\]80x86汇编指令_CarlosX的博客-CSDN博客_80386指令集](https://blog.csdn.net/CarlosX/article/details/105763559)

[80386 算术运算指令，逻辑运算指令，移位指令 (三) _ttzyanswer的博客-CSDN博客](https://blog.csdn.net/ttzyanswer/article/details/2063168)
