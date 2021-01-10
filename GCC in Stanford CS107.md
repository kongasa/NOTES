# GCC in Stanford CS107

GNU Compiler Collection

## 最简单的使用方式

```bash
$ gcc hello.c
$ 
```

gcc会根据在`.c`文件中的声明读取`.h`文件，故在编译过程中不应出现`.h`文件。如果编译成功，不会得到输出，文件目录中会生成一个叫`a.out`的文件，使用`./`来执行。

```bash
$ ./a.out
Hello, World!
$ 
```

## 可选项

通过设置可选项`-o programName`可以自行设置程序的名字。

```bash
$ gcc hello.c -o hello
$ ./hello
Hello, World!
$
```

使用优化可选项`-O`即大写O对程序进行优化。

1. **-O**或**-O1**至**-O3**:优化程度逐渐加深，通常**-O3**为最佳选项
2. **-O0**不进行优化，为默认值，减少编译时间
3. **-Os**在**-O2**的基础上去掉了使代码量增大的优化，添加了减少代码量的优化
4. **-Ofast**在**-O3**的基础上添加了非标准化的优化
5. **-Og**在**-O1**的基础上去掉了影响调试过程的优化

`-std=gnu99`意为使用GNU C99标准进行编译。`-g`生成调试信息，用于使用debugger (gdb) 进行debug。例子：

```bash
$ gcc -std=gnu99 -g -Og loop.c -o loop
```

## 参考

[Compiling C Programs with GCC](https://web.stanford.edu/class/cs107/resources/gcc)

[GCC 优化级别](https://cloud.tencent.com/developer/article/1524971)



