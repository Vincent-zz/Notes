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

端口模式

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
  - `generic`：类属，用于说明entity结构参数的静态信息

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

进程

```VHDL
-- 定义
-- 写在architecture中
-- process内部顺序执行，但一个archi中的多个process之间还是并行的
[label: ] process (sensitivity list) is
[variable declarations]
begin
  ...
end process;
-- variable即刻赋值
-- sensi-list中的信号在process结束后赋值
```

进程内部的顺序语句

```VHDL

```
