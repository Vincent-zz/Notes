# Very Hard Design Language

## Basis

- 不区分大小写
- `--`注释
- 层级关系：库>包>实体>结构>顺序代码块
- 单值信号`''`；多值信号`""`

## Framework

例子：三人表决器

```VHDL
library ieee;
use ieee.std_logic_1164.all;
---------------------------------
entity VOTE is
    port
    (   A1, A2, A3  :   in  bit;
        F           :   out bit );
end;
---------------------------------
architecture VOTE_arch of VOTE is
    signal F1,F2,F3 : bit;
begin
    F1 <= A1 and A2;
    F2 <= A2 and A3;
    F3 <= A1 and A3;
    F  <= F1 or F2 or F3;
end;
```

## Library

结构：library.package.project

```VHDL
--常用library及其package

-------ieee--------
library ieee;
use ieee.std_logic_1164.all;

--------std---------
--VHDL标准资源库，实际程序中无需声明（默认打开）
library std;
use std.standard.all;

-------work---------
--当前工作库，存放当前设计的所有代码，无需声明
library work;
use work.all;
```

## Entity

```VHDL
entity <entity_name> is
    [declarations]
    port
    (   
        port_name1 : signal_mode1 data_type1;
        port_name2 : signal_mode2 data_type2;
        port_name3 : signal_mode3 data_type3
    );
end;
```

端口模式

- `in`：输入，右值
- `out`：输出，左值
- `inout`：双向
- `buffer`：带反馈的输出

## Architecture

```VHDL
architecture <archi_name> of entity_name is
  [declarations]
  begin
      --code
  end;
```

## Data object

- non-static
  - `signal`：表示电路内部连接（一根导线）；实体所有端口默认为信号
    - 定义在实体、实体端口、结构首部，向下全局
  - `variable`：在**顺序代码块**中表示一些局部信息，**即时更新**，**不是实际电路连接**
    - 仅在process、function、proccedure中定义
- static
  - `constant`：常数
    - 包、实体、结构中皆可定义，向下全局
  - `generic`：类属，用于说明entity结构参数的静态信息
    - 定义在实体中，可在其结构中使用

```VHDL
signal name : data_type [ := default_value];
--signal赋初值是不可综合的，仅用于仿真

variable name : data_type [ := default_value];

constant name : data_type := value;

generic
(
    name1 : data_type1 [ := default_value1];
    name2 : data_type2 [ := default_value2];
);

```

## Attribute

- 信号对象`s`的属性
  - 信号类属性
    - `s'event`：`s`变化返回true（可综合）
    - `s'stable`：`s`稳定无变化返回true（可综合）
  - 数值类属性
    - `s'length`
    - `s'range`

## Data type

常用类型

- `bit`与`bit_vector`：来自std库的standard包，只能进行逻辑运算
- `std_logic`与`std_logic_vector`：来自ieee库的std_logic_1164包，有8种取值（其中仅`'1'`、`'0'`、`'Z'`可综合）
  - 用`falling_edge(s)`、`rising_edge(s)`来特指下降沿、上升沿

```VHDL
entity data_type_example is
    port
    (   
        port1 : in bit := '1';
        port2 : in bit_vector(0 to 7);
        port3 : in bit_vector(7 downto 0);
        port4 : out std_logic;
        port5 : out std_logic(0 to 3) := "0001";
        port6 : out std_logic(3 downto 0)
    );
end;
```

## Operator

- 赋值
  - `<=`：信号赋值
  - `:=`：其他数据对象赋值（以及赋初值）
- 逻辑运算
  - `not`优先级最高
  - `and`、`or`、`nand`、`nor`、`xor`并列
- 算术运算
  - `+`、`-`、`*`、`/`、`**`、`abs`、`mod`（右值返回）、`rem`（左值返回）
- 移位
  - `sll`、`srl`、`sla`、`sra`、`rol`、`ror`
- 拼接
  - `x <= '1'; y <= x & "1010";`
  - `y <= ('1', '1', '0', '1', '0');`

## Define your own package

元件

```VHDL
--component.vhd
library ieee;
use ieee.std_logic_1164.all;
--------------------------------
entity comp_name is 
  generic(...);-- 有generic时无法直接实例化（综合）这个元件
  port(...);
end component;
--------------------------------
architecture of comp_name is
  begin
    ...
  end;
```

包

```VHDL
--my_package.vhd
library ieee;
use ieee.std_logic_1164.all;
--------------------------------
package package_name is

  component comp_name is 
    generic(...);
    port(...);
  end component;

end package_name;
--------------------------------
package body package_name is

  -- procedures and functions

end package_name;
```

包中元件的使用

```VHDL
--project.vhd
library ieee;
use ieee.std_logic_1164.all;

library work;
use work.my_package.all;
-------------------------------
entity my_project is
  generic(...);
  port(...);
end;

architecture of my_project is
  begin
    label: comp_name generic map(...) port map(...);
  end;
```

- 端口映射
  - 位置映射`port map(x, y)`
  - 名称映射`port map(x => a, y => b)`
- 类属映射
  - `generic map(para.list)`

## Concurrent Statements

```VHDL
-- 1. when-else
F <= A when flag1 else
     B when flag2 else
     C;
-- 2. with-select
with flag select
  F <= A when valueA, 
       B when valueB,
       C when others;
-- 3. use package component
-- 4. after（无法综合，仅用于仿真）
F <= F0, F1 after 10ns, F2 after 20ns;-- 时延都是相对仿真开始时间而言的
CLK <= not CLK after 10ns;
-- 5. transport after
```

## Sequential Statements

- 顺序语句常用于描述时序逻辑电路，非必要不写顺序语句
- HDL的目的是描述电路，并无所谓“执行”的说法，与编程语言有本质不同。
- 顺序语句综合较为复杂，必须清楚所需电路结构否则容易出错

进程

- process内部顺序执行，但一个archi中的多个process之间还是并行的
- variable非实际电路，即刻赋值，仅仅起辅助作用（对变量赋的初值只会在开始执行一次）
- signals in process
  - 敏感表中为该process电路的（非锁存）输入信号
  - 未出现在敏感表中但出现在process中的输入信号，仅在敏感表中信号变化时采样，对应实际电路中的锁存器
  - 输出信号在process最后统一赋值
  - 常见错误
    - 某信号作为输出同时也作输入，在process中的计算都只取用输入值，它仅在process最后才会得到新的赋值（电路会因此陷入unstable loop，即进程反复执行）
    - 一个输出信号有多个赋值语句，以最后的为准（这种信号多次赋值的写法没有意义）
- 绝大多数综合器不支持单个process中存在多个时钟，也不支持时钟触发的if语句带有else

```VHDL
-- 定义
-- 写在architecture中
[label: ] process (<sensitivity table>) is
[variable declarations]
begin
  ...
end process;
```

进程内部的顺序语句

```VHDL
-- 1. wait
-- *碰到wait时会对之前的信号赋值，而不是最后统一赋值
wait on <signal table>; -- 信号发生变化才继续
wait until <condition>; -- 某条件发生才继续（*condition必须要发生变化，即使一开始就满足条件仍会阻塞*）
wait for <time>; -- 延时，只能仿真不可综合
wait on <signal table> until <condition> for <time>; -- 当信号改变之后，某条件发生则继续；time为最长挂起时间
-- 2. if
-- 优先级电路
if <condition> then
  [statement]
elsif <condition> then
  [statement]
else <condition> then
  [statement]
end if;
-- 3. case-when
case <expression> is
  when <value> => [statement]
  when <value> => [statement]
  when others => [statement]
end case;
-- 4. loop
loop
  [statement]
end loop;
-- while-loop
while <condition> loop
  [statement]
end loop;
-- for-loop
for <variable> in <range> loop
  [statement]
end loop;
-- exit/next
exit [when <condition>];
next [when <condition>];
```

## FPGA

Field Programmable Gate Array -- a reconfigurable semiconductor integrated circuit

内部组成

- Configurable Logic Blocks (the fundamental piece)
  - Look Up Table：储存器，储存“真值表”，修改“真值表”即等效于设置组合逻辑
  - Flip-Flop：实现时序逻辑
- Interconnects：导线
- IO Blocks
- ...

## By-Talk about EDA

- 时序逻辑 = 组合逻辑 + D触发器 + CLK
- “数电的尽头是模电”
