# 操作系统 

## 概念 

Operating System: A body of software, in fact, that is responsible for *making it easy to run programs*(even allowing you to seemingly run many at the same time), alowing programs to share memory, enabling programs to interact with devices, and other fun stuff like that. 

管理软硬件资源，为程序提供服务（管理软件的软件） 

狭义：windows、linux、android等，管理计算机、服务于二进制代码 

广义：管理硬件、服务程序，如微信也可以看作操作系统（服务各种小程序）

## 引入：电脑开机 

（x86为例） 

=> 开机时，CS = 0xFFFF，IP = 0x0000，指向ROM BIOS，执行 

    => 检查RAM、键盘显示器、磁盘 

    => 将磁盘0磁道0扇区（操作系统的引导扇区，512B，bootsect.s）读入0x7C00处 

    => 设置CS = 0x07C0，IP = 0x0000 

=> 开机程序bootsect执行 

    => 将7C00处256B的bootsect程序MOV到90000处，腾出空间，并跳转到新的位置继续顺序执行bootsect 

    => 