# AI Python 编码规范
[TOC]


## 1 前言
Python是重要的脚本语言。本文档的目标是使Python代码风格保持一致，提高代码可读性，便于代码的维护和管理。

## 2 风格规范

### 2.1 文件

#### 2.1.1 Shebang
__[建议] 根据 <a href="http://www.python.org/dev/peps/pep-0394/" target="_blank">PEP-394</a>，程序的`main`文件应该以` #!/usr/bin/python2`或者 `#!/usr/bin/python3`开始。其他大部分`.py`文件不必以`#!`作为文件的开始。__

解释:

 [Shebang](http://en.wikipedia.org/wiki/Shebang_(Unix)) 是一个由井号和叹号构成的字符串行(`#!`)，其出现在文本文件的第一行的前两个字符。在文件中存在Shebang的情况下，类Unix操作系统的程序载入器会分析Shebang后的内容，将这些内容作为解释器指令，并调用该指令，并将载有Shebang的文件路径作为该解释器的参数。例如, `#!`用于帮助内核找到Python解释器，但是在导入模块时，将会被忽略。因此只有被直接执行的文件中才有必要加入`#!`。

#### 2.1.2 文件和sockets管理
__[建议] 在文件和sockets结束时, 显式的关闭它。__

解释：

除了文件外，sockets或其他类似文件的对象在没有必要的情况下打开，会有许多副作用，特别是安全问题。例如：消耗系统资源；未关闭文件将会阻止该文件的诸如移动、删除等操作。

__[建议] 推荐使用["with"语句](http://docs.python.org/reference/compound_stmts.html#the-with-statement) 以管理文件。__

示例：

```python
with open("hello.txt") as hello_file:
     for line in hello_file:
         print (line)
```



### 2.2 结构

#### 2.2.1 分号

行尾不要加分号，也不要用分号将两条命令放在同一行。

#### 2.2.2 行长度

每行不超过80个字符。如果需要，可利用Python会将 [圆括号, 中括号和花括号中的行隐式的连接起来](http://docs.python.org/2/reference/lexical_analysis.html#implicit-line-joining)这一特点，在表达式外围增加一对额外的圆括号实现隐式行连接。

示例：

```python
x = ('This will build a very long long '
         'long long long long long long string')
```

对于长的导入模块语句以及注释里的URL可放在一行上。

示例：

```python
#See details at 
#http://www.example.com/us/developer/documentation/api/content/v2.0/csv_file_name_extension_full_specification.html
```

#### 2.2.3 括号

括号可用于行连接或者元组两边，但是在返回语句或条件语句中不要使用括号。

示例：

```python
Yes:  if foo:
         bar()
      while x:
         x = bar()
      if not x:
         bar()
      return foo
      for (x, y) in dict.items(): ... 

No:   if (x):
         bar()
      if not(x):
         bar()
      return (foo)
```

#### 2.2.4 缩进

用4个空格来缩进代码。对于行连接的情况，垂直对齐换行的元素，或者使用4空格的悬挂式缩进(这时第一行不应该有参数)。

示例：

```python
# Aligned with opening delimiter
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Aligned with opening delimiter in a dictionary
foo = {
    long_dictionary_key: value1 +
                         value2,
    ...
    }

# 4-space hanging indent; nothing on first line
foo = long_function_name(
    var_one, var_two, var_three,
    var_four)

# 4-space hanging indent in a dictionary
foo = {
    long_dictionary_key:
        long_dictionary_value,
    ...
}
```

#### 2.2.5 空行

空一行：用于类成员函数之间，或者用于区分不同逻辑块；

空两行：类与类，类与函数，函数与函数之间。

示例：

```python
class Test(object):
    
    def __init__(self): 
        pass

    def function1(self):
        pass

    def function2(self):
        pass


def function3():
    pass
```

#### 2.2.6 空格

加空格：二元操作符两侧都加上一个空格，比如赋值(=)，比较(==, <, >, !=, <>, <=, >=, in, not in, is, is not)，布尔(and, or, not)。算术操作符两侧空格依据喜好添加。

示例：

```python
Yes: x == 1
No:  x<1
```

不加空格：

括号内不加空格。

示例：

```python
Yes: spam(ham[1], {eggs: 2}, [])
No:  spam( ham[ 1 ], { eggs: 2 }, [ ] )
```

逗号、分号、 冒号前面不加空格, 但在其后面加(除了在行尾)。

示例：

```python
Yes: if x == 4:
         print x, y
     x, y = y, x
No:  if x == 4 :
         print x , y
     x , y = y , x
```

参数列表、索引或切片的左括号前不应加空格。

示例：

```
Yes: spam(1)
No:  spam (1)
```

当'='用于指示关键字参数或默认参数值时，不要在其两侧使用空格。

示例：

```python
Yes: def complex(real, imag=0.0): return magic(r=real, i=imag)
No:  def complex(real, imag = 0.0): return magic(r = real, i = imag)
```

不要用空格来垂直对齐多行间的标记，减少维护的负担。

示例：

```
Yes:  foo = 1000  # comment
      long_name = 2  # comment that should not be aligned
   
      dictionary = {
          "foo": 1,
          "long_name": 2,
          }
No:   foo       = 1000  # comment
      long_name = 2     # comment that should not be aligned
   
      dictionary = {
          "foo"      : 1,
          "long_name": 2,
          } 
```



### 2.3 注释

#### 2.3.1 注释方式

Python使用文档字符串进行注释，文档字符串的惯例是使用三重双引号''' '''。文档字符串第一行为以句号, 问号或惊叹号结尾的概述。第二行为一个空行. 第三行开始为文档字符串剩下的部分, 其与文档字符串的第一行的第一个引号对齐。

#### 2.3.2 模块注释

每个文件应该包含一个许可样板。根据项目使用的许可(例如, Apache 2.0, BSD, LGPL, GPL)，选择合适的样板。

示例：

```python
# -*- coding: utf-8 -*-
# (C) JiaaoCap, Inc. 2017-2018
# All rights reserved
# Licensed under Simplified BSD License (see LICENSE)
```

#### 2.3.3 函数/方法注释

下文所述函数包括函数、方法以及生成器。对于每一个函数必须要有文档字符串，除非函数外部不可见或者非常短小或者简单明了，不需注释。

对于函数的注释，文档字符串要包含以下方面：函数的用途、函数的输入、输出等。通常分为几个小节来进行详细描述。每节应该以一个标题行开始，标题行以冒号结尾。除标题行外，小节内的其他内容应被缩进2个空格。

示例：

```python
def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.  Silly things may happen if
    other_silly_variable is not None.

    Args:
        big_table: An open Bigtable Table instance.
        keys: A sequence of strings representing the key of each table row
            to fetch.
        other_silly_variable: Another optional variable, that has a much
            longer name than the other args, and which does nothing.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {'Serak': ('Rigel VII', 'Preparer'),
         'Zim': ('Irk', 'Invader'),
         'Lrrr': ('Omicron Persei 8', 'Emperor')}

        If a key from the keys argument is missing from the dictionary,
        then that row was not found in the table.

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
    pass
```

#### 2.3.4 类注释

对于每一个类，在其定义下面应该有一个用于描述该类的文档字符串。若类存在公共属性，文档中应该有一个属性(Attributes)段，并且应该遵守和函数参数相同的格式。

示例：

```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

#### 2.3.5 块注释和行注释

代码中技巧性的部分应该重点添加注释。对于复杂的操作, 应该在其操作开始前写上若干行注释. 对于不是一目了然的代码，应在其行尾添加注释。为了提高可读性，注释应该至少离开代码2个空格。

示例：

```python
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.

if i & (i-1) == 0:        # True if i is 0 or a power of 2.
```

#### 2.3.6 TODO注释

临时代码使用TODO注释。TODO注释应该在所有开头处包含"TODO"字符串，然后是用括号括起来的名字, email地址或其它标识符。随后是一个可选的冒号。 接着必须有一行注释，解释要做什么。

示例：

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.
# TODO(Zeke) Change this to use relations.
```

### 2.4 导入格式

每个导入应该独占一行。

示例：

```python
Yes: import os
     import sys
No:  import os, sys
```

导入应该按照从最通用到最不通用的顺序分组，首先为标准库导入，接下来为第三方库导入，最后为应用程序指定导入。

示例：

```python
import foo
from foo import bar
from foo.bar import baz
from foo.bar import Quux
from Foob import ar
```

### 2.5 命名

**Python之父Guido推荐的规范：**

| Type | Public | Internal |
| :---- | :------ | :-------- |
| Modules | lower_with_under | _lower_with_under |
| Packages | lower_with_under |  |
| Classes | CapWords | _CapWords |
| Exceptions | CapWords |  |
| Functions | lower_with_under | _lower_with_under() |
| Global/Class Constants | CAPS_WITH_UNDER | _CAPS_WITH_UNDER |
| Global/Class Variables | lower_with_under | _lower_with_under |
| Instance Variables | lower_with_under | _lower_with_under (protected) or __lower_with_under (private) |
| Method Names | lower_with_under() | _lower_with_under() (protected) or __lower_with_under() (private) |
| Function/Method Parameters | lower_with_under |  |
| Local Variables | lower_with_under |  |

### 2.6 其他

#### 2.6.1 语句

通常每个语句独占一行。如果测试结果与测试语句在一行放得下, 也可以将它们放在同一行。但是如果是if 语句，只有在没有else时，可以放在一行。特别地，不能将`try/except`放在同一行。

示例：

```python
Yes:

  if foo: bar(foo)
    
No:
  if foo: bar(foo)
  else:   baz(foo)

  try:               bar(foo)
  except ValueError: baz(foo)

  try:
      bar(foo)
  except ValueError: baz(foo)  
```

#### 2.6.2 字符串

字符串规范化主要包括格式化和引号的引用。

字符串一般使用%或者format来格式化字符串。

示例：

```python
x = a + b
x = '%s, %s!' % (imperative, expletive)
x = '{}, {}!'.format(imperative, expletive)
x = 'name: %s; score: %d' % (name, n)
x = 'name: {}; score: {}'.format(name, n) 
```

避免在循环中用+和+=操作符来累加字符串。

示例：

```python
Yes: items = ['<table>']
     for last_name, first_name in employee_list:
         items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
     items.append('</table>')
     employee_table = ''.join(items)
        
No: employee_table = '<table>'
    for last_name, first_name in employee_list:
        employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
    employee_table += '</table>'
```

在同一个文件中，保持使用字符串引号的一致性。使用单引号'或者双引号"之一用以引用字符串，并在同一文件中沿用。在字符串内可以使用另外一种引号，以避免在字符串中使用。

```python
Yes:
     Python('Why are you hiding your eyes?')
     Gollum("I'm scared of lint errors.")
     Narrator('"Good!" thought a happy Python reviewer.')
        
No:
     Python("Why are you hiding your eyes?")
     Gollum('The lint. It burns. It burns us.')
     Gollum("Always the great lint. Watching. Watching.")
```

#### 2.6.3 Main

主功能应该放在一个main()函数中。这样当其他模块被导入时主程序就不会被执行。

示例：

```python
def main():
      ...

if __name__ == '__main__':
    main()
```

## 3.语言规范

### 3.1 全局变量

避免使用全局变量，用类变量来代替。如下情况例外：

1. 脚本的默认选项。
2. 模块级常量. 例如:　PI = 3.14159. 常量应该全大写, 用下划线连接。
3. 用全局变量作为缓存值或者作为函数返回值。
4. 如果需要, 全局变量应该仅在模块内部可用, 并通过模块级的公共函数来访问。

### 3.2 默认参数

在函数参数列表的最后指定变量的值，例如，`def foo(a,b=0):`。如果调用foo时只带一个参数，则b被设为0。如果带两个参数，则b的值等于第二个参数。需要注意的是不要在函数或方法定义中使用可变对象作为默认值。

示例：

```python
Yes: def foo(a, b=None):
         if b is None:
             b = []
            
No:  def foo(a, b=[]):
         ...
No:  def foo(a, b=time.time()):  # The time the module was loaded???
         ...
No:  def foo(a, b=FLAGS.my_thing):  # sys.argv has not yet been parsed...
         ...
```

### 3.3 列表推导

列表推导(list comprehensions)与生成器表达式(generator expression)提供了一种简洁高效的方式来创建列表和迭代器。其结构一般为映射表达式，for语句，过滤器表达式。每个部分应该单独置于一行。禁止多重for语句或过滤器表达式。复杂情况下还是使用循环。

示例：

```python
Yes:
  result = []
  for x in range(10):
      for y in range(5):
          if x * y > 10:
              result.append((x, y))

  for x in xrange(5):
      for y in xrange(5):
          if x != y:
              for z in xrange(5):
                  if y != z:
                      yield (x, y, z)

  return ((x, complicated_transform(x))
          for x in long_generator_function(parameter)
          if x is not None)

  squares = [x * x for x in range(10)]

  eat(jelly_bean for jelly_bean in jelly_beans
      if jelly_bean.color == 'black')

No:
  result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

  return ((x, y, z)
          for x in xrange(5)
          for y in xrange(5)
          if x != y
          for z in xrange(5)
          if y != z)
```

### 3.4 条件表达式

条件表达式是对于if语句的一种更为简短的句法规则。适用于单行函数. 在其他情况下，推荐使用完整的if语句。

示例：

```python
x = 1 if cond else 2
```
### 3.5 True/False的求值

Python在布尔上下文中会将某些值求值为false. 例如0， None, [], {}, "" 都被认为是false。尽可能使用隐式的false, 例如: 使用 `if foo:` 而不是 `if foo != []:` 。此外还有如下注意事项：

1. 注意'0'(字符串)会被当做true。

2. 永远不要用==或者!=来比较单件, 比如None. 使用is或者is not。

3.  注意: 当写下 `if x:` 时, 其含义是 `if x is not None` 。 例如: 当测试一个默认值是None的变量或参数是否被设为其它值。这个值在布尔语义下可能是false!

4. 永远不要用==将一个布尔量与false相比较。使用 `if not x:` 代替。如果需要区分false和None, 应该用像 `if not x and x is not None:` 这样的语句。

5. 对于序列(字符串, 列表, 元组)，要注意空序列是false。因此 `if not seq:` 或者 `if seq:` 比 `if len(seq):` 或 `if not len(seq):` 要更好。

6. 处理整数时，使用隐式false可能会得不偿失(即不小心将None当做0来处理)。可以将一个已知是整型(且不是len()的返回结果)的值与0比较。

   示例：

   ```python
   Yes: if not users:
            print 'no users'
   
        if foo == 0:
            self.handle_zero()
   
        if i % 10 == 0:
           
   No:  if len(users) == 0:
            print 'no users'
   
        if foo is not None and not foo:
            self.handle_zero()
   
        if not i % 10:
            self.handle_multiple_of_ten()
   ```



### 3.6 默认迭代器和操作符

容器类型，像字典和列表，定义了默认的迭代器和关系测试操作符(in和not in)。如果类型支持，就使用默认迭代器和操作符，例如列表，字典和文件。内建类型也定义了迭代器方法。优先考虑这些方法，而不是那些返回列表的方法。

示例：

```python
Yes:  for key in adict: ...
      if key not in adict: ...
      if obj in alist: ...
      for line in afile: ...
      for k, v in dict.iteritems(): ...
        
No:   for key in adict.keys(): ...
      if not adict.has_key(key): ...
      for line in afile.readlines(): ...
```

### 3.7 生成器

所谓生成器函数, 就是每当它执行一次生成(yield)语句，它就返回一个迭代器，这个迭代器生成一个值。生成值后，生成器函数的运行状态将被挂起，直到下一次生成。

示例：

```python
def odd():
    print('step1')
    yield 1
    print('step2')
    yield 3
    print('step3')
    yield 5
    
```

generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。上述示例定义一个generator，依次返回数字1，3，5。

### 3.8 类或函数

#### 3.8.1 嵌套/局部/内部类或函数

类可以定义在方法，函数或者类中。函数可以定义在方法或函数中。封闭区间中定义的变量对嵌套函数是只读的。因此推荐使用嵌套/本地/内部类或函数。

#### 3.8.2 Lambda函数

适用于单行函数。如果代码超过60-80个字符，最好还是定义成常规(嵌套)函数。

示例：

```python
p = lambda x,y:x+y
```

#### 3.8.3 函数与方法装饰器

[用于函数及方法的装饰器](https://docs.python.org/release/2.4.3/whatsnew/node6.html) (也就是@标记)。最常见的装饰器是@classmethod 和@staticmethod，用于将常规函数转换成类方法或静态方法。装饰器语法也允许用户自定义装饰器。对于函数 `my_decorator` , 下面的两段代码是等效的:

示例：

```python
class C(object):
   @my_decorator
   def method(self):
       # method body ...
```

```python
class C(object):
    def method(self):
        # method body ...
    method = my_decorator(method)
```

### 3.9 线程

优先使用Queue模块的 `Queue` 数据类型作为线程间的数据通信方式。另外，使用threading模块及其锁原语(locking primitives)。

### 3.10 异常

异常是一种跳出代码块的正常控制流来处理错误或者其它异常条件的方式。

异常必须遵守特定条件：

1. 像这样触发异常: `raise MyException("Error message")` 或者 `raise MyException` 。不要使用两个参数的形式( `raise MyException, "Error message"` )或者过时的字符串异常( `raise "Error message"` )。

2. 模块或包应该定义自己的特定域的异常基类，这个基类应该从内建的Exception类继承。模块的异常基类应该叫做"Error"。

   示例：

   ```python
   class Error(Exception):
       pass
   ```

3. 永远不要使用 `except:` 语句来捕获所有异常，也不要捕获 `Exception` 或者 `StandardError` ，除非你打算重新触发该异常，或者你已经在当前线程的最外层(记得还是要打印一条错误消息)。在异常这方面，Python非常宽容，`except:`真的会捕获包括Python语法错误在内的任何错误。使用 `except:` 很容易隐藏真正的bug。

4. 尽量减少try/except块中的代码量。try块的体积越大，期望之外的异常就越容易被触发。这种情况下，try/except块将隐藏真正的错误。

5. 使用finally子句来执行那些无论try块中有没有异常都应该被执行的代码。这对于清理资源常常很有用，例如关闭文件。

6. 当捕获异常时, 使用 `as` 而不要用逗号。

   示例：

   ```python
   try:
       raise Error
   except Error as error:
       pass
   ```

   
