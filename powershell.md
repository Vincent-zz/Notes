# PowerShell

## Basis

- 不区分大小写
- $\uparrow$上一条命令
- `Tab`自动补全
- 通配符`*`
- 右键单击复制粘贴
- DOS、UNIX、windows原生命令皆可执行（别名）
- 大量的别名，查看别名：`get-alias <string>`


## Help System

Commands

```
update-help
get-help *service*
get-help *service* -detailed
get-help g*service -examples
get-help get-service -full
get-help get-service -showwindow
```

Example of Syntax
```
command -parameter_name <argument_type>
command [-parameter_name <argument_type[]>]
command [[-parameter_name] <argument_type[]>]
```

- `[...]`代表该参数可选optional
- `argument_type[]`代表可输入多个值，使用`,`隔开
- `[-parameter_name]`代表参数名可省略