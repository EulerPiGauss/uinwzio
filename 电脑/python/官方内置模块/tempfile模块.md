<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline1">1. TemporaryFile</a></li>
<li><a href="#orgheadline2">2. NamedTemporaryFile</a></li>
<li><a href="#orgheadline3">3. TemporaryDirectory</a></li>
</ul>
</div>
</nav>

有时可能程序需要创建某个临时文件或者临时文件夹，用完之后就删除掉。python的tempfile模块可以便捷的实现这个功能，并具有跨平台特性。更多信息请参考 [官方文档](https://docs.python.org/3.4/library/tempfile.html) 。

# TemporaryFile<a id="orgheadline1"></a>

返回一个临时文件对象，由tempfile.mkstemp函数生成。在执行close语句或者跳出with语句或者最终垃圾回收的时候，该文件将被删除掉。默认的文件模式是'w+b'，所以这个临时文件可以读和写而不需要关闭该文件。具体其参数设置如下:

    TemporaryFile(mode='w+b', buffering=None, encoding=None, newline=None, suffix='', prefix='tmp', dir=None)

简单的使用如下所示:

    >>> import tempfile
    >>> tmpf = tempfile.TemporaryFile()
    >>> tmpf.name
    4
    >>> tmpf.write(b'test hello')
    10
    >>> tmpf.read()
    b''
    >>> tmpf.seek(0)
    0
    >>> tmpf.read()
    b'test hello'
    >>> tmpf.write(b'test hello')
    10
    >>> tmpf.seek(0)
    0
    >>> tmpf.read()
    b'test hellotest hello'
    >>> tmpf.close()

在上面例子中， `tmpf.name` 返回的是临时文件的句柄号<file descriptor>，在 [这个网页](http://stackoverflow.com/questions/18280245/where-does-python-tempfile-writes-its-files) 中指出，该临时文件出于安全性考虑是被隐藏起来的，其他的进程是不能读写它的。然后如果你使用NamedTemporaryFile来创建一个临时文件对象，该临时文件将会有一个名字，具体如下所示。

# NamedTemporaryFile<a id="orgheadline2"></a>

    >>> tmpf = tempfile.NamedTemporaryFile()
    >>> tmpf.name
    '/tmp/tmpe_9pw819'

这个临时文件是找得到的，读者可以到tmp文件夹下看一下。

临时文件具体是通过tempfile.mkstemp函数来创建的。临时文件在读取的时候如上所示注意用seek调整指针位，然后write是附加模式。临时文件close之后就会被自动删除，如果采用with语句，如下所示，出来之后也会自动删除:

    with tempfile.NamedTemporaryFile() as tempfile:
        print('created temporary file', tempfile.name)
        import time
        time.sleep(5)

# TemporaryDirectory<a id="orgheadline3"></a>

我们注意到上面创建临时文件的时候可以设置dir参数，其可以具体设置临时文件放在那里。你在设置dir参数时需要确保该文件夹是存在的，而最好这个文件夹也是临时的。推荐的使用方式如下所示:

    import tempfile
    
    with tempfile.TemporaryDirectory() as tmpdirname:
        print('temporary diretory',tmpdirname)
        with tempfile.NamedTemporaryFile(dir=tmpdirname) as tempfile:
            print('created temporary file', tempfile.name)
            import time
            time.sleep(5)

这将创建一个临时文件夹，其有具体的临时文件名，传递给了临时文件的dir参数，除了with语句之后，临时文件，临时文件夹都将删除。

临时文件夹具体是通过tempfile.mkdtemp函数来创建的。

此外还值得一提的是，在上面的临时文件夹模式下，哪怕不是临时文件最后都将随着临时文件夹的自动删除而一起删除。所以你也不一定要使用TemporaryFile等操作方式，常规的文件操作也行。

```python
import tempfile

with tempfile.TemporaryDirectory() as tmpdirname:
    print('temporary diretory',tmpdirname)
    with tempfile.NamedTemporaryFile(dir=tmpdirname) as tempfile:
        print('created temporary file', tempfile.name)
        import time
        time.sleep(5)
    with open(tmpdirname+'/test.txt','w') as f:
        f.write('test')
    import time
    time.sleep(5)
```