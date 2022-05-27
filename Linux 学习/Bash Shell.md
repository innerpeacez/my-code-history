## Bash Shell 学习笔记

### 前戏

####  查看当前运行的 shell

```bash
echo $SHELL
```

#### 查看当前系统安装的所有 shell

```bash
cat /etc/shells
```

#### 进入 Bash 

如果你的当前 Shell 不是 bash 可以执行以下命令切换

```bash
bash
```

#### 查看 bash shell 版本

```bash
echo $BASH_VERSION
或者
bash --version
```



Bash 单个命令一般都是一行，用户按下回车键，就开始执行。有些命令比较长，写成多行会有利于阅读和编辑，这时可以在每一行的结尾加上反斜杠，Bash 就会将下一行跟当前行放在一起解释。

```
$ echo foo bar

# 等同于
$ echo foo \
bar
```



## 分号

分号（`;`）是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令。

```
$ clear; ls
```

上面例子中，Bash 先执行`clear`命令，执行完成后，再执行`ls`命令。

注意，使用分号时，第二个命令总是接着第一个命令执行，不管第一个命令执行成功或失败。





## 命令的组合符`&&`和`||`

除了分号，Bash 还提供两个命令组合符`&&`和`||`，允许更好地控制多个命令之间的继发关系。

```
Command1 && Command2
```

上面命令的意思是，如果`Command1`命令运行成功，则继续运行`Command2`命令。

```
Command1 || Command2
```

上面命令的意思是，如果`Command1`命令运行失败，则继续运行`Command2`命令。

下面是一些例子。

```
$ cat filelist.txt ; ls -l filelist.txt
```

上面例子中，只要`cat`命令执行结束，不管成功或失败，都会继续执行`ls`命令。

```
$ cat filelist.txt && ls -l filelist.txt
```

上面例子中，只有`cat`命令执行成功，才会继续执行`ls`命令。如果`cat`执行失败（比如不存在文件`flielist.txt`），那么`ls`命令就不会执行。

```
$ mkdir foo || mkdir bar
```

上面例子中，只有`mkdir foo`命令执行失败（比如`foo`目录已经存在），才会继续执行`mkdir bar`命令。如果`mkdir foo`命令执行成功，就不会创建`bar`目录了。





## type 命令

Bash 本身内置了很多命令，同时也可以执行外部程序。怎么知道一个命令是内置命令，还是外部程序呢？

`type`命令用来判断命令的来源。

```
$ type echo
echo is a shell builtin
$ type ls
ls is hashed (/bin/ls)
```

上面代码中，`type`命令告诉我们，`echo`是内部命令，`ls`是外部程序（`/bin/ls`）。

`type`命令本身也是内置命令。

```
$ type type
type is a shell builtin
```

如果要查看一个命令的所有定义，可以使用`type`命令的`-a`参数。

```
$ type -a echo
echo is shell builtin
echo is /usr/bin/echo
echo is /bin/echo
```

上面代码表示，`echo`命令既是内置命令，也有对应的外部程序。

`type`命令的`-t`参数，可以返回一个命令的类型：别名（alias），关键词（keyword），函数（function），内置命令（builtin）和文件（file）。

```
$ type -t bash
file
$ type -t if
keyword
```

上面例子中，`bash`是文件，`if`是关键词。





## 快捷键

Bash 提供很多快捷键，可以大大方便操作。下面是一些最常用的快捷键，完整的介绍参见《行操作》一章。

- `Ctrl + L`：清除屏幕并将当前行移到页面顶部。
- `Ctrl + C`：中止当前正在执行的命令。
- `Shift + PageUp`：向上滚动。
- `Shift + PageDown`：向下滚动。
- `Ctrl + U`：从光标位置删除到行首。
- `Ctrl + K`：从光标位置删除到行尾。
- `Ctrl + W`：删除光标位置前一个单词。
- `Ctrl + D`：关闭 Shell 会话。
- `↑`，`↓`：浏览已执行命令的历史记录。



# Bash 的模式扩展

Bash 一共提供八种扩展。

- 波浪线扩展
- `?` 字符扩展
- `*` 字符扩展
- 方括号扩展
- 大括号扩展
- 变量扩展
- 子命令扩展
- 算术扩展

Bash 允许用户关闭扩展。

```
$ set -o noglob
# 或者
$ set -f
```

下面的命令可以重新打开扩展。

```
$ set +o noglob
# 或者
$ set +f
```

## 波浪线扩展 [#](https://wangdoc.com/bash/expansion.html#波浪线扩展) 

波浪线`~`会自动扩展成当前用户的主目录。

```
$ echo ~
/home/me
```

`~/dir`表示扩展成主目录的某个子目录，`dir`是主目录里面的一个子目录名。

```
# 进入 /home/me/foo 目录
$ cd ~/foo
```

`~user`表示扩展成用户`user`的主目录。

```
$ echo ~foo
/home/foo

$ echo ~root
/root
```

上面例子中，Bash 会根据波浪号后面的用户名，返回该用户的主目录。

如果`~user`的`user`是不存在的用户名，则波浪号扩展不起作用。

```
$ echo ~nonExistedUser
~nonExistedUser
```

`~+`会扩展成当前所在的目录，等同于`pwd`命令。

```
$ cd ~/foo
$ echo ~+
/home/me/foo
```

## `?` 字符扩展

`?`字符代表文件路径里面的任意单个字符，不包括空字符。比如，`Data???`匹配所有`Data`后面跟着三个字符的文件名。

```
# 存在文件 a.txt 和 b.txt
$ ls ?.txt
a.txt b.txt
```

上面命令中，`?`表示单个字符，所以会同时匹配`a.txt`和`b.txt`。

如果匹配多个字符，就需要多个`?`连用。

```
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls ??.txt
ab.txt
```

上面命令中，`??`匹配了两个字符。

`?` 字符扩展属于文件名扩展，只有文件确实存在的前提下，才会发生扩展。如果文件不存在，扩展就不会发生。

```
# 当前目录有 a.txt 文件
$ echo ?.txt
a.txt

# 当前目录为空目录
$ echo ?.txt
?.txt
```

上面例子中，如果`?.txt`可以扩展成文件名，`echo`命令会输出扩展后的结果；如果不能扩展成文件名，`echo`就会原样输出`?.txt`。

## `*` 字符扩展 [#](https://wangdoc.com/bash/expansion.html#-字符扩展-1) 

`*`字符代表文件路径里面的任意数量的任意字符，包括零个字符。

```
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls *.txt
a.txt b.txt ab.txt
```

上面例子中，`*.txt`代表后缀名为`.txt`的所有文件。

如果想输出当前目录的所有文件，直接用`*`即可。

```
$ ls *
```

`*`可以匹配空字符，下面是一个例子。

```
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls a*.txt
a.txt ab.txt

$ ls *b*
b.txt ab.txt
```

注意，`*`不会匹配隐藏文件（以`.`开头的文件），即`ls *`不会输出隐藏文件。

如果要匹配隐藏文件，需要写成`.*`。

```
# 显示所有隐藏文件
$ echo .*
```

如果要匹配隐藏文件，同时要排除`.`和`..`这两个特殊的隐藏文件，可以与方括号扩展结合使用，写成`.[!.]*`。

```
$ echo .[!.]*
```

注意，`*`字符扩展属于文件名扩展，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。

```
# 当前目录不存在 c 开头的文件
$ echo c*.txt
c*.txt
```

上面例子中，当前目录里面没有`c`开头的文件，导致`c*.txt`会原样输出。

`*`只匹配当前目录，不会匹配子目录。

```
# 子目录有一个 a.txt
# 无效的写法
$ ls *.txt

# 有效的写法
$ ls */*.txt
```

上面的例子，文本文件在子目录，`*.txt`不会产生匹配，必须写成`*/*.txt`。有几层子目录，就必须写几层星号。

Bash 4.0 引入了一个参数`globstar`，当该参数打开时，允许`**`匹配零个或多个子目录。因此，`**/*.txt`可以匹配顶层的文本文件和任意深度子目录的文本文件。详细介绍请看后面`shopt`命令的介绍。

