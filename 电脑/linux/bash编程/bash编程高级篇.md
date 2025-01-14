<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline1">1. bash里面的特殊符号</a></li>
<li><a href="#orgheadline7">2. bash里面的array</a>
<ul>
<li><a href="#orgheadline2">2.1. 赋值和引用</a></li>
<li><a href="#orgheadline6">2.2. 其他array操作</a>
<ul>
<li><a href="#orgheadline3">2.2.1. 两个array合并在一起</a></li>
<li><a href="#orgheadline4">2.2.2. 字符串刷成array</a></li>
<li><a href="#orgheadline5">2.2.3. 简单的文件刷成列表</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#orgheadline8">3. bash批处理</a></li>
</ul>
</div>
</nav>


# bash里面的特殊符号<a id="orgheadline1"></a>

-   $0 本命令名字
-   $1 $2 &#x2026; 命令接受的参数
-   $@ 所有参数
-   $? 命令运行返回的状态
-   $! 命令运行最后一个进程号
-   && `cmd1 && cmd2` 前一个命令执行成功才执行第二个命令
-   ; 如果某个命令以 `;` 结尾，则意思是逐个执行这些命令
-   & 如果某个命令以 `&` 结尾，则该命令是异步的和后台执行的

更多信息请参看 [这个网页](http://stackoverflow.com/questions/5163144/what-are-the-special-dollar-sign-shell-variable) 。

# bash里面的array<a id="orgheadline7"></a>

bash编程最好不要过多涉及高级编程的那些内容，那将是很痛苦的，但是在某些情况下你可能需要了解array这个概念。下面来演示这样一个例子，其需求就是在一个自动备份程序之上再加上自动删除逻辑。

```sh
#! /bin/bash

DATE=$(date +%F)
DAYS=30

## here is the autodump code, and the output filename is whatdump_${DATE}

### the allowed list
ALLOWED[0]=whatdump_${DATE}

for (( i=1; i&lt;=${DAYS}; i++ ))
do  ALLOWED[i]=whatdump_$(date -d "now -${i}days" +"%F")
done


### use the python script , it is really hard to write it on bash
python ~/thepython/scripts/whatdump_autoremove.py ${ALLOWED[@]}
```

这个程序整个逻辑就是创建一个名字叫做"whatdump\_\({DATE}"的备份文件（你可以通过cron后台服务来控制好每天运行一次），然后我们希望目标文件只保留最近三十天的文件。因为这个逻辑较为复杂，本来我是打算直接将 ~\){DATE}~ 参数传递给python脚本来做接下来的工作的，但是在了解到date命令强大的人类友好的日期时间表达功能（请参考 [这个网页](http://unix.stackexchange.com/questions/24626/quickly-calculate-date-differences) ），我决定将整个允许存在的文件名都传递进python脚本中去。

有兴趣的可以了解下date命令-d选项的灵活表达支持，比如 date -d "now -3days" 就是现在之前三天的日期时间，而 date -d "+3weeks" 就是现在三周之后的日期时间。用兴趣的可以继续了解下，这个date命令的-d选项真的好强大。比如下面这几行代码（来自 [这个网页](http://www.thegeekstuff.com/2013/05/date-command-examples/) ）:

```sh
$ date --date='3 seconds ago'
Mon May 20 21:59:20 PDT 2013

$ date --date="1 day ago"
Sun May 19 21:59:36 PDT 2013

$ date --date="yesterday"
Sun May 19 22:00:26 PDT 2013

$ date --date="1 month ago"
Sat Apr 20 21:59:58 PDT 2013

$ date --date="1 year ago"
Sun May 20 22:00:09 PDT 2012
```

好了下面详细讲一下这里array涉及到的语法。

## 赋值和引用<a id="orgheadline2"></a>

    =>array[0]="hello"
    =>array[1]="world!"
    =>echo ${array[0]} ${array[1]}
    hello world!

这种表达和我们常见的那种数组概念相近，记得索引从0开始这个惯例即可。然后或者如下一起赋值:

    =>array2=("hello" "python")
    =>echo ${array2[@]}
    hello python

上面的 \({arrayname[@]} 这种表达方式就是引用所有的array，这个 ~@~ 符号是特殊符号。然后还有 ~\){#arrayname[@]}~ 这种表达是返回这个array的长度。

    =>echo ${#array2[@]}
    2

然后最后这句:

    python ~/thepython/scripts/whatdump_autoremove.py ${ALLOWED[@]}

就是将收集到的所有允许的文件名array传递进python脚本中去了。在python脚本中一个粗糙的做法就是如下这样引用

    def main(args,path=""):
        print args
    
    main(sys.argv[1:])

这样所有的这些array就传递进args里面去了，读者有兴趣可以试一下，这个args在python中就是一个列表对象了。到python里面了更复杂的逻辑我们都可以很快写出来了，我就不说了。

大概类似下面所示，注意这里的pathlib在python3.4之后才会有，之前需要用pip安装之。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from __future__ import print_function
import sys
from datetime import datetime
import os
from pathlib import Path
import shutil


def main(args,path=""):
    allowed = args
    p = Path(path)
    pfolds = [p for p in p.iterdir() if p.is_dir()]
    print(pfolds)
    for p in pfolds:
        if p.name in allowed:
            print(p.name,"passed")
        else:
            print(p.name,"removed")
            ### really do the remove thing
            shutil.rmtree(p.name)

if __name__ == '__main__':
    ### 切换到autodump目录
    os.chdir(os.path.expanduser("~/autodump"))

    main(sys.argv[1:])
```

## 其他array操作<a id="orgheadline6"></a>

你可能还会需要在bash中进行其他array操作，只是一般能够传递python就传进python中去，然后后面的再深加工吧。更多内容读者请参看 [这个网页](http://www.thegeekstuff.com/2010/06/bash-array-tutorial/) 。下面我简介一下我觉得应该还是有点用的内容。

### 两个array合并在一起<a id="orgheadline3"></a>

这在合并某些array结果然后传递进python脚本中有用。

    =>Unix1=('Debian' 'Red hat' 'Ubuntu' 'Suse')
    =>Unix2=('Fedora' 'UTS' 'OpenLinux')
    =>Unix3=("${Unix1[@]}" "AIX" "HP-UX")
    =>Unix4=("${Unix2[@]}" "${Unix3[@]}")
    =>echo ${Unix3[@]}
    Debian Red hat Ubuntu Suse AIX HP-UX
    =>echo ${Unix4[@]}
    Fedora UTS OpenLinux Debian Red hat Ubuntu Suse AIX HP-UX

### 字符串刷成array<a id="orgheadline4"></a>

看到这里，我想读者应该猜到了，圆括号里面放着一列字符串然后就自动成为array了:

    =>string="hello bash"
    =>test=(${string})
    =>echo ${test[@]}
    hello bash
    =>echo ${test[1]}
    bash
    =>echo ${test[0]}
    hello

### 简单的文件刷成列表<a id="orgheadline5"></a>

这里就简单复制前面提及的那个网页的代码了，其实说白了还是和字符串转array一个道理，就是读取了一下文件即可。

    #Example file
    $ cat logfile
    Welcome
    to
    thegeekstuff
    Linux
    Unix
    
    $ cat loadcontent.sh
    #!/bin/bash
    filecontent=( `cat "logfile" `)

# bash批处理<a id="orgheadline8"></a>

这个在bash编程123那里也说一点，这里再强调一下，python脚本的命令行参数，如果是输入文件，我喜欢简单起见就单个文件，然后具体bash的时候，如下简单写一下批量处理命令即可，很简单的，就不用麻烦python脚本那边又要处理单个文件的情况又要处理多个文件的情况，还要缩进一次，多麻烦啊。

```bash
for f in $(ls *.pdf); do infome_image_convert_format --outputformat=png ${f} --dpi=300 ; done
```