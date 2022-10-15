# Makefile

1.变量
---
#### 1.1自定义变量：

=            延迟赋值
:=           立即赋值
？=        空赋值
+=         追加赋值

#### 1.2 自动化变量：

$<         第一个依赖文件
$^         全部依赖文件
$@        目标

#### 1.3 模式匹配

%:          匹配任意多个非空字符(类似于shell的通配符*)

#### 1.4 默认规则

.o文件默认使用.c文件进行编译



例子1： 编译1.c main.c 生成可执行文件main
```makefile
main:1.o main.o
    gcc -o main main.o 1.o

1.o:1.c
    gcc -c 1.c

main.o:main.c
    gcc -c main.c
    
clean:
    rm *.o
    rm main
```

例子2：使用变量
```makefile
CC?=gcc
TARGET=main
OBJS=main.o 1.o

$(TARGET):$(OBJS)
    $(CC) -o $@ $^    //$@:表示目标  $^:表示依赖文件全部集合

main.o:main.c
    $(CC) -c main.c
    
1.o:1.c
    $(CC) -c 1.c
    
clean:
    rm *.o
    rm main
```

例子3：使用模式匹配
```makefile
CC?=gcc
TARGET?=main
OBJS?=main.o 1.o

$(TARGET):$(OBJS)
    $(CC) -o $@ $^
    
%.o:%.c
    $(CC) -c $<  //$<:表示依赖文件第一个元素
    
clean:
    rm *.o
    rm main

```

例子4：默认规则.o默认由.c生成
```makefile
CC?=gcc
TARGET?=main
OBJS?=main.o 1.o

$(TARGET):$(OBJS)
    $(CC) -o $@ $^

//.o默认由.c生成

clean:
    rm *.c
    rm main

```

2.@作用
---
`@echo`和`echo`区别
①

```makefile
all:
    echo "test"
```
执行后终端打印:
echo "test"
test

②
```makefile
all:
    @echo "test"
```
执行后终端打印:
test


3.函数
---
#### 3.1 模式替换

`$(patsubst PATTERN,REPLACEMENT,TEXT)`
功能：搜索TEXT中的单词，符合PATTERN中的则替换为REPLACEMENT

示例：
`$(patsubst %.c,%.o,main.c bar.c)`
将main.c和bar.c替换为main.o和bar.o

用法:假如我们存在一个代表所有.o 文件的变量。定义为“ objects = foo.o bar.o baz.o”。
为了得到这些.o 文件所对应的.c 源文件。我们可以使用：
`$(patsubst %.o,%.c,$(objects)) `

----------
#### 3.2 获取文件名函数

`$(wildcard PATTERN)`
函数功能：列出当前目录下所有符合模式“ PATTERN”格式的文件名。

注意：PATTERN可以使用shell中的通配符
示例：`$(wildcard *.c)`
列出当前目录下全部.c文件

----------

#### 3.3 循环函数

`$(foreach VAR,LIST,TEXT)`

函数功能：把LIST中使用空格分割的单词依次取出赋值给变量VAR，然后执行TEXT表达式。

> 通常与`wildcard函数连用`

示例：
```makefile
DIRS?=a b c d
$(foreach dir,$(DIRS),$(wildcard $(dir)/*.c))   #循环取出a,b,c,d目录下所有.c文件
```


----------

#### 3.4 取文件函数

`$(notdir src/foo.c hacks)`
函数说明：从文件名序列“ NAMES…”中取出非目录部分。目录部分是指最后一个斜线（“ /”）（包括斜线）之前的部分。

示例：
```makefile
DIRS?=a b c d #四个目录
FILES?=$(notdir $(foreach dir,$(DIRS),$(wildcard $(dir)/*c)))

.PHONY:all
all:
    @echo "$(FILES)"
```