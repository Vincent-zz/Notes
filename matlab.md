# Matlab-Notes
是看郭彦甫老师的视频学习的Matlab，非常感谢郭老师细致的讲解，也很感谢搬运视频的up主 

[视频链接](https://www.bilibili.com/video/BV1GJ41137UH?p=1) 

**常用command**：

- 定义变量、赋值、给出计算的值（到ans）、给出变量的值（到ans） 
- `clc` clear命令行窗口 
- `clear` 清除所有变量；`clear name`清除指定变量 
- `help name` 查找关于name的信息、使用方式等（也可以在右上角search） 
- `whos` 返回变量信息（规模、类型）
- `format type`更改输出显示（type默认为short，最多保留小数点后4位）；long是最多保留小数点后15位；shortE 与 longE 是相应的科学计数法；`bank`固定显示到小数点后2位；`rat`分数显示

**脚本的执行**： 

运行全部：运行/F5 

运行部分：选中右键执行所选内容（就相当于事先写好命令行窗口的每次输入）；运行节 

**杂**： 

- MATLAB是解释性语言，不编译
- `%` 注释行的开头；`%% ` 同样是注释，但在这行上方多一条分隔线，将语句分成“节”（section）；选中右键可以方便地注释大片语句
- 换行，行尾加`...`表示和下一行是连着的
- 结尾加`;`则不在命令窗口显示这一步的结果
- 按`↑``↓`调出命令历史记录
- 选中语句右键智能缩进
**Debug**： 

每行语句前加断点，运行进入Debug模式（命令窗口中会出现`K>>`），鼠标放在变量上可以查看值 

**数据类型**： 

- 数字`i = 1`（默认是double，还有single、int8、int16、uint8、uint16、uint32等等），科学计数法比如`1.2e2`
- 逻辑logical
- 字符`ch ='a'`
- 字符串`str = 'hello world'` 第i个字符`str(i)`，拼接`s = [s1 s2]`，*字符串是以字符为元素的行矩阵，相比于一般矩阵，仅是输出时形式不同*
- 矩阵`A = [a11 a12; a21 a22; a31 a32]`（也可用`,`分隔每行间的各个数）（定义数组，即定义行/列矩阵）
- 元胞数组`A = cell(m, n)` *从1开始*
- 结构体`A = struct('membername1', value1,'membername2', value2,...)`
**关于variable**： 

- 定义`name = value`
- multiple定义`[name1, name2,..., valueN] = [value1, value2,..., valueN]`
- 特殊的定义allocate（耗时，不推荐，推荐pre-allocate）:
  - `a(i) = value`就会产生1*i的**行**矩阵，i之前补0，且可以继续定义`a(j) = value`，结尾index为max{i, j}
  - `a(i, j) = value`会产生i*j矩阵，类似的补0，且可以继续定义
- 变量完整值与类型信息查看可在workplace窗口双击打开
- 特殊的变量与常量：`ans`；`i`、`j`虚数（单独时可作变量名）；`pi`常量；`Inf`常量-无穷大；`eps`常量-无穷小；`NaN`not a number
**更多关于矩阵**： 

- 取用、赋值
  - 取用一个元素：`A(row, column)` 或 `A(n)`n是所有列从左到右从上到下拼成一列中（将矩阵拉成列）的位置，n = (column - 1) * m + row
  - 取用多个元素：`A([row1, row2, row3...], [column1, column2...])`输出对应子矩阵；`A(matrix)`输出与matrix同等规模的矩阵ans，且ans(i, j) = A(matrix(i, j))
  - `A(matrix)`*matrix为与A同等规模的logical矩阵时*，取true位置上的值，常用于左值，示例`str(str == 'x') = 'y' % 将str中的'x'全部替换为'y'`
- colon：`a:k:b`输出a开头k为公差的等差数列中不大于b的元素排成的一行（行矩阵）；例如`A = 1:100`（k省略时默认为1）；`A = [1:100;2:2:200;2:3:300]`；甚至还能`A = 'a':2:'y'`
  - colon取用整行/列元素：`A(row, :)`/`A(:, column)`
- 去掉元素：赋值为`[]`，去掉一个元素只能这样`A(n) = []`且会把原矩阵拉成列；去掉整行/列则用colon取用
- 分块矩阵：很好理解，比如`F = [A B; C D]`，`F = [A, B]` 
- 矩阵数学操作
  - 转置：`B = A'`
  - （方阵）求逆：`C = inv(A)`
  - 行列式：`det(A)`
- 运算：`+`、`-`，矩阵加减；`*`矩阵乘法，`.*`矩阵元素对应相乘；`/`乘逆矩阵，`./`元素对应相除；`^`、`.^`乘方与乘法类似。矩阵对于数的加减乘除乘方都是每个元素加减乘除乘方
- 分块：用`mat2cell`，见后
- concatenation，`cat()`见后multidimensional
- 特殊矩阵：
  - `eye(n)`n阶单位矩阵；
  - `zeros(m, n)`m*n的零矩阵；`ones(m, n)`类比zeros；
  - `diag([a11, a22, a33, ... ann])`对角矩阵；
  - `linspace(start, end, num)`行矩阵，从start到end的num个等差数 
  - 字符为元素的矩阵，输出时每行合并为一个字符串，排成一列输出
- rand（生成0~1之间的随机数）：`rand(n)`生成n*n的随机矩阵；`rand(m, n)`/`rand([m n])`；`rand(size(A))`
- 关于矩阵的函数：
  - `size(A)`返回一个1*2矩阵[m n]；
  - `max(A)`每列最大数排成一行，但当A只有一行时会输出最大的那个数，所以`max(max(A))`一定表示A中最大的元素；`min(A)`类似；`sum(A)`类似；`mean(A)`类似
- 与/非：`&`/`|`，矩阵间对应元素（非零为1，零为0）与/非，输出一个logical矩阵 

**更多关于structure** 

- 定义
  - `struct('membername1', value1,'membername2', value2,...)`（可以nest structure）
  - `structname.membername = value`，这样也定义了一个struct以及其中一个成员的值 

**更多关于cell array** 

- 定义
  - 理解：structure与cell array类比vector与matrix
  - `A = {a11, a12, ...; a21, ...}`（类比matrix）
  - `A(row, column) = {value}`或`A{row, column} = value`，这样也定义了一个元胞数组以及其中一个位置元素的值（A(row,column)是个pointer） 
- 读取cell值 `A{row, column}`；若此cell是矩阵，也可以`A{row, column}(row', column')`（这有点反matlab）
- 关于cell的函数：
  - 与matrix互相转化：`cellarray = num2cell(matrix)`matrix的每一个num元素成一个cell（不知道意义何在）；`cellarray = mat2cell(matrix, [combinedRowsNum1, combinedRowsNum2, ...], [combinedcolumnsNum1, combinedcolumnsNum2, ...])`
  
**multidimensional（cell）array** 

- 定义
  - `A{row, column, layer} = value`
  - matrix/cell concatenation`cat(dim, A1, A2, A3, ...)`
  
**逻辑判断**： 

`~`非，`~=`不等于，其他同C++ 

可以批量比较，例如`1 == [1 2 4; 3 1 1]`、`'l' == 'hello world'`将会输出一个logical矩阵 

矩阵比较，同等规模，每个元素逐一比较，输出logical矩阵 

**语句**： 

```
%% if语句
if condition1
    statement1
elseif condition2
    statement2
else condition3
    statement3
end

%% switch语句
switch name
case value1
    statement1
case value2
    statement2
otherwise
    statement
end

%% while语句
while condition
    statement
end

%% for语句
for i = a:k:b % for i=某个行向量/行矩阵
    statement(i)
end
% P.S.i的最终值为行矩阵最后一个元素的值

``` 
循环同样有`break`，`continue`的操作 

**一些常用function**: 

（可以在上方 编辑器-插入-函数（fx） 处查询函数） 

- `rem()`取余
- `disp(n)`在命令窗口中display变量n的值（有没有;结尾无所谓），也可以直接输入值，如`disp('Hello World')`（P.S.字符串输出时将没有单引号）
- 类型转换
  - 数字之间的转换`typename(num)`例如，`int8()`、`single()`
  - `num2str(number)`数字转字符,`str2num(string)`字符转数字
  - 字符转ASCII`abs(ch)`，也可以用数字间转换的函数，如`uint64(ch)`；ASCII转字符`char(asciinum)`
- 字符串长度`length(str)`
- `isempty()`是否为空
- 计时
```
 tic
 statement
 toc
```

**自定义function**： 

例： 

```
%%% functionname.m文件
function output = functionname(input1, input2)
% function注释
...
output = ...
end
%%% 脚本中调用
ans = functionname(value1, value2)
``` 

每个function都存在一个单独的.m文件里，且文件名 = 函数名 

function内部代码最好用`.*`、`./`之类的，便于input使用矩阵批量输入（同时output自然也是矩阵） 

\* 以上为单输出，只有单输出才能直接运算 

\* 也可以没输出，`function functionname(input)` 

\* 多值输出的实现： 

- output赋为矩阵，将要输出的多个值作为矩阵的元素，通过单输出矩阵来实现多输出（但还要后期取用，理论上可行但不方便）
- 直接定义为多输出函数，这适用于输出个数确定的情况，例： 

```
%%% functionname.m文件
function [output1, output2, output3]  = functionname(input1, input2)
% function注释
...
output1 = ...
output2 = ...
output3 = ... 
% 也可以multiple赋值：[output1, output2, output3] = [value1, value2, value3]
end
%%% 脚本中调用
[ans1, ans2, ans3] = functionname(value1, value2)
``` 
\* 函数中的为局部变量，但也可以声明全局变量：`global a b c % 在脚本里`声明全局变量a、b、c；`global a b c % 在函数定义里`引入全局变量a、b、c 

\* 多输出函数必须用multiple赋值来接，*并不是输出一个矩阵*，不能直接参与运算 

\* 不管是单输出还是多输出，输出的值（只要合理）可以是任何数据类型的值（当然也包括矩阵） 

**函数句柄（function handle）**： 

**文件导入导出** 

（1）`save filename.mat`以.mat文件将workspace的内容存在current folder下，用文字工具打开不可阅读内容；`save filename.mat -ascii`用文字工具打开可阅读内容，但变量名会丢失，多变量时会混乱且无法load 

`load('filename.mat')`，`load('filename.mat', '-ascii')`与上面两个save对应 

（2）从excel导入，例`A = xlsread('filename.xlsx', 'A1:Z4') % 第二项区域可省略`将current folder下filename.xlsx文件的数据导入矩阵A中（且*只导入数字类型*） 

导出到excel，例`xlswrite('filename.xlsx', A, 1, 'A1')`，既可以是已有的excel文件也可以新建一个excel文件，A是一个数字矩阵，也可以是一个字符串的cell例如`xlswrite('filename.xlsx', {'text'}, 1, 'A1')`，1代表导出到sheet1，最后一项为导入起始位置。xlswrite写入将覆盖原值。