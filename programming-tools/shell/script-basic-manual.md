# Shell脚本常用语法手册 

Linux Shell 是与 Linux 系统交互的一个应用程序，我们通过这个程序可以操作 Linux 系统的内核服务。

执行 `$cat /etc/shells`, 可以看到系统中现在可用的 Shell 解释器

```bash
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
/usr/local/bin/zsh
```

现代的 Linux 系统中 `/bin/sh` 已经被 `/bin/bash`, 作为 Linux 默认的 Shell.

输入 `$echo $SHELL` 可以看到当前系统的 Shell.

## Shell 脚本

Shell 脚本 (Shell Script), 是为 Shell 编写的一个脚本程序。我们说的 Shell 通常指的是 Shell 脚本。

## Shell 变量

Shell 脚本属于弱类型的脚本语言，在使用的时候不需要提前定义变量类型。

> 可能的坑：

> 1. 赋值变量不能有美元符号 (`$`)
> 2. 赋值语句等号 (`=`) 左右都不能有空格


### 变量定义

```bash
#!/bin/bash

# 直接赋值
name="cizel"

# 语句赋值
for file in `ls /etc`
```

### 变量使用

```bash
#!/bin/bash

# 定义变量 name
name="cizel"

# 使用美元 ($) 符号
echo $name

# 使用美元 ($) 符号和括号结合，常用于字符串拼接
echo ${name}
```

or

```bash
#!/bin/bash

# 高级用法

# 默认值：如果变量没有声明，使用默认值 ${var=DEFAULT}
echo ${name="ok"}
# output: ok

# 默认值：如果变量没有声明，或者为空字符串，使用默认值 ${var:=DEFAULT}
name=""
echo ${name:="ok"}
# output: ok
```

## Shell 数字运算

Shell 中的数字运算可以采用 `$((num1 + num2))` 的方式，例如：

> 可能的坑：

> 1. Shell 中的变量默认是字符串，使用 `result=1+2;echo $result` , 输出会是 `1+2`
> 2. 数值运算的两个变量必须是 `数字` 或者 `数字字符串`, 不然会报错

```bash
#!/bin/bash

a=2
b="3"

echo (($a + $b))
# output: 5

echo (($a - $b))
# output: -1

echo (($a * $b))
# output: 6

echo (($a / $b))
# output: 0

# 取模 / 求余
echo $(($a % $b))
# output: 1

# 乘方
echo $(($a ** $b))
# output: 8

# 复杂运算
echo $(($a + ($a * $b)))
# output: 8
```

## Shell 字符串

Shell 的字符串与 PHP 的字符串相同，分为 `单引号字符串` 和 `双引号字符串`

### 字符串定义

```bash
#!/bin/bash

$name="cizel"
#单引号中变量和符号不会被解析
echo 'my name is ${name}'
# output: my name is ${name}

#双引号中变量和符号会被解析
echo 'my name is ${name}'
# output: my name is $shizhen
```

### 字符串连接

```bash
#!/bin/bash

name="cizel"
echo $name $name
# output:cizel cizel
```

### 字符串长度

```bash
#!/bin/bash

name="cizel"
echo ${#name}
# output: 5
```

### 字符串截取

```bash
#!/bin/bash

name="my name is cizel"

echo ${name:2}
# output: name is cizel

echo ${name:2:5}
# output: name
```

### 字符串删除

${变量名#substring 正则表达式} 从字符串 **开头** 开始配备 `substring`, 删除匹配上的表达式。

${变量名 %substring 正则表达式} 从字符串 **结尾** 开始配备 `substring`, 删除匹配上的表达式。

```bash
#!/bin/bash

test="/home/work/.vimrc"

echo ${test#/home}
# output: /work/.vimrc
```

or

```bash
#!/bin/bash

# 高级用法

test="/home/work/.vimrc"

# 快速获取文件名
echo ${test##*/}
# output: .vimrc

# 快速获取路径
echo ${test%/*}
# output: /home/work
```

### 字符串替换

使用内置的字符串替换，会比 `awk`, `sed`, `expr` 的性能更好，

${变量 / 查找 / 替换值} 一个"/"表示替换第一个，"//"表示替换所有。

```bash
#!/bin/bash

test="/home/work/.vimrc"

echo ${test/.vimrc/.zshrc}
# output: /home/work/.zshrc

echo ${test/w*k/cizel}
# output: /home/cizel/.vimrc
```

## Shell 逻辑运算

在 Shell 中，使用 `test` 来进行逻辑判断。与其他编程语言有许多不同，如果为真返回 `0`, 假返回 `1`.

> 可能的坑：

> 1. 逻辑判断结果真返回 `0`, 假返回 `1`
> 2. 使用 `-gt`, `-lt`, `-ge`, `-le`, `-ne`, `-eq`  替换 `>`, `<`, `>=`, `<=`, `!=`, `=` 做数值比较
> 2. 与或非运算符使用 `-a`, `-o`, `!` 替换 `&` `|` `!`

### 数值比较

数值比较的运算符和汇编语言中类似，常见的 5 种数值比较如下：

| 符号 | 英文解释 | 中文解释 |
| ---  |   ---  |   ---    |
| `-gt` | greater than | 大于 |
| `-lt` | less than | 小于 |
| `-ge` | greater equal | 大于等于 |
| `-le` | less equal | 小于等于 |
| `-ne` | not equal | 不等于 |
| `-eq` | equal | 等于 |

```bash
#!/bin/bash

# 大于
test 3 -gt 2; echo $?
# output: 0

# 小于
test 3 -lt 2; echo $?
# output: 1

# 大于等于
test 3 -ge 2; echo $?
# output: 0

# 小于等于
test 3 -le 2; echo $?
# output: 1

# 不等于
test 3 -ne 2; echo $?
# output: 0

# 等于
test 3 -eq 2; echo $?
# output: 1
```

### 字符串比较

字符串比较的运算符如下表：

| 符号 | 解释 |
| ---  | --- |
| `=` |  字符串等于 |
| `!=` | 字符串不等 |
| `-z` | 判断字符串长度是否为零 |
| `-n` | 判断字符串长度是否大于零 |

```bash
#!/bin/bash

# 字符串等于
test "my name is cizel" = "my name is cizel"; echo $?
# output: 0

# 字符串不等
test "my name is cizel" = "my name is cz"; echo $?
# output: 1

# 字符串长度判断
test -z "my name is cizel"; echo $?
# output: 1
test -n "my name is cizel"; echo $?
# output: 0
```

### 文件比较

| 符号 | 解释 |
| ---  | --- |
| `-e` | 判断文件是否**存在**. |
| `-d` | 判断文件是否为**目录**. |
| `-f` | 判断文件是否为**常规文件**. |
| `-L` | 判断文件是否为**符号链接**. |
| `-r` | 判断文件是否**可读**. |
| `-w` | 判断文件是否**可写**. |
| `-x` | 判断文件是否**可执行**. |

```bash
#!/bin/bash

ls -l

# 当前目录有如下文件，lib 文件夹，run.sh 文件，sh 符号链接，当前角色：work
# drwxr-xr-x 1 work work 4096 Jun 28  2018 lib
# -rwxr-xr-x 1 work work 2364 Jul  7  2018 run.sh
# lrwxrwxrwx 1 root root  4 May 26  2014 sh -> bash

# 判断文件是否存在
test -e run.sh; echo $?
# output: 0

# 判断目录是否存在
test -d lib; echo $?
# output: 0

# 判断文件是否为常规文件
test -f run.sh; echo $?
# output: 0

# 判断文件是否为符号链接
test -L sh; echo $?
# output: 0

# 判断文件是否为符号链接
test -L sh; echo $?
# output: 0

# 判断文件是否可读 / 写 / 执行 （当前角色 work, 权限 rwx, 可读可写可执行）
test -r run.sh; echo $?
# output: 0
test -w run.sh; echo $?
# output: 0
test -x run.sh; echo $?
# output: 0
```

### 逻辑连接

与其他编程语言一样，Shell 中也有与或非运算符。用于连接逻辑判断条件，形成复合的逻辑判断。

| 符号 | 英文解释 | 中文解释 |
| ---  |   ---  |   ---    |
| `-a` | and | 与 |
| `-o` |  or | 或 |
| `!` | -- | 非 |

```bash
#!/bin/bash

# 与
test "1" = "1" -a "1" = "2"; echo $?
# output: 1

# 或
test "1" = "1" -o "1" = "2"; echo $?
# output: 0

#非
test ! "1" = "2"; echo $?
# output: 0
```

## Shell 选择结构

Shell 中的选择语句和其他编程语言类似，支持 if, if-else, if-elif, if-elif-else, case-esac 常见的条件选择方式

> 可能的坑：
>
> - if 条件的左括号 (`[`) 后必须有一个空格，右括号前 (`]`) 必须有一个空格。if [<kbd>空格</kbd>expression<kbd>空格</kbd>]
> - if, elif 后面都需要加 `then` 然后添加语句

### if 选择

```bash
#!/bin/bash

var=`uname -s`

if [ $var = "Linux" ]; then
  echo "Linux System"
fi
```

### if-else 选择

```bash
#!/bin/bash

var=`uname -s`

if [ $var = "Linux" ]; then
  echo "Linux System"
else
  echo "Other System"
fi
```

### if-elif 选择

```bash
#!/bin/bash

var=`uname -s`

if [ $var = "Linux" ]; then
  echo "Linux System"
elif [ $var = "FreeBSD" ]; then
  echo "FreeBSD System"
fi
```

### if-elif-else 选择

```bash
#!/bin/bash

var=`uname -s`

if [ $var = "Linux" ]; then
  echo "Linux System"
elif [ $var = "FreeBSD" ]; then
  echo "FreeBSD System"
else
  echo "Other System"
fi
```
### case-esac 选择

case-esac 与常用的 switch-case 类似，可以对比 if-elif-else 选择食用

```bash
#!/bin/bash

var=`uname -s`

case $var in
"Linux")
  echo "Linux System"
  ;;
"FreeBSD")
  echo "FreeBSD System"
  ;;
*)
  echo "Other System"
  ;;
esac
```

## Shell 循环结构

### for 循环

常见的类似 c 语言的写法

```bash
#!/bin/bash

# 打印 1-10, 必须使用双括号，使符号转移
for ((i=1; i<=10; i++)); do
  echo $i
done
```

in 的方法 (**常用**)

```bash
#!/bin/bash

for i in {1..10}; do
  echo $i
done
```

### while 循环

```bash
#!/bin/bash

count=1
while [ $count -lt 3 ]; do
  echo $count
  count=$((count + 1))
done
```

### until 循环

直到条件为真时，停止循环

```bash
#!/bin/bash

count=1
until [ $count -eq 3 ]; do
  echo $count
  count=$((count + 1))
done
```

## Shell 函数

Shell 函数，使用 $1..$n 的方式接收参数

```bash
#!/bin/bash

my_func() {
  echo "my function"
  echo "params 1: $1"
  echo "params 2: $2"
  echo "params 3: $3"
}

my_func 1 2 3
```

## Shell 加载脚本

Shell 中使用 `source` 命令可以加载其他文件到当前 Shell 脚本中

```bash
# echo.sh

echo() {
  command printf %s\\n "$*" 2>/dev/null
}
```

```bash
#!/bin/bash

source echo.sh

echo 123
```

## 相关链接

- [linux 几种常见的 Shell](https://blog.csdn.net/whatday/article/details/78929247)
- [Shell 脚本编程 30 分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)
- [linux shell 字符串操作（长度，查找，替换）详解](https://www.cnblogs.com/chengmo/archive/2010/10/02/1841355.html)
- [linux shell 正则表达式 (BREs,EREs,PREs) 差异比较](https://www.cnblogs.com/chengmo/archive/2010/10/10/1847287.html)
- [Shell 风格指南](http://zh-google-styleguide.readthedocs.io/en/latest/google-shell-styleguide/contents/)
