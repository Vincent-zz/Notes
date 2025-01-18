# 计算机

$$
\text{用户} \stackrel{\text{操作系统}}{\rightarrow} \text{交互软件} \stackrel{\text{操作系统}}{\rightarrow}  \text{驱动软件} \stackrel{\text{操作系统}}{\rightarrow}  \text{CPU} \rightarrow \text{硬件}
$$

## 操作系统 

Operating System: A body of software, in fact, that is responsible for *making it easy to run programs*(even allowing you to seemingly run many at the same time), alowing programs to share memory, enabling programs to interact with devices, and other fun stuff like that. 

管理软硬件资源，为程序提供服务（管理软件的软件） 

狭义：windows、linux、android等，管理计算机、服务于二进制代码 

广义：管理硬件、服务程序，如微信也可以看作操作系统（服务各种小程序）

## 引入：电脑开机 

（x86为例） 

=> 开机时，CS = 0xFFFF，IP = 0x0000，指向ROM BIOS，执行 

    => 检查RAM、键盘显示器、磁盘 

    => 将磁盘0磁道0磁头1扇区（操作系统的引导扇区，512B，bootsect.s）读入0x07C00处 

    => 设置CS = 0x07C0，IP = 0x0000 

=> 开机程序bootsect执行（读入系统） 

    => 将0x07C00处512B的bootsect程序MOV到0x90000处，腾出空间（），并跳转到新的位置继续顺序执行bootsect 

    => 调用BIOS读磁盘中断，将0磁道0磁头2扇区开始4个扇区（setup）读入内存，接bootsect后面（0x90200）

    => 调用BIOS显示中断，屏幕上显示开机画面

    => 读入system模块（OS代码）

    => 设置CS:IP指向0x90200

=> setup执行（接管计算机） 

    => 调用BIOS中断获取内存大小等硬件参数

    => 将0x90000处开始的所有操作系统程序移动到0x00000处