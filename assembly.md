# 汇编/嵌入式/计算机组成 

## 基础知识 

### 计算机 

**计算机系统主要的三个层次结构**

- 硬件
  - 主机
    - 主板
      - CPU
      - 内存
    - 接口卡/IO接口
  - 外设
- 系统软件：提供基本操作界面，提供对应用软件的支持
  - 操作系统（功能：存储、进程、设备、网络通信、安全性能管理，命令处理）
    - 交互（一般电脑）
    - 实时（常用于嵌入式系统）
  - 编译程序
  - 解释程序
- 应用软件

虚拟机：指通过软件模拟的具有完整硬件系统功能的、运行在一个完全隔离环境中的完整计算机系统 

实际硬件以上的层次都称为虚拟机，是由软件构成的外部特性（下层特征向上透明）

**杂** 

现在很多电脑将CPU、内存、IO接口（当然还有总线）都集成在一起，构成嵌入式计算机/单片机/微控制器（Micro Control Unit） 

固件（Firmware）的定义：固定不变的软件固定在硬件中，硬件为实际形态，按软件设计，介于两者之间，如ROM内存中的BIOS，以及大部分嵌入式系统中的程序 

编程语言用于编写软件但不是软件 

计算机系统大部分功能既可用硬件实现，又可用软件实现。这两种实现在逻辑上是等效的，其区别在于速度、成
本、可靠性、存储容量、变更周期等因素 

**冯诺依曼计算机模型**（#图片）

- 控制器（内含译码器、指令寄存器、时钟等）
- 运算器（内含ALU、寄存器）
- 存储器（寄存器也属于存储器的一部分）
- 输入、输出单元

### 存储器 

**存储器的层次结构** 

- 寄存器（CPU中）
- 高速缓存（Cache），存储被频繁用到的程序和数据，由寄存器组成
- 内存/主存储器
  - RAM
  - ROM
- 外存/辅助存储器（是外设）
  - 磁带，顺序访问
  - 磁盘，部分顺序访问（RAM+顺序访问）
  - U盘、光盘等等……

**两种计算机结构**

- 冯诺依曼结构（普林斯顿结构）：单存储器储存数据与指令
- 哈佛结构：双存储器分别储存数据与指令

**存储器的组织**

- 每个存储单元是1字节
- 位扩展：地址总线可寻址的空间大于一个存储器的空间，将几个存储器“串联”
- 字扩展：数据总线决定的一个字大于8位，将几个存储器“并联”（但地址仍是一维线性的）

### 总线 

**总线上设备分类**

- 主设备：能够启动总线服务的设备
- 从设备：只能等待启动命令的被动设备

**总线功能层次分类**

- 芯片级总线：如CPU内部总线
- 板机总线/局部总线
- 系统级总线：连接系统中各个模块

**板级总线**（CPU、内存、IO间的交互） 

- 地址总线（address bus）：cpu出发在内存、IO口中寻找指定地址（*单向*），n位地址总线 <=> n条线 <=> 宽度为n <=> 可以寻找2^n个内存单元（一内存单元 = 1 Byte），地址总线所能寻到的地址即为内存地址空间
- 控制总线（control bus）：传输读、写等控制，越宽则能传输的控制越多，控制能力越强，每根线都是单向的但是有两种方向（*准双向*）
- 数据总线（data bus）：传送数据，越宽速度越快，每根线都是*双向*的，n位计算机 <=> n位数据总线 <=> 一个字是n位

三条总线与CPU内部的输入输出控制器相连 

### 微机性能指标 

- 主频：晶体振荡器频率，即时钟频率
- 字长
- 内存容量
- 存取周期：内存读写一次的时间
- 响应时间：系统响应用户请求的时间
- 吞吐率：反应响应用户请求的速率
- 运算速度：每秒执行指令数

## CPU（central process unit）/微处理器 

**CPU的组成** 

- 运算器
  - 算术逻辑单元（Arithmetic Logic Unit）：n位CPU的运算器一次可处理n位（1个字）的数据
  - 寄存器：n位CPU的通用寄存器宽度为n位，寄存器与运算器之间的通路是n位的
- 控制器：读取内存中的代码，控制CPU内各器件的工作
  - 译码器：指令翻译成控制信号
  - 指令寄存器：存放指令
  - 时序节拍发生器：时钟
  - 指令计数器：计算并指出下一条指令的地址
  - 操作控制部件：组合控制信号

通过内部总线（bus）连接 

**流水线技术** 

8086将CPU的工作循环分为取指令、译码执行两个部分，采用流水线的方式工作 

译码执行某条指令的同时读取下一条指令，储存在指令流队列中，待当前指令译码执行完后可立即执行下一条指令 

现在的CPU将工作循环分为更多的部分，流水线的效率更高 

**CPU的时序** 

- 时钟周期：内部晶振产生的时钟信号周期
- 总线周期：访问一次内存的时间，通常包含多个时钟周期
- 指令周期：执行一条指令的时间，由于每个指令周期执行必然要先从内存中读指令，故至少包含一个总线周期
- 中断响应周期：每条指令周期最后一个时钟采样CPU的INTR引脚，决定是否进入中断响应周期

**8086CPU** 

- 16位数据总线，16位寄存器，20位地址总线
- 引脚复用
- 单总线结构
- 累加器结构
- 三态电路：高电平、低电平、高阻态
- 总线分时复用

## 内存（主存储器） 

**内存的组成** 

- RAM：随机存储器，可以读取任意地址的内容（断电后数据会遗失）
- ROM：只读存储器（装有BIOS：Basic Input/Output System，硬件的基本输入输出程序，主板、某些接口卡都有这样的程序）

**数据的存储方式** 

“小数端”：所有内存单元构成的储存空间是一维线性的，数据高位对应高地址，低位对应低地址（小数端易于计算，大数端易于比较），例： 

`mov ax,0123H`这句汇编代码在内存中储存的形式是：B8（B8表示mov ax，假设此地址为10000），23（地址：10001），01（地址：10002）

此时，0123H为内存地址10001上储存的字型数据（8086中，1 word = 2 Byte） 

字对齐与字不对其：多个存储器（每个单元1字节）并联，组成“字” 

**段** 

提高寻址能力，以8086（16位CPU）为例，8086有20位数据总线，CPU内部采用两个16位地址（段地址、偏移地址）通过地址加法器合成20位地址后输出到地址总线 

`内存地址 = 段地址 * 16 + 偏移地址`，可以表示为“段地址:偏移地址”的形式，同一个内存物理地址可以有多种分解方式 

## 寄存器 

（#图片）

### 通用寄存器 

通用寄存器：AX、BX、CX、DX（可存储一个字，8086中是16位） 

可以当两个独立8位寄存器使用（为了向下兼容），例如AX可以分为AH（high）、AL（low） 

**通用寄存器的特殊用途** 

- A（Accumulator）X，累加器；且所有IO指令都使用AX
- B（Base）X，通常与DS配对使用
- C（Count）X，循环和串操作时计数
- D（Data）X
- 双字乘法：AX（低16位） DX（高16位）
- 双字除法：AX（商） DX（余数）

### 指针和变址寄存器 

- BP基地址指针、SP堆栈指针（指向栈顶），与SS配对使用
- SI源变址、DI目标变址

### 控制寄存器 

- 指令指针寄存器IP（Instruction Pointer），与CS配套使用，永远指向*下一条*指令，用户程序不能直接访问
- 标志寄存器FLAGS：计算机将数据作为无符号数运算，按有符号数给出标志
  - 6个状态标志位
  - 3个控制标志位

### 段寄存器 

段寄存器、指令指针寄存器：CS（code）、DS（data）、SS（stack）、ES（extra） 

CPU访问内存时由段以及指针提供地址  

**CS:IP** 

指令汇编代码储存在内存中，CPU利用CS:IP提供的地址合成指令代码入口处的内存物理地址（(CS) * 16 + IP），读入指令到指令寄存器，CPU的执行控制器执行指令寄存器中的指令，并且使CS:IP指向下一条指令代码的位置（(IP) += 所读取指令长度），循环往复 

通过转移指令jmp改变CS:IP的值： 

`jmp 段地址:偏移地址` 

仅改变IP的值（通过其他寄存器的值）： 

```
mov ax,偏移地址
jmp ax 
``` 

\* 开机时，CS:IP被初始化 

**DS（:BX）** 

DS用于存放要访问的数据的段地址，搭配mov指令可以实现对内存中数据的读取和写入（缺省使用BX为基地址） 

```
mov bx,段地址
mov ds,bx
mov al,[偏移地址]   ; 读取一个内存单元的数据到al中
mov [偏移地址],al   ; 将al中的数据写入一个内存单元
mov ax,[偏移地址]   ; 读取指定地址的字型数据到ax中
mov [偏移地址],ax   ; 将ax中的字型数据写入指定地址

mov bx,偏移地址
mov ax,[bx]         ;先将偏移地址存入寄存器中的写法
``` 

\* `mov`指令不支持直接将数据存入段寄存器 

**SS:SP** 

栈是一种对内存空间的特殊访问形式 

\* 将一段连续内存视为栈时以高位为栈底，低位为栈顶，初始时SS:SP指向栈底的后一位内存单元 

SS:SP提供栈顶的地址，相关指令如下： 

```
mov ax 栈的段地址
mov ss ax
mov sp 起始偏移地址

push 寄存器 ;将(SP)-=2，寄存器中的值（一个字）移入栈顶
push [偏移地址] ;(SP)-=2，将内存单元中的值（一个字）移入栈顶
pop 寄存器  ;将栈顶处的字型数据移入寄存器，同时(SP)+=2
pop [偏移地址]  ;将栈顶处的字型数据移入内存单元，同时(SP)+=2
``` 

\* 栈顶超界是CPU无法解决的，必须在写代码时人工确认，保证对栈的操作不会越界 

## IO

- 设备
- 接口：CPU通过接口控制设备
- 输入输出方式
  - 程序查询
  - 中断
  - DMA
  - 通道

### IO接口芯片 

五大部分

- 控制逻辑电路
- 数据缓冲寄存器
- 外设选择电路
- 命令/控制寄存器
- 设备状态标记

$
CPU
\begin{cases}
<=> 数据\\
=> 地址\\
=> 命令/控制\\
<= 状态\\
\end{cases}
接口
\begin{cases}
<=> 数据\\
=> 命令/控制\\
<= 状态\\
\end{cases}
（地址所对应的）外设
$

### 中断方式的接口 

- 中断请求触发器、中断屏蔽触发器
- 排队器（根据中断请求的优先级）
- 中断向量地址形成部件（类似于编码器的功能）

硬件实现：接口芯片包含以上三部分 

连接的外设发出中断请求 => 中断请求触发器（中断屏蔽触发器）=> 排队器 =(优先级最高的中断请求)=> 中断向量地址形成器 =(中断向量地址)=> 通过数据线传入CPU => 程序跳转到中断向量地址处，由该处指令（如JMP 666H）跳转到外设请求调用的程序处 

### DMA方式的接口 

direct memory access，外设与内存间的数据交换无需经过CPU 

## 中断系统 

引发中断的因素

- 人为设置
- 程序事故
- 硬件故障
- IO设备

### 中断的过程 

- 中断请求
- 保护现场，入栈
- 跳转到对应程序
  - 硬件实现，各中断源的信号接入中断向量地址形成部件，由硬件给出中断向量地址
  - 软件实现，进入中断后，中断识别程序查询各个中断源并跳转（查询顺序对应优先级），类似于switch
- 回复现场

# 汇编程序 

完成某种功能的指令序列成为*程序* 

## 数据的编码 

计算机以二进制存储数据，称为*机器数* 

分类

- 符号
  - 有符号数
  - 无符号数
- 小数点
  - 定点数
  - 浮点数

### 有符号定点数 

- 原码：首位为符号位
- 正数：反码、补码与原码一致
- 负数
  - 反码：除符号位外取反
  - 补码：反码加一（最后一位往前直到遇到第一个1，这个1之后的数字位取反，对除0外的数都适用）

\* 原码与反码有两种0的表示（0000 0000B、1000 0000 B），而补码只有一种0（+0、-0的补码一样，都是0000 0000 B），补码中不可能取到的1000 0000 B作为最小的负数使用 

\* 计算机中一律以补码储存，使用补码进行加（减）法运算：$[x]_补 + [y]_补 = [x + y]_补$ 

\* 移码：补码不方便比较大小，所以加上一个常数（一般为$2^{n-1}$）变成移码，用于大小比较 

### 浮点数 

组成：符号位S、阶码E、尾数M 

S、M：原码或补码 

E：补码或移码 

**规格化** 

基数为r，尾数M应满足：$\frac{1}{r} < 0.M < 1$ 

**IEEE754规格化浮点数编码**  

S、M为原码，且M前自动包含一位1 

E：移码 

- 单精度：S（1）E（8）M（23），E为+127的移码
- 双精度：S（1）E（11）M（52），E为+1023的移码

**浮点数的表达范围与精度** 

- n位浮点数与n位定点数表示的数的个数是一样多的（$2^n$），但浮点数表示的范围更大
- 浮点数靠近0处密集，远离0时稀疏
- 阶码越长，范围越大；尾数越长，精度越高
- 溢出：负上溢（负数）负下溢（0）正下溢（正数）正上溢

### 数据的逻辑运算 

## 源程序处理 

1、**编写**： 

产生一个储存了源程序的文本文件 

2、**编译、链接**： 

生成可执行文件（`.com`、`.exe`） 

可执行文件包括： 

- 程序和数据：汇编指令机器码和源程序中定义的数据
- 描述信息：程序相关信息，如占用内存空间的大小等

3、**执行**： 

将可执行文件内的机器码与数据加载入内存（由操作系统完成），初始化（比如设置CS:IP等），CPU执行程序代码 

## 源程序 

**源程序组成** 

- 伪指令
- 汇编指令

**程序返回** 

对于单任务的DOS系统，正在运行的程序（dos系统的shell）把将要执行的程序加载入内存，再使CPU执行这个程序，执行完后要返回shell 

为实现程序返回，在每个程序的末端都应加上如下汇编指令： 

```
mov ax,4c00h
int 21h
``` 

## DOS中可执行文件（.exe）的加载过程 

1、找到一段起始位置为SA:0000的容量足够大的空闲区域 

2、SA:0000到SA：00FF（256B）存放程序段前缀（PSP）用于与DOS系统通信 

3、将之后的空间视为一个新的段，(SA+10H):0000为起始位置，存入可执行文件中的程序机器码 

4、初始化相关寄存器：(DS) = SA，CS:IP指向(SA+10H):0000，CX中存放了这个程序的长度... 

## 一些伪指令语法

```
assume 寄存器:段名   ;假设段与寄存器的关联
段名 segment        ;定义段，这个段名在编译链接之后会指向一个地址

入口名:              ;表示程序入口，即初始时CS:IP指向的位置
    ...             ;段的内容

段名 ends           ;结束段，与定义段成对使用
end 入口名           ;end提示编译器结束编译
``` 

定义段用于存放代码/存放数据/作为栈使用，一个有意义的汇编程序至少有一个段 

## 一些简单汇编指令语法 

```
mov ax,bx   ;(ax) = (bx)
add ax,bx   ;(ax) += (bx)
sub ax,bx   ;(ax) -= (bx)
inc ax      ;(ax)++
``` 

### \[bx\]与loop 

**loop指令** 

```
mov cx 循环次数     ;cx与loop指令有特殊的关系，每轮循环后(cx)--，到(cx) == 0为止
循环名: 
...                 ;要循环的语句 
loop 循环名         ;固定格式
``` 

操作流程：循环名标记了循环起始地址，当程序运行到loop时，执行(cx)--
- 若(cx) != 0，CS:IP指向相应循环名标记的地址
- 若(cx) == 0，继续向下执行后续指令

