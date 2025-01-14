<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline1">1. 前言</a></li>
<li><a href="#orgheadline3">2. helloworld</a>
<ul>
<li><a href="#orgheadline2">2.1. makefile</a></li>
</ul>
</li>
<li><a href="#orgheadline8">3. hi</a>
<ul>
<li><a href="#orgheadline4">3.1. fprint函数</a></li>
<li><a href="#orgheadline5">3.2. strcmp函数</a></li>
<li><a href="#orgheadline6">3.3. main函数</a></li>
<li><a href="#orgheadline7">3.4. getenv函数</a></li>
</ul>
</li>
<li><a href="#orgheadline9">4. 符号常量</a></li>
<li><a href="#orgheadline20">5. 附录</a>
<ul>
<li><a href="#orgheadline12">5.1. 编译器</a>
<ul>
<li><a href="#orgheadline10">5.1.1. -c选项</a></li>
<li><a href="#orgheadline11">5.1.2. -o选项</a></li>
</ul>
</li>
<li><a href="#orgheadline15">5.2. 变量声明</a>
<ul>
<li><a href="#orgheadline13">5.2.1. 符号位控制</a></li>
<li><a href="#orgheadline14">5.2.2. 字符类型补充说明</a></li>
</ul>
</li>
<li><a href="#orgheadline19">5.3. 常量</a>
<ul>
<li><a href="#orgheadline16">5.3.1. const修饰符</a></li>
<li><a href="#orgheadline17">5.3.2. static修饰符</a></li>
<li><a href="#orgheadline18">5.3.3. register修饰符</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#orgheadline21">6. 参考资料</a></li>
</ul>
</div>
</nav>


# 前言<a id="orgheadline1"></a>

本文既不是一个c语言参考手册，也不是c语言指南手册，实际上我并不打算用c语言来编码，只是有时会或多或少接触一些c语言或者c++语言的代码，然后学习的知识整理在这里。而且就算将来我也没有比如在python中调用c语言写的代码的那种意愿，实际上我讨厌这种混合语言的做法，我更喜欢的风格是以系统为平台来混合多种语言写的工具来协作，或者以数据库为平台来混合多种工具从而达到某种效果。所以也许以后我会考虑用c语言写一些工具放入系统中，然后通过bash或者python中通过subprocess等来调用吧。所以本文的学习需求主要有两个:

1.  学习某些linux系统相关领域不可避免接触到的某些c语言代码，以求能够读懂。
2.  编写一些小的脚本工具。

# helloworld<a id="orgheadline3"></a>

```c
#include &lt;stdio.h&gt;//one line comment
/*multiline
comment*/

int main(void){
    printf("Hello,The World!\n");
    return 0;
}
```

首先我们运行 `gcc helloworld.c -o helloworld` 来看一下这个小脚本。应该没啥问题吧。然后我们继续写个简单的makefile文件来安装这个小脚本。

这个c语言脚本很简单，没什么好讲的，若有疑问，请参看 [C编程语言](#orgtarget1) 。

## makefile<a id="orgheadline2"></a>

makefile里面内容其实很丰富的，实际上甚至有点过于复杂了。不过这里只是简单使用其基本功能还是很便利的。

```makefile
project=helloworld
cflags=-g -Wall  ${CCFLAGS}

${project}: 
        ${CC}  helloworld.c -o $@ ${cflags}

clean:
        rm ${project}

install:
        cp ${project} /usr/local/bin

uninstall:
        rm /usr/local/bin/${project}

.PHONY:
        clean uninstall install
```

为了便于理解，上面这个makefile我有意采用了一种和bash接近的风格。前面 `project=` 就是一个定义变量的行为。这个project变量就是本脚本的名字。然后makefile下面的主体部分格式如下:

    target: prerequisites
        the command

具体意思就是要生成target这个文件，首先要确保prerequisites这些依赖文件都在而且是最新的，不在或者不是最新的那么查找对应的目标生成规则继续生成。而对于这个target的生成就是执行下面的bash命令。下面是关于上面例子的一些讲解信息:

1.  *特别要强调，命令前面请用 Tab键 隔开。* 还好emacs虽然设置成为Tab自动切换space，其对于makefile还是能够保留Tab键，其他编辑器就不知道了。这个真的算是一个很不好的设计。

2.  关于变量的使用读者看到上面的例子，我有意采用了类似bash脚本的语法。这么写也是支持的。

3.  `$@` 这个特殊的符号并不是什么神秘东西，其意思就是当前目标的文件名，上面例子中当前目标是 `${target}` ，也就是helloworld，所以这里 `$@` 就是 helloworld。

4.  `.PHONY` 这后面跟着一些生成目标，具体意思就是这些生成目标是伪目标，或者说其并没有生成文件，只是执行了某个命令。

5.  我们注意到上面的 `${CC}` 和 `${CCFLAGS}`  并没有为用户定义，其是make命令的一些默认变量。 `$(CC)` 就是调用系统默认的c编译器，一般为gcc。

6.  `make` 命令不输入任何子命令时，默认执行输出第一个目标命令，一般是本项目目标。

7.  makefile的每个命令都有一个独立的终端，也就是不同的终端不共享变量，所以最好多个命令连接成为一个命令，换行用\\换行。

8.  makefile扫描两边，第一遍变量替换，第二遍依赖关系。变量声明最好跟着对应的规则，还有要保证不能被后面的变量声明改变。

# hi<a id="orgheadline8"></a>

```c
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;stdlib.h&gt;

int main(int argc, char *argv[], char **env_var_ptr){
    if (argc != 2){
        fprintf(stderr,"usage: hi name\n");
        return 1;
    }

    if (strcmp(argv[1],"linux") == 0) {
        printf("hi %s.\n",getenv("USER"));
    }
    else
        printf("sorry, my name is not %s. \n\
my name is linux.\n",argv[1]);

   return(0);
}
```

makefile稍微做了一些修改，这里就不贴代码了。

## fprint函数<a id="orgheadline4"></a>

这里fprint函数可以向具体某个文件输出字符流，这里是stderr，而printf实际上是 `fprintf(stdout,...)` ，更多细节请参看 [这个网页](http://stackoverflow.com/questions/4627330/difference-between-fprintf-printf-and-sprintf) 。

## strcmp函数<a id="orgheadline5"></a>

strcmp函数来自 <string.h> ，其将比较两个字符串，如果相等，则返回0——也就是其结果等于0则表明这两个字符串是相等的。

## main函数<a id="orgheadline6"></a>

每个c语言脚本入口都需要有一个main函数，这里定义的main函数格式实际上就是c语言中定义一个函数的格式。前面的 `int` 表示本函数将会返回一个int值（所以我们看到后面 `return 0` ）。然后后面是参数，这个我们很熟悉了。和python有点区别的是，c语言是静态类型语言，其在每一个变量在使用之前必须声明变量类型，这里函数的本地变量也就是参数需要加个类型声明。main函数的参数:

-   **argc:** 其为本脚本接受的参数的个数，最小也是1，和python的sys.argv一样， `sys.argv[0]` 为本脚本名字。
-   **argv:** 具体刷入的参数字符串数组。
-   **envp:** 传输给该脚本的环境字符串数组。

下面通过这个例子来理解:

```c
#include &lt;stdio.h&gt;

int main(int argc, char *argv[], char *envp[])
{
    while(*envp){
        printf("%s\n",*envp);
        envp +=1;
    }

    int i;
    for (i=0; i&lt;argc;i++){
        printf("argv[%d] = %s\n",i,argv[i]);
    }

    printf("argc count is %d\n",argc);

    return 0;
}
```

请读者运行该脚本来的main函数的更深层次认识。还可以参考这个网页: [main函数argc和argv用法](http://www.thegeekstuff.com/2013/01/c-argc-argv/) 。

其中这段代码:

    int i;
    for (i=0; i<argc;i++){
        printf("argv[%d] = %s\n",i,argv[i]);
    }

为常见的遍历数组的形式，从0开始遍历，然后保证index小于数组的length。然后运行i++。数组的长度可以如下获得 `sizeof(array) / sizeof(array[0])` 。

然后我们注意到在遍历envp这个字符串数组的时候使用了这种古怪的表达:

    while(*envp){
        printf("%s\n",*envp);
        envp +=1;
    }

这是个特例，因为envp其实一个总以null结尾的数组，所以可以这样做。

## getenv函数<a id="orgheadline7"></a>

好了，我们继续来看前面的hi例子。getenv函数来自 <stdlib.h> 。其将根据上面谈及的envp那个环境变量，然后给定一个字符串（相当于key），然后返回对应的value。请参看 [这个网页](http://www.tutorialspoint.com/c_standard_library/c_function_getenv.htm) 。

# 符号常量<a id="orgheadline9"></a>

    #define PI 3.14

# 附录<a id="orgheadline20"></a>

## 编译器<a id="orgheadline12"></a>

一系列的源代码需要经过 **编译** 成为汇编语言代码的目标文件（.o文件），然后 **连接** 在一起成为一个可执行文件。这个编译和连接过程都可以用 **gcc** 工具来完成。如果要编译的是C++语言，那么推荐用 **g++** 命令，其使用方法和命令参数等完全和gcc一样。

### -c选项<a id="orgheadline10"></a>

将源代码编译成为 `.o文件` ，即汇编语言代码。只是编译但不连接。

### -o选项<a id="orgheadline11"></a>

将多个.o文件连接然后生成可执行文件（即机器码），或者直接将多个.c源代码文件编译并连接生成可执行文件。这里的 `-o` 选项后面跟着具体生成的可执行目标的文件名。

## 变量声明<a id="orgheadline15"></a>

记住c语言每一个变量都需要先类型声明才能进一步使用。目前可用的类型声明有：

-   **int:** 整型
-   **float:** 浮点型
-   **char:** 一个字节
-   **short:** 短整型
-   **long:** 长整型
-   **double:** 双精度浮点型

c语言中会经常使用上面的类型声明关键词。这些都和计算机具体运行时变量具体的内存存储方案有关，具体多少多少个bit位的讨论这里就略过了。

### 符号位控制<a id="orgheadline13"></a>

所有的整型（int，short，long）都可以前面加上 **unsigned** 或者 **signed** 来明确表示这个整型无符号位或者有符号位。此外char类型（当用于表示8位整型值时）也类似的可以用unsigned和signed来控制符号位。

比如说char类型8个bit位，那么unsigned char类型的取值范围为 \(0 \sim (2^8-1)\) 即 \(0 \sim 255\) ；而signed char类型的取值范围为 \((-2^7-1) \sim (2^7-1)\) 即 \(-127 \sim 127\) ，其他类型取值范围类推。

### 字符类型补充说明<a id="orgheadline14"></a>

c语言的单个字符用单引号表示，然后双引号表示字符串。在python中单引号和双引号基本上是不区分的。

## 常量<a id="orgheadline19"></a>

python中似乎没有常量这个概念， `#define WHAT 1` 对应python中的语句就是 `WHAT = 1` ，这里的常量变量名都大写是一个很好的习惯。

c语言的枚举常量在python中一般就简单的用列表或者元组表示了。而且即使对于c语言一般也不推荐使用宏，即上面的 `#define` 语句，而是推荐使用下面的const常量修饰符。

### const修饰符<a id="orgheadline16"></a>

const常量修饰符能够让这个变量成为一个常量值，这样这个变量的值 <span class="underline">不能被修改</span> 。在C++语言中对于类的成员函数放在后面一个const修饰符，如double x() const 这样的形式，可以让这个成员函数不能修改本类的存在状态（即所有属性都不能改动）。

既然之后这个变量的值不能再修改了，所有通常这个变量的声明和赋值都如下同时完成的：

    const int x = 5;

### static修饰符<a id="orgheadline17"></a>

声明本变量或者函数只能被本文件的函数所使用，文件中的全局变量默认一般大家都是可以用的，static会让其他文件无法使用某些变量或函数。

### register修饰符<a id="orgheadline18"></a>

python中并无此概念，变量声明时加入register修饰符即表示本变量使用频率非常高，最好放在寄存器里面。

\section{流程控制}
C语言的goto语句不推荐使用，下面不做说明了。然后C语言的其他流程控制语句其实和python的都大同小异。

\subsection{条件控制}
条件控制语句在C语言中和python语言是最小的，它的最简写法甚至花括号都不需要。如下所示：

\begin{tcbcode}[]{c}
if (x == 1) 
    printf("1");
\end{tcbcode}

这里稍作修改，表达式去掉圆括号，后面的分号去掉\footnote{不去掉也可以，不过这些残留只会让别人怀疑你复制的C代码然后删都懒得删掉。}，加上冒号控制好缩进即可。上面并没有花括号，是因为C语言中只有多行代码为一个语句区块结构才需要加上花括号，如果有那么需要把花括号去掉。

\begin{tcbcode}[]{python}
if x == 1:
    print("1")
\end{tcbcode}

下面演示完整的C语言条件控制语句：

\begin{tcbcode}[]{c}
if (x == 1) 
    printf("1");
else if (x == 2)
    printf("2");
else if (x == 3)
    printf("3");
......
else
    printf("hello");
\end{tcbcode}

下面是对应的python语言代码：

\begin{tcbcode}[]{python}
if x == 1:
    print("1")
elif x == 2:
    print("2")
elif x == 3:
    print("3")
......
else:
    print("hello")
\end{tcbcode}

我们看到要做的修改的地方是很少的。

\subsection{switch语句}
switch语句python里面没有，不过可以用上面的if elif 语句代替，下面列出switch语句的样子，会看就行了。

\begin{tcbcode}[]{c}
int a;
switch (a){
case 1:printf("Monday\n");
case 2:printf("Tuesday\n");
......
default:printf("error\n");
}
\end{tcbcode}

\subsection{for语句}
对于C语言的for语句一般推荐规范为如下写法：

\begin{tcbcode}[]{c}
for (i = 0; i < n; i++){
    do someting;
    ...
}
\end{tcbcode}

然后对应的python代码如下：

\begin{tcbcode}[]{python}
for i in range(0,n):
    do something
    ...
\end{tcbcode}

\subsection{while语句}
python中也有while语句，只是用的不是很频繁了。和C语言比较起来也很相似。

\begin{tcbcode}[]{c}
while (condition statement){
    do something;
    ...
}
\end{tcbcode}

然后对应的python代码如下：

\begin{tcbcode}[]{python}
while condition statement:
    do something
    ...
\end{tcbcode}

然后C语言的\textbf{break}和\textbf{continue}的语义和python语言中是一致的。

\section{函数详解}
C语言是函数驱动的语言，其定义好的函数稍加处理就可以非常方便地为如python等高级脚本编程语言所用。其实前面看到的main函数就是定义的一个特殊的函数。

现在让我们看下面的一个计算阶乘的函数：

\begin{tcbinput}{c}{code-works/ch01/factorial/factorial.c}
\end{tcbinput}

这个例子演示如下几个概念：

\begin{itemize}
\item 函数的前面的类型声明是说明这个函数的返回值的类型。
\item 这里我有意采用了一种类似python缩进区块的缩进风格。每个\}{}都表示一个区块结构的结束，从而缩进格回退一个Tab（编辑器都转换成了四个空格）。
\item 这里也演示了函数调用自身函数从而形成了简单地递归风格。
\item 熟悉python的读者会对这种if语句和for语句风格很熟悉。多看多写几个类似的函数对于C语言的条件语句和for循环（类似迭代）语句就会很熟悉了。然后C语言的while语句和python的while语句可以说是完全一致的。
\end{itemize}

关于函数的作用域问题大部分编程语言都大同小异的，即函数内部的变量x和函数外部的x是不同的，函数内部调用x先优先调用本地变量x然后才考虑全局变量x。

\section{指针详解}
C语言的指针其实就是对应的python中的变量的概念，并不是什么深不可测的东西。在python中当你说x=1时，实际存储在内存中的是1这个对象，而x其实就是指针，指向具体内存中的那个1；而在C语言中当你说int x=1时，这个x对应的是具体1这个数字在内存上的存储位置。所以我们可以把python这样的动态高级语言看作其内部实现了一种机制，其\uwave{内部一切变量皆是指针}。

请看下面这个例子：

\begin{tcbcode}[]{c}
#include <stdio.h>
int main(void){
    int x = 1;
    int y = 2;
    int *p;
    p = &x;
    y = *p;
    printf("%d",y);
    return 0;
}
\end{tcbcode}

这里变量x，y，p都是对应的本数值具体在内存上的位置，其中p存储的是指针值。当我们把p的值设置成为\verb+&x+，这里符号\\&{}是取某个变量的具体内存地址。这样p内存储的指针值是指向x这个值的，这个时候p就有点高级语言中的变量的意思了，而\verb+\*p+中的星号\*操作可以看作变量的求值操作。

就简单的变量类型int啊，char等是没有必要动用指针这个概念的，指针这个概念在高级变量类型，比如数组、字符串中特别有用。实际上可以这样说C语言的高级变量类型如数组，字符串，结构体等都是基于指针这个概念的。

当简单声明

\begin{Verbatim}
int a[10];
\end{Verbatim}

这个数组的时候，a就是一个指针值，具体指向数组的开头元素。我们可以把a看作一个高级变量来使用，实际上在使用它的时候和使用python中的变量感觉是一致的。

C语言常常让人困惑的地方就在于其内的变量分为两种，一种是直接数据变量，比如：

\begin{Verbatim}
int x = 1;
\end{Verbatim}

中的x就是元数据变量。其变量名x存储的数据就是本体；另一种是间接数据变量，比如指针变量就是间接数据变量，其内存储的数值并不是代表的本体，而是指针值，要获得其值必须用星号\*索引。

# 参考资料<a id="orgheadline21"></a>

1.  C编程语言[K&R] <a id="orgtarget1"></a>
2.  the C++ program language
3.  C++ GUI Qt4 编程
4.  [指针和引用的区别](http://www.cnblogs.com/kingln/archive/2008/03/29/1129118.html)