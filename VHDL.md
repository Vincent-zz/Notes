# Very Hard Design Language

## Basis

- 不区分大小写
- `--`注释
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
entity entity_name is
    [declarations]
    port
    (   
        port_name1 : signal_mode1 data_type1;
        port_name2 : signal_mode2 data_type2;
        port_name3 : signal_mode3 data_type3
    );
end;
```

信号模式

- `in`：输入，右值
- `out`：输出，左值
- `inout`：双向
- `buffer`：带反馈的输出

## Architecture

```VHDL
architecture [archi_name] of entity_name is
    [declarations]
    begin
        --code
    end;
```

## Data object

- non-static
  - `signal`：表示电路内部连接（一根导线），在顺序代码中时非及时更新（结束后更新）；实体所有端口默认为信号
  - `variable`：在**顺序代码**中表示一些局部信息，**即时更新**，**不是实际电路连接**
- static
  - `constant`：常数
  - `generic`：类属，用于说明的静态信息

```VHDL
signal name : data_type [ := initial_value];
--signal赋初值是不可综合的，仅用于仿真

variable name : data_type [ := initial_value];

constant name : data_type := value;

generic
(
    name1 : data_type1 [ := initial_value1];
    name2 : data_type2 [ := initial_value2];
);

```

## Data type

常用类型

- `bit`与`bit_vector`：来自std库的standard包，只能进行逻辑运算
- `std_logic`与`std_logic_vector`：来自ieee库的std_logic_1164包，有8种取值（其中仅`'1'`、`'0'`、`'Z'`可综合）

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

## Attribute

- 信号对象`s`的属性
  - 信号类属性
    - `s'event`：`s`变化返回true（可综合）
    - `s'stable`：`s`稳定无变化返回true（可综合）
  - 数值类属性
    - `s'length`
    - `s'range`

## Concurrent Statements

## Sequential Statements
