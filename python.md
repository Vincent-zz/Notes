# Python

## 基础语法知识

- [菜鸟教程](https://www.runoob.com/python/python-tutorial.html)
- python中的所有变量都是类引用，数据类型是类的属性之一
- 数字、字符串、元组为不可变类型，具有相同不可变类型值的变量会指向同一内存位置

```python
>>> a = 1
>>> b = 1
>>> print(a is b)
True
>>> print(a.index == b.index)
True
>>> print(id(a) == id(b))
True
``` 

- 关于字符串变量`__name__`：运行时某个module时该module中`__name__ = '__main__'`，而在其`import`的其他module中`__name__ = '__对应的modele名__'`

## 面向对象

语法：

```python
class classname：
    'document' #对应内置属性__doc__的值
    static_attr1 = 1
    static_attr2 = '2' #类所有实例共用的属性


    def __init__(self, attr1_value, attr2_value): #构造函数
        self.attr1 = attr1_value
        self.attr2 = attr2_value


    def __del__(self): #析构函数
        print('销毁')


    def function1(self, para1, para2): #实际调用时无需输入第一个参数self
        print('this is function1 with' + str(para1) + str(para2))

    @staticmethod #静态函数
    def staticfunction(self)
        print('this is a static function')

inst = classname(1, 2) #类实例化，调用的是__init__函数
``` 

- 双下划线开头的属性、方法为私有
- 特殊函数：`__init__()`、`__del__()`、`__str__()`（返回字符串作为`print()`结果）、`__repr__()`、`__len__()`（返回数字作为`len()`结果）等
- 类内置属性：`__doc__`、`__dict__`、`__module__`等，返回类的相关信息
- 特殊的属性`__slot__`：`__slot__ = [tuple]`，限定类的属性（不过对子类不起作用）
- 属性操作：`hasattr()`、`setattr()`、`delattr()`等（只对对象生效）
- 抽象类，需要`abc`模块的抽象基类
```python
    @abstructmethod #抽象类函数
    def abstructfunction(self)
        print('this is a abstruct function')
```

继承： 

```python
class classname(parentclass):
    ...
```  

- `super().function()`调用父类函数
- 优先查找子类方法，找不到再在父类中查找

## 图形界面

默认GUI开发模块：`tkinter` 

```python
import tkinter
def command():
    print('hello')
top = tkinter.Tk() #创建窗口
top.geometry('1024x512') #设置窗口大小
top.title('gui') #设置窗口标题
panel = tkinter.Frame(top)  #创建panel
button = tkinter.Button(panel, text='button', command=command) #创建按钮
button.pack(side='left') #设置按钮位置
panel.pack(side='bottom') #设置panel位置
tkinter.mainloop() #开启事件循环
``` 

`tkinter`功能并不强大，gui开发有许多其他module可供使用

## 正则表达式

使用`re`模块 

`flag`

- re.l：忽略大小写
- re.M：多行模式

函数

- `pattern = re.compile(string, flag)`，生成正则表达式对象（`RegexObject`）
- `re.match(pattern, string, flag=0)`，匹配字符与正则表达式
- `re.search(pattern, string, flag=0)`，查找并返回字符串中与正则表达式的第一处成功匹配