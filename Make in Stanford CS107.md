# Make in Stanford CS107

*Make*用于构建 (build) 有依赖关系的项目，它只对改变的文件进行重编译，避免大量命令行代码。

## 使用方法

最简单的方式是在拥有`Makefile`的目录下使用`make`指令，如

~~~bash
$ make
gcc -g -Og -std=gnu99  -o hello helloWorld.c helloLanguages.c
$ ./hello
Hello World
Hallo Welt
Bonjour monde
~~~

## 文件结构

~~~makefile
#
# A very simple makefile
#

# The default C compiler
CC = gcc

# The CFLAGS variable sets compile flags for gcc:
#  -g          compile with debug information
#  -Wall       give verbose compiler warnings
#  -O0         do not optimize generated code
#  -std=gnu99  use the GNU99 standard language definition
CFLAGS = -g -Wall -O0 -std=gnu99

hello: helloWorld.c helloLanguages.c hello.h
    $(CC) $(CFLAGS) -o hello helloWorld.c helloLanguages.c

.PHONY: clean

clean:
    rm -f hello *.o
~~~

#开头部分为注释。`CC = gcc`与`CFLAGS = -g -Wall -O0 -std=gnu99`定义了使用的变量。`hello:`一行告诉Make当三个文件中的任意一个发生改变时，重新编译 (re-compile)。此行后应紧跟tab且无空格的输入命令，这里使用`$()`进行变量引用。`clean:`一行使得能够通过`make clean`命令清除编译相关文件，同样需要在下行以tab开头键入命令。只键入`make`时会默认执行第一条target指令（见后文），不需要输入`make hello`。

## 一份更长的例子

```makefile
#
# A simple makefile for managing build of project composed of C source files.
#


# It is likely that default C compiler is already gcc, but explicitly
# set, just to be sure
CC = gcc

# The CFLAGS variable sets compile flags for gcc:
#  -g          compile with debug information
#  -Wall       give verbose compiler warnings
#  -O0         do not optimize generated code
#  -std=gnu99  use the GNU99 standard language definition
CFLAGS = -g -Wall -O0 -std=gnu99

# The LDFLAGS variable sets flags for linker
#  -lm   says to link in libm (the math library)
LDFLAGS = -lm

# In this section, you list the files that are part of the project.
# If you add/change names of source files, here is where you
# edit the Makefile.
SOURCES = demo.c vector.c map.c
OBJECTS = $(SOURCES:.c=.o)
TARGET = demo


# The first target defined in the makefile is the one
# used when make is invoked with no argument. Given the definitions
# above, this Makefile file will build the one named TARGET and
# assume that it depends on all the named OBJECTS files.

$(TARGET) : $(OBJECTS)
    $(CC) $(CFLAGS) -o $@ $^ $(LDFLAGS)

# Phony means not a "real" target, it doesn't build anything
# The phony target "clean" is used to remove all compiled object files.
# 'core' is the name of the file outputted in some cases when you get a
# crash (SEGFAULT) with a "core dump"; it can contain more information about
# the crash.
.PHONY: clean

clean:
    rm -f $(TARGET) $(OBJECTS) core
```

其组成部分依次为

### Macros

`CC = gcc`类似于`#define`语句。`$(SOURCES:.c=.o)`相当于`demo.o vector.o map.o`。`$@`和`$^`为内建macro符号，分别指代`demo`和`demo.o vector.o map.o`

### Targets

格式为

```makefile
target-name : dependencies
    action
```

`target-name`通常是构建时产生的文件的名字。`dependencies`指target在被构建时必须存在且保持最新的文件。Make会识别一些隐式的target，比如每个`.o`文件都有一份如下的隐式target：

```makefile
[filename].o : [filename].c
    $(CC) $(CFLAGS) -o [filename].o [filename].c
```

Make会递归地处理依赖文件。如果某个依赖文件同时拥有自己的target，那么Make会首先处理此依赖文件的target。Make会根据文件的存在与否和时间戳判断是否对某个target进行编译。

target的命令通常需要使用编译器，但是实际上它可以是任何一条可以产生具有target名称的文件的指令。

#### phony targets

一种伪target，Make遇到时会直接执行命令而不进行依赖检查。需要通过加入到`.PHONY`的依赖文件中进行声明。

```makefile
.PHONY: clean
```

## 原文

[Compiling Programs with Make](https://web.stanford.edu/class/cs107/resources/make)