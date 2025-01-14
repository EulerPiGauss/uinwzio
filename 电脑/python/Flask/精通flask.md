<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline2">1. 前言</a>
<ul>
<li><a href="#orgheadline1">1.1. 第一个例子</a></li>
</ul>
</li>
<li><a href="#orgheadline27">2. flask基础</a>
<ul>
<li><a href="#orgheadline6">2.1. 路径分发和对应的视图函数</a>
<ul>
<li><a href="#orgheadline3">2.1.1. 路径分发带变量</a></li>
<li><a href="#orgheadline4">2.1.2. 构建自己的刷url的参量类型</a></li>
<li><a href="#orgheadline5">2.1.3. HTTP Methods</a></li>
</ul>
</li>
<li><a href="#orgheadline7">2.2. url_for函数</a></li>
<li><a href="#orgheadline8">2.3. request对象</a></li>
<li><a href="#orgheadline14">2.4. session对象和g对象和简单的登录机制</a>
<ul>
<li><a href="#orgheadline9">2.4.1. 测试用</a></li>
<li><a href="#orgheadline10">2.4.2. 应用添加数据库</a></li>
<li><a href="#orgheadline11">2.4.3. 应用用户登录识别</a></li>
<li><a href="#orgheadline12">2.4.4. secret_key的设置</a></li>
<li><a href="#orgheadline13">2.4.5. flash函数</a></li>
</ul>
</li>
<li><a href="#orgheadline15">2.5. app的配置</a></li>
<li><a href="#orgheadline16">2.6. jinja2模板</a></li>
<li><a href="#orgheadline17">2.7. 静态文件</a></li>
<li><a href="#orgheadline18">2.8. redirect函数</a></li>
<li><a href="#orgheadline23">2.9. abort函数抛出某个异常</a>
<ul>
<li><a href="#orgheadline19">2.9.1. 404 Not Found</a></li>
<li><a href="#orgheadline20">2.9.2. 403 Forbidden</a></li>
<li><a href="#orgheadline21">2.9.3. 410 Gone</a></li>
<li><a href="#orgheadline22">2.9.4. 500 Internal Server Error</a></li>
</ul>
</li>
<li><a href="#orgheadline24">2.10. 异常捕捉</a></li>
<li><a href="#orgheadline25">2.11. logger</a></li>
<li><a href="#orgheadline26">2.12. 文件上传</a></li>
</ul>
</li>
<li><a href="#orgheadline31">3. 组织你的项目</a>
<ul>
<li><a href="#orgheadline28">3.1. 配置文件的补充说明</a></li>
<li><a href="#orgheadline30">3.2. 使用蓝图来管理你的视图</a>
<ul>
<li><a href="#orgheadline29">3.2.1. url_prefix</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#orgheadline32">4. 表单</a></li>
<li><a href="#orgheadline33">5. 用户管理规范</a></li>
<li><a href="#orgheadline40">6. restful api设计</a>
<ul>
<li><a href="#orgheadline37">6.1. restful api简介</a>
<ul>
<li><a href="#orgheadline34">6.1.1. REST软件架构风格</a></li>
<li><a href="#orgheadline35">6.1.2. REST系统设计规范</a></li>
<li><a href="#orgheadline36">6.1.3. RESTful API</a></li>
</ul>
</li>
<li><a href="#orgheadline38">6.2. 我对restful api的反思</a></li>
<li><a href="#orgheadline39">6.3. flask-restful插件</a></li>
</ul>
</li>
<li><a href="#orgheadline41">7. 缓存</a></li>
<li><a href="#orgheadline58">8. 附录</a>
<ul>
<li><a href="#orgheadline42">8.1. 用gunicorn和nginx配置</a></li>
<li><a href="#orgheadline51">8.2. 在heroku上部署项目</a>
<ul>
<li><a href="#orgheadline43">8.2.1. 指定python2或python3</a></li>
<li><a href="#orgheadline44">8.2.2. 安装第三方模块</a></li>
<li><a href="#orgheadline45">8.2.3. Procfile文件</a></li>
<li><a href="#orgheadline46">8.2.4. hello.py文件</a></li>
<li><a href="#orgheadline47">8.2.5. 安装heroku或者不？</a></li>
<li><a href="#orgheadline48">8.2.6. 通过网页新建项目得到一个好名字</a></li>
<li><a href="#orgheadline49">8.2.7. 或者更简单直接通过github来管理</a></li>
<li><a href="#orgheadline50">8.2.8. 如何在heroku上加入数据库</a></li>
</ul>
</li>
<li><a href="#orgheadline56">8.3. 和apache2一起</a>
<ul>
<li><a href="#orgheadline52">8.3.1. 简单的apache2配置</a></li>
<li><a href="#orgheadline53">8.3.2. Require做了什么</a></li>
<li><a href="#orgheadline54">8.3.3. what.wsgi文件</a></li>
<li><a href="#orgheadline55">8.3.4. 更复杂点的配置</a></li>
</ul>
</li>
<li><a href="#orgheadline57">8.4. 参考资料</a></li>
</ul>
</li>
</ul>
</div>
</nav>


# 前言<a id="orgheadline2"></a>

flask框架底层基于Werkzeug模块和jinja2模块，关于这两个模块的知识将另行讨论。当然本文一开始就假设读者至少已经算得上精通python语言了，然后对网络编程的一些基础知识已有所了解，那么我们继续吧。

## 第一个例子<a id="orgheadline1"></a>

安装推荐就简单用pip2或pip3安装之，然后一个简单的helloworld例子如下所示，读者可以先运行一下看看flask框架安装上去了没有。

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

Flask类提供了所谓的Web Server Gateway Interface，也就是WSGI application。上面代码第一第二行基本上都是这样写的，其中Flask接受的 `__name__` 也就是本py文件作为模块被引入时的名字或者是当前执行py文件 `__main__` ，这样设置是方便flask找到静态文件（比如通过 `rend_template` 函数加载的模板文件，名字是index.html的，flask会在这个app所在的目录下的templates文件夹下面查找这个index.html文件。）等。

然后下面hello\_world函数加上了装饰器 `@app.route('/')` ，意义是明显是在分发url路径。那么这个函数的返回将作为这个url路径的返回值。

# flask基础<a id="orgheadline27"></a>

## 路径分发和对应的视图函数<a id="orgheadline6"></a>

在上面最简单的那个例子中，就已经包含了flask web框架的最核心部分。其包括如下任务: 第一，我们定义该网站的子路径（具体就是对应HTTP协议的GET方法）；第二，如果用户浏览器请求到该路径，那么我们应该给用户浏览器传回去点什么东西。

一般路径分发就是直接把子路径写上去即可，这自不用多说。下面演示分发路径带变量的情况: 

### 路径分发带变量<a id="orgheadline3"></a>

    @app.route('/user/<user>')
    def get_user(user):
        return "user {}".format(user)

用尖括号<>来包围具体的变量名，注意相应的函数应该接受这个参数（装饰器做了一个字典值环境，然后对下面的视图函数对应的变量名进行填充）。具体这个变量在函数中具体数据类型可以用 `<int:id>` 这样的形式来控制，默认是字符串类型。具体如下所示:

-   **string:** 接受任意文本除了url的斜线"/"。
-   **int:** 整型
-   **float:** 浮点型
-   **path:** 类似string，不过也可以接受斜线"/"

这里的视图函数是理解flask框架的关键，比如说后面要谈论的静态文件和模板文件，一个和视图函数内的具体引用有关，一个和视图函数具体输出有关。然后再后面要谈论的重定向和HTTP错误处理也不过是一些对一些特殊的情况进行了特殊的视图函数封装。而上面谈及，这个 `@app.route` 装饰器函数可以接受路径中的变量参数，其通过一个字典值环境变量传递进来，实际上这个字典值环境变量里面还有更多的东西，比如说客户浏览器HTTP的请求，将会封装成为一个request对象。这个 **request** 对象你在视图函数里面一样可以直接用。比如 `request.headers` 就是我们熟知的HTTP请求头文件的字典值。然后 `request.headers.get('User-Agent')` 你就可以获得输入HTTP请求头文件的"User-Agent" 这一行的值。

视图函数最后需要返回一个response对象，其可以是一个简单的字符串或者一个简单的网页文件，你也可以用 `make_response` 函数来构建自己的response对象。

### 构建自己的刷url的参量类型<a id="orgheadline4"></a>

path类型已经可以接受斜线了，所以只适合某个子节点下面全是路径的情况，如 `/dir/some/path` ，因为后面所有的子节点都将传入到那个path参量上去。然后就是虽然flask也允许 `/project/` 这样的url，而且具体和 `/project` 是两个不同的url，我很不喜欢这种url，实际上我们应该完全避免 `/` 结尾的url的情况，包括restful api里面也是。

然后就是我们可以深度定制自己的刷url的参量类型。比如:

    from werkzeug.routing import BaseConverter
    
    class ListConverter(BaseConverter):
        def to_python(self, value):
            return value.split('+')
    
        def to_url(self, values):
            return '+'.join(BaseConverter.to_url(value)
                for value in values)
    
    app.url_map.converters['list'] = ListConverter

主要就是向 `url_map` 的 `converters` 这个字典值里面添加你想要的额外的类型转换，这里是一个列表转换。你需要做的就是子类化 `BaseConverter` 这个类，然后定义好 `to_python` 和 `to_url` 这两个方法。具体一个是根据url刷python变量，另外一个是根据python量返回url中的字符串。

上面这个转换器大体实现了 `/a+b` 刷成 ['a','b'] ，然后反之。这个了解下即可，实在有需要才考虑使用，因为这种转换器是越少越好，写了一些自己的转换器混进去，会让你的程序代码更难读和更难理解，同时自己如果驾驭不好的，会出各种bug吧。

### HTTP Methods<a id="orgheadline5"></a>

如下所示:

    @app.route('/login', methods=['GET', 'POST'])

你可以在分发URL的时候，确定该URL要处理的一些HTTP方法。如果你不加上 **methods** 参数，那么默认是GET方法，而多个方法你需要如下来确定具体视图函数等下要处理的HTTP方法类型:

    if request.method == 'POST':

这个 **request** 参量是每个视图函数自动都会接受的一个变量。这个 **request** 变量是个非常重要的概念，请看下面这个小例子:

```python
@app.route('/',methods=['GET','POST'])
def index():
    if request.method == 'GET':
        query = request.args.get('query')

        ...

        return render_template('index.html',**locals())
```

这里GET方法中URL传递的如 `?key=value` 这些参数对应的就是request对象的args这个字典值，然后你用get方法取之即可。

## url\_for函数<a id="orgheadline7"></a>

`url_for` 函数是一个很有用的函数，你在jinja2模板系统里面也是可以继续使用的。比如你定义的视图函数

    @app.route('/')
    def index():
        return 'hello'

该网页对应的路径就可以这样生成: `url_for('index')` 

再比如你定义的视图函数里面有参量:

    @app.route('/user/<int:userid>')
    def user(userid):
        return str(userid)

该视图函数对应的url请求路径是: `url_for('user',userid=3)`

然后静态文件也可以用 `url_for` 来快速生成引用地址，这个在下面的静态文件一章里面有详细说明。

然后比如说利用 flask-restful 插件搭建你的restful api ，其中有个设置 `endpoint` 的参数，你可以用那里具体设置的endpoint的名字来对应该api的url，如:

    api.add_resource(FoldersAPI,'/api/v1/folders',endpoint='folders')

对应的就是 `url_for('folders')` ，然后值得一提的是，这生成的是相对主站的相对路径，如这样的形式 `/api/v1/folders` ，我不清楚ajax技术会如何，但如果你使用requests模块来进行请求的话，则需要加上 `_external` 选项:

    requests.get(url_for('get_subfolders',path=path,_external=True))

然后如果你加入的其他参数没有在url里面的话则会作为查询参数加入进构建的url中。

    >>> @app.route('/login')
    ... def login(): pass

    ...  print url_for('login', next='/')
    /login?next=/

## request对象<a id="orgheadline8"></a>

因为request对象在flask框架中非常重要，而且里面的变量的值flask都会自动帮你处理好，下面详细介绍之（更多信息请参看[这里](https://flask.readthedocs.org/en/latest/api/#flask.Request)）:

request对象是一个请求上下文环境下的变量，也就是每一个不同的请求request变量的值都不同、

-   **form:** 在网页表单中通过定义 `value` 可以获得一个变量，具体填入的值可以通过 `request.form.get('what')` 来获得。
-   **files:** 网页表单中上传文件的input `type=file` 将传到这里。具体返回对象和python的文件对象类似，除了还提供额外的 `save` 方法用于保存文件。
-   **args:** 就是URL中各个参数。
-   **values:** 相当于上面谈及的form和args这两个字典的合并字典

-   **cookies:** 一个字典值，request传输的cookies值
-   **headers:** 字典值，request请求的headers

-   **get\_json:** request的data如果mimetype是json时（ `application/json` ），接受之后会自动刷成字典值。如果设置 `force=True` ，那么就算mimetype不是json也如此强制执行。
-   **is\_json:** 判断请求发送的数据是否是json格式。
-   **data:** 处理request过来的data，如果请求体发送过来的mimetype是json，那么推荐用 `get_json` 方法来处理之。否则用这个 `request.data` 将不做任何处理只是返回字符串。

-   **max\_content\_length:** HTTP协议里面的 `MAX_CONTENT_LENGTH` 字段值，也就是发送的content的size。具体文件上传情况和json处理情况后面到时再详细讨论之。
-   **method:** POST GET等
-   **path:** 具体分发的路径，也就是 `@route` 的第一个参数。这里值得一提的是如果用gunicorn和nginx这样的架构，具体server\_name的解析会出错，如果你想要表达当前url，用 `request.url` 可能不是你希望的结果，推荐使用 `request.path` 这样的相对表达方式。
-   **url:** 完整url

## session对象和g对象和简单的登录机制<a id="orgheadline14"></a>

每一个请求都会自动新建一个request对象，这是容易理解的，此外每一个视图函数里面还可以使用 `flask.session` 和 `flask.g` 这两个变量。要深刻理解这两个变量就需要理解flask的两种环境: 应用环境和请求环境。

我们看到flask与之相关的 `globals.py` 的源码:

```python
from functools import partial
from werkzeug.local import LocalStack, LocalProxy

def _lookup_req_object(name):
    top = _request_ctx_stack.top
    if top is None:
        raise RuntimeError(_request_ctx_err_msg)
    return getattr(top, name)


def _lookup_app_object(name):
    top = _app_ctx_stack.top
    if top is None:
        raise RuntimeError(_app_ctx_err_msg)
    return getattr(top, name)


def _find_app():
    top = _app_ctx_stack.top
    if top is None:
        raise RuntimeError(_app_ctx_err_msg)
    return top.app


# context locals
_request_ctx_stack = LocalStack()
_app_ctx_stack = LocalStack()
current_app = LocalProxy(_find_app)
request = LocalProxy(partial(_lookup_req_object, 'request'))
session = LocalProxy(partial(_lookup_req_object, 'session'))
g = LocalProxy(partial(_lookup_app_object, 'g'))
```

首先我们看到 `from werkzeug.local import LocalStack, LocalProxy` ，那里有解释信息:

> The Python standard library has a concept called “thread locals” (or thread-local data). A thread local is a global object in which you can put stuff in and get back later in a thread-safe and thread-specific way. That means that whenever you set or get a value on a thread local object, the thread local object checks in which thread you are and retrieves the value corresponding to your thread (if one exists). So, you won’t accidentally get another thread’s data.

他说的意思是python就有一个 `线程本地` 的概念，也就是在该线程内的全局变量。然后 `werkzeug` 提到:

> Werkzeug provides its own implementation of local data storage called werkzeug.local. This approach provides a similar functionality to thread locals but also works with greenlets.

也就是 `werkzeug` 提供了他自己的线程全局变量支持，其同时也支持 `greenlets` 。这里使用到的 `LocalStack` 和 `LocalProxy` 类我们不需要深入了解，一个是堆栈结构有push , pop 方法之类的，一个有类似字典的api。然后flask就是利用这两个类来建立了两个环境: 两个线程本地全局变量。 一个是应用环境，一个是请求环境。

其中session , request 这两个变量是于请求环境堆栈的，也就是每一个新到的请求都是重新创建的。然后 current\_app 和 g 是位于应用环境堆栈的，新建的app都是重新创建的。flask是支持多线程模式的，如这样设置 `app.run(threaded=True)` ，这样每一个线程都有各自的 `flask.g` ，然后flask是通过 `current_app` 来获取当前app的。请参看 [这个网页](https://stackoverflow.com/questions/14672753/handling-multiple-requests-in-flask) ，推荐还是用 `gunicorn` 来解决多线程多请求问题。一般这种需求不太明显的，除非你的网站流量请求特别大了，但如果你的app有特别费时的IO，比如上传或下载大型文件，那么就可能阻塞整个app，这个时候就必须用gunicorn做多线程了。

好吧，为了简单起见我们用 `gunicorn` 来管理多线程，然后flask内部我们认为一个app，一个线程，则这个 `flask.g` 就可以看作一个方便的全局变量管理方式。而 `request` 和 `session` 则是每次请求都不同的，至于 `session` 其每次请求环境搭建时都会自动加载你的 cookies，大体就是这样。

### 测试用<a id="orgheadline9"></a>

在测试的时候你需要自己创建一个应用上下文环境，然后请求通过requests模块来做。如下所示，调用 `app.app_content()` :

    from flask import Flask, current_app
    
    app = Flask(__name__)
    with app.app_context():
        # within this block, current_app points to app.
        print current_app.name

### 应用添加数据库<a id="orgheadline10"></a>

这个例子来自官方文档:

    import sqlite3
    from flask import g
    
    def get_db():
        db = getattr(g, '_database', None)
        if db is None:
            db = g._database = connect_to_database()
        return db
    
    @app.teardown_appcontext
    def teardown_db(exception):
        db = getattr(g, '_database', None)
        if db is not None:
            db.close()

这里的 `@app.teardown_appcontent` 用于上下文环境销毁时需要额外做的动作。

### 应用用户登录识别<a id="orgheadline11"></a>

    @app.before_request
    def get_current_user():
        g.user = None
        email = session.get('email')
        if email is not None:
            g.user = email

这里的逻辑是每次请求先都初始化 `g.user` ，然后尝试从session中获取cookies中保存的用户的值。

### secret\_key的设置<a id="orgheadline12"></a>

你需要为你的session的cookies设置一个密钥，就像这样:

    app.secret_key = 'what...'

可以通过这样获得:

    >>> import os
    >>> os.urandom(24)

具体登录的例子请参看  [flask-examples](https://github.com/a358003542/flask-examples) 的 `session.py` 。

### flash函数<a id="orgheadline13"></a>

具体登录的例子请参看  [flask-examples](https://github.com/a358003542/flask-examples) 的 `session_flash.py` 。

首先在python脚本中通过 `flash` 函数刷如一些信息，然后在 `render_template` 函数在处理模板的时候，request那个参量也传递进去了，这里不是重点。重点是 `get_flashed_messages` 这个函数也传递进去了，这里就是利用这个函数在jinja2模板中获取flash进入的那些信息，这个函数返回的是一个可迭代对象（可能就是一个列表吧）。

## app的配置<a id="orgheadline15"></a>

前面也接触过一些了，比如 `app.config['DEBUG'] = True` ，这个 `app.config` 是一个类似字典对象的对象。一些重要的配置下面列出来，更多的请参看 [这个网页](http://docs.jinkan.org/docs/flask/config.html) 。

-   **DEBUG:** 调试模式
-   **SECRET\_KEY:** 密钥
-   **SERVER\_NAME:** 服务器名和端口，如果你想让flask支持子域名，则需要设置这个。
-   **SEND\_FILE\_MAX\_AGE\_DEFAULT:** 缓存控制的最大期限，默认是12小时。
-   **MAX\_CONTENT\_LENGTH:** 最大content长度，当你需要上传和下载文件的时候，最好把这个设置得大一点。

## jinja2模板<a id="orgheadline16"></a>

具体jinja2模块的基本知识这里就不说了，这里主要说一下jinja2模板系统在flask框架中额外的东西。

1.  config

request
当模版不是在活动的请求上下文中渲染时这个变量不可用。
session

当模版不是在活动的请求上下文中渲染时这个变量不可用。

g
当模版不是在活动的请求上下文中渲染时这个变量不可用。

url\_for()
flask.url\_for() 函数

get\_flashed\_messages()
flask.get\_flashed\_messages() 函数

## 静态文件<a id="orgheadline17"></a>

静态文件有很多解决方案，比如说在你的flask项目里面新建一个 **static** 文件夹，然后在css文件夹里面要引用"boostrap.min.css"文件，那么可以在模板文件中如下引用之:

    <link rel="stylesheet" href="{{ url_for('static',filename='css/bootstrap.min.css') }}">

如果你打开网页，可以看作最终生成的实际是这样的形式:

    <link rel="stylesheet" href="/static/css/bootstrap.min.css">

其他文件的引用大致类似。

然后就是模板文件，你需要在你的flask项目中新建一个 **templates** 文件，然后在里面新建一个index.html文件，那么通过 `render_template` 渲染该文件时你只需要写上"index.html"文件即可，具体如下所示:

    render_template('index.html')

`render_template` 函数后面接受的参数都将刷入模板系统，一个不错的做法如下所示:

    render_template('index.html', **locals())

这样将把本视图函数中所有的本地变量都刷进去，毕竟模板文件有时要刷如的参量太多了，要一个个写实在太麻烦了。

## redirect函数<a id="orgheadline18"></a>

页面重定向函数。

    @app.route('/')
    def index():
        return redirect(url_for('login'))

## abort函数抛出某个异常<a id="orgheadline23"></a>

抛出某个HTTP状态异常

    abort(404)

### 404 Not Found<a id="orgheadline19"></a>

经典的“哎呦，您输入的 URL 当中有错误”消息。这个消息太常见了，即使是 互联网的新手也知道 404 代号的意义: 该死，我寻找的东西不在那儿。确保 404 页面上有一些有用的信息是一个好主意，至少应该提供一个返回主页的链接。

### 403 Forbidden<a id="orgheadline20"></a>

如果您的网站包含一些类型的访问控制，您必须向非法的请求返回 403 错误代号。 所以请确保用户不会在试图访问了一个禁止访问的资源后不知所措。

### 410 Gone<a id="orgheadline21"></a>

您知道 404 Not Found 代号还有一个兄弟名为 410 Gone 么? 很少有人真正实现 它，您可以考虑将其返回给对以前曾经存在、但是现在已经删除的资源的请求，而 不是直接返回 404 。 如果您还没有从数据库里永久删除这个文档，仅仅是将他们 标记为删除。那么可以为用户展示一个消息，说明他们寻找的东西已经永远删除了。

### 500 Internal Server Error<a id="orgheadline22"></a>

通常在出现编程错误或者服务器过载的时候会返回这个错误代号。在这里放一个 漂亮的页面是一个非常好的主意。因为您的应用 总有一天 会出现错误(请参考 记录应用错误 )

## 异常捕捉<a id="orgheadline24"></a>

如下所示:

```python
@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404
```

## logger<a id="orgheadline25"></a>

    app.logger.debug('A value for debugging')

## 文件上传<a id="orgheadline26"></a>

具体例子请参看我写的 [flask-examples](https://github.com/a358003542/flask-examples) 那里，具体是 upload.py 。

# 组织你的项目<a id="orgheadline31"></a>

这里主要讨论更加成熟和稍微复杂点的那种做法。这个参看 [这个网页](http://spacewander.github.io/explore-flask-zh/4-organizing_your_project.html) 。

    run.py
    config.py
    instance/
      /config.py
    project/
      /__init__.py
      /views.py
      /models.py
      /forms.py
      /static/
      /templates/

`run.py` 是作为启动开发服务器的脚本，里面内容一般很简单，大体是:

```python
from project import app

if __name__ == '__main__':
    app.run()
```

这样运行根据后面讲到的两个 `config.py` 的不同的设置来获得开发和工作不同的配置环境。比如这里具体是先加载顶层目录的config.py，然后再加载instance里面的config.py

    config loaded
    instance config loaded
     * Debugger is active!
     * Debugger pin code: 916-109-430

然后 `instance/config.py` 里面一般放着密钥，不纳入git版本控制，具体就是在 `.gitignore` 文件里面加入 `instance/` 。这样的话为了保证远程服务器那边能够正常运行，你需要在那边手工添加这个文件夹和文件，然后在里面加入你的密钥。因为其没有进入版本控制，所以我们还需要加入另外一个不太重要的 `config.py` 文件，其放着关于flask app的不太重要的一些配置信息。然后推荐最后确认加上 `debug=False` ，确保生产环境debug模式没有打开。

在project文件夹里面， `static` 文件夹和 `templates` 文件夹就如上这样放置，然后通过 `run.py` 来运行是可以正常启动的。而在生产环境下，比如通过gunicorn来启动flask的app，则执行如下命令:

    gunicorn -w 4 -b localhost:8001 project:app

这样将直接调用你的project里面的app对象，并且没有打开debug。

`config.py` 和instance里面的config我们如下引入进来，具体在project的 `__init__.py` 里面:

    from flask import *
    app = Flask(__name__, instance_relative_config=True)
    app.config.from_object('config')
    app.config.from_pyfile('config.py')

其中 `instance_relative_config=True` 和 `app.config.from_pyfile('config.py')` 来引入instance文件夹里面的 `config.py` ，然后 `app.config.from_object('config')` 引入顶层目录下的 `config.py` 。

然后 `views.py` 和 `models.py` 里面的含义是很明显的，其中models.py里面一般定义一些数据库的模型，这个在project里面直接引入进来即可，而在 `views.py` 里面定义的一些视图函数我们也需要引入进来。这里值得一提的就是相对引入的用法:

在project的 `__init__.py` 里面有:

    #### more views
    from .views import *

在 `views.py` 里面有:

    from flask import *
    from . import app
    
    @app.route('/')
    def index():
        return render_template('index.html', **locals())

这种相对引入语法不支持直接运行脚本了，而需要在外面，通过 `run.py` 来运行，然后gunicorn那边也一切运行正常。

## 配置文件的补充说明<a id="orgheadline28"></a>

上面提及的 `config.py` 里面大体放着这种形式的配置信息:

    DEBUG = True

然后我们知道关于这些配置变量我们都可以通过 `app.config['WHAT']` 来加载。

上面提及的:

    app.config.from_object('config')
    app.config.from_pyfile('config.py')

先加载顶层目录的 `config.py` 然后再加载instance下的 `config.py` 。这样将形成一种覆盖机制，按照 [这个网页](http://spacewander.github.io/explore-flask-zh/5-configuration.html) 的描述，我们可以在顶层设置:

    DEBUG = False
    SQLALCHEMY_ECHO = False

然后instance那边设置:

    DEBUG = True
    SQLALCHEMY_ECHO = True

生产环境那边将这两行不加上即可。

## 使用蓝图来管理你的视图<a id="orgheadline30"></a>

随着你的项目变大，你可能需要使用蓝图来重构你的项目代码。蓝图有两种风格，一种是功能式架构，只是简单的新建一个视图views文件夹，然后在里面放着你的蓝图或说视图定义；另一种是各自独立的分区式，视图，模板，静态文件都在某个文件夹下面。这里只讨论稍显简单的功能式架构。

最简单的一个蓝图定义文件如下所示:

```python
from flask import *
from .. import app

index = Blueprint('index', __name__)

@index.route('/')
def view_index():
    return render_template('index.html', **locals())
```

大体和之前接触的flask的视图函数定义类似，只是额外新建了一个Blueprint对象，然后使用 `@index.route()` 这样的装饰器来装饰该蓝图的视图函数。

然后在project也就是 `__init__.py` 文件那里，把这个蓝图注册进来。

```python
from .views.index import index
app.register_blueprint(index)
```

最基本的情况就是这样，其他一切照旧。

### url\_prefix<a id="orgheadline29"></a>

`url_prefix` 是你定义一个Blueprint对象是很有用的一个可选参数，意义很明显，就是该蓝图下所有的url都加上了这个前缀。

# 表单<a id="orgheadline32"></a>

请参看 [flask-wtf](flask-wtf.html) 一文。

# 用户管理规范<a id="orgheadline33"></a>

本小节主要参考了 [这个网页](http://spacewander.github.io/explore-flask-zh/12-handling_users.html) 。更成熟的方案请参看 flask-user

# restful api设计<a id="orgheadline40"></a>

这里主要参看了 [这个网页](http://www.pythondoc.com/flask-restful/index.html) ，但原创者在 [这里](https://github.com/sixu05202004/Flask-RESTful-cn) 。

## restful api简介<a id="orgheadline37"></a>

### REST软件架构风格<a id="orgheadline34"></a>

REST(Representational State Transfer) 表征性状态传输是Roy Fielding在2000年提出的一种软件架构风格。目前在三种主流Web服务实现方案中，因为REST模式和复杂的SOAP和XML-RPC相比更加简洁，越来越多的web服务开始采用REST风格设计和实现。

### REST系统设计规范<a id="orgheadline35"></a>

-   **客户端-服务器:** 客户端和服务器之间隔离，服务器提供服务，客户端进行消费。
-   **无状态:** 从客户端到服务器的每个请求都必须包含理解请求所必需的信息。换句话说，服务器不会存储客户端上一次请求的信息用来给下一次使用。
-   **可缓存:** 服务器必须明示客户端请求能否缓存。
-   **分层系统:** 客户端和服务器之间的通信应该以一种标准的方式，就是中间层代替服务器做出响应的时候，客户端不需要做任何变动。
-   **统一的接口:** 服务器和客户端的通信方法必须是统一的。
-   **按需编码:** 服务器可以提供可执行代码或脚本，为客户端在它们的环境中执行。这个约束是唯一一个是可选的。

### RESTful API<a id="orgheadline36"></a>

符合REST设计风格的Web API称为RESTful API。RESTful API的核心概念是 **资源** 。资源可用URI表示，客户端用HTTP协议所定义的方法来请求这些URIs，当然也包括被访问资源状态的改变。

具体HTTP方法和对资源的操作如下所示:

<table>


<colgroup>
<col  class="org-left">

<col  class="org-left">

<col  class="org-left">
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">HTTP方法</th>
<th scope="col" class="org-left">行为</th>
<th scope="col" class="org-left">URI示例</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">GET</td>
<td class="org-left">获取资源信息</td>
<td class="org-left"><http://example.com/api/orders></td>
</tr>


<tr>
<td class="org-left">GET</td>
<td class="org-left">获取某个特定资源</td>
<td class="org-left"><http://example.com/api/orders/123></td>
</tr>


<tr>
<td class="org-left">POST</td>
<td class="org-left">创建新的资源</td>
<td class="org-left"><http://example.com/api/orders></td>
</tr>


<tr>
<td class="org-left">PUT</td>
<td class="org-left">更新资源</td>
<td class="org-left"><http://example.com/api/orders/123></td>
</tr>


<tr>
<td class="org-left">DELETE</td>
<td class="org-left">删除资源</td>
<td class="org-left"><http://example.com/api/orders/123></td>
</tr>
</tbody>
</table>

REST设计不需要特定的数据格式。在请求中数据可以以JSON形式, 或者有时候作为url中查询参数项。

## 我对restful api的反思<a id="orgheadline38"></a>

我觉得目前的restful api 设计有问题，不够实用。我同意api的描述应该偏资源，但应该再增加一类纯动作型api。实际上动作也是一种资源有的时候，但是动作作为资源的时候资源名一定要清晰的显示出来。然后资源的描述应该是URI风格的，弄个id挂在上面没什么用处还没有达到描述资源特征的目的。我们应该用统一的资源URL描述，下面简称为<path:path>

资源应该统一到一个URI上，弄个 `/res` 又还弄个 `/res/id` 纯属浪费。于是我提出建议如下:

1.  资源的描述URI应该侧重位置，然后跟上一个特征词，如下所示:

    /resource_name/<path:res_uri>

1.  我们应该彻底杜绝 `/` 尾的URL。

2.  不用使用 `/resource_name` 这种形式来返回全部uri，推荐使用 `/resource_name/main` 来表达全部资源。其他名字如 `all` 等也是欢迎的，自己内部约定好即可。

3.  方法可以使用如下四种: get post put delete ，其中:
    -   get 用于查询某个资源 （可以接受更多的查询参数 包括uri）
    -   post 用于insert某个资源
    -   put 用于更新某个资源
    -   delete 用于删除某个资源

然后delete可能会被 `/delete_resname/<name>` 这样的URL取代，甚至可能某些资源不需要删除操作，则上面的delete方法不需要编写。

put方法可能被 `/update_resname/<name>` 这样的URL取代，甚至可能insert和update逻辑都融合在post操作里面，则上面的put方法也不需要编写。

在视图层是推荐用上面的 `/delete_resname/<name>` 这样的风格，然后表单提交的时候更改 `formaction` 即可。而formmethod我们知道只能是 get 和 post 。然后如果insert和update逻辑好合并的则推荐合并为upsert逻辑，统一用post方法调用。

restfulapi则可以上面四个方法都编写出来，调用者清晰的知道意义就行了。

1.  返回json统一用如下格式:

    {
        "resource_name" : [
                    {
                           .....
                    }
    }

即使只返回一个资源也用这种格式，只是resource\_name列表里面只包含一个元素罢了，更好的风格，没有元素也保持这种格式，只是里面的列表为空罢了。

## flask-restful插件<a id="orgheadline39"></a>

请参看 [flask-restful](flask-restful.html) 一文。
具体原生态的flask restful api代码编写就不在这里做了，有兴趣的可以看下本节开头提及参考资料的地一小节部分。

# 缓存<a id="orgheadline41"></a>

<http://spacewander.github.io/explore-flask-zh/6-advanced_patterns_for_views_and_routing.html>

# 附录<a id="orgheadline58"></a>

## 用gunicorn和nginx配置<a id="orgheadline42"></a>

gunicorn没啥好说的，就是命令行开个本地端口运行即可，主要是nginx那边的配置需要说一下。

首先nginx的配置文件编辑在这里: `/etc/nginx/sites-available`

然后你需要激活这个网站，则如下在 `sites-enabled` 那里创建一个符号链接即可:

    sudo ln -s /etc/nginx/sites-available/books /etc/nginx/sites-enabled/books

然后nginx配置有很多内容，这里就最简单的说一下:

    server{
            listen 80;
            server_name books.cdwanze.org;
    
            location /{
                     proxy_pass http://localhost:5000;
                     }

这是最简单的配置了，设置简单对外端口 `listen` ，然后设置 `server_name` 。然后 `location /` 那里设置 `proxy_pass` ，对应的就是具体你的guniron那边开的本地端口。参考了 [这个网页](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-14-04) 和 [这个网页](https://realpython.com/blog/python/kickstarting-flask-on-ubuntu-setup-and-deployment/) 。

更复杂的配置需要进一步学习nginx相关知识。

## 在heroku上部署项目<a id="orgheadline51"></a>

在heroku上部署项目其实很简单，只是感觉就是官方教程也谈论得干扰因素有点多了，比如说virtualenv，我会用，但总不喜欢用，不知道怎么回事。官方上手教程把virtualenv引进来一开始给我的感觉就是heroku而运作机制是依赖于virtualenv或者说本地的模块的，这其实是错误的。heroku在远程安装各个python的第三方模块都是通过pip命令来完成的，和你本地virtualenv具体的安装毫无关系，当然了有人会说通过pip freeze命令会很方便，但我真没看出这有多方便，如果这个项目是你自己写的，具体用了那几个模块你自己还不清楚吗。好了，闲话少说，首先谈谈python2和python3的问题。。

### 指定python2或python3<a id="orgheadline43"></a>

在主文件夹目录下你新建一个 `runtime.txt` 文件，里面简单写上：

    python-3.4.3

这样，远程heroku会帮你确定python运行环境为python-3.4.3。

    remote: -----> Installing runtime (python-3.4.3)

就这么简单，同时要注意简写 `python3` 或者其他书写格式等都不支持，就作为python3里面还有其他版本号，按照 [heroku官方文档这里](https://devcenter.heroku.com/articles/python-runtimes) 的说法，pypy-1.9或者python-2.4.6或者python-3.3.3等这些都是支持的，但他们只确保python-2.7.10和python-3.4.3能够被很好地支持。【2015-06-09】

### 安装第三方模块<a id="orgheadline44"></a>

这个也很简单，就在 `requirements.txt` 文件里面上写上模块名字即可。 比如：

    flask
    gunicorn

这样heroku远程会自动利用pip来安装对应的第三方模块，如下所示：

    remote:        Installing collected packages: Werkzeug, markupsafe, Jinja2, itsdangerous, flask, gunicorn

我们看到flask依赖的其他第三方模块也安装上去了，然后这样自动安装的就是最新的模块的版本号，而我们不用virtualenv的话，本地也安装的是最新的版本号，这样也能起到检验的效果，而且也更简单。只有某些极个别的情况才会考虑固定模块的版本号，毕竟让模块更新到最近的版本号总是好的。当然了，这是我个人的体验，如果读者确实觉得virtualenv很好用，就当我没说。

### Procfile文件<a id="orgheadline45"></a>

这是主文件夹和heroku配置相关的第三个文件，官方教程的是这样一行：

    web: gunicorn hello:app --log-file=-

我们大致可以猜到这是heroku的初始化配置参数，比如web引擎用gunicorn，然后hello是app主入口等。这个后面再详细讨论。

### hello.py文件<a id="orgheadline46"></a>

这个hello.py对应的就是上面的 `hello:app` 这句，告诉heroku了本app的主入口在哪里。

其内容就是最简单的flask演示hello world例子：

    import os
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello():
        return 'Hello World!'

heroku远程是如何编译源文件的我们不管，然后接下来就是利用git把你写的源码推到heroku服务器上去，推上去之后heroku会自动编译一次源文件。

### 安装heroku或者不？<a id="orgheadline47"></a>

按照官方文档的说明来，不赘述了。在ubuntu下的是：

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

本终端要登录一次，就是运行命令：

    heroku login

登录的目的是什么？是为了有权限在heroku服务器上新建项目，也就是让你能够运行 `heroku create` 命令。但是我们日常用git推送是不需要heroku登录的，而且实际上这个 `heroku create` 命令你也可以不使用，你可以在heroku的网页上新建一个项目。 

### 通过网页新建项目得到一个好名字<a id="orgheadline48"></a>

通过网页我们新建的项目名字可以稍微取得好看点，然后新建完之后我们如何利用git将你的目标源码推送到那里去呢？首先git init ，然后git commit 这些我就不废话了，那么最关键的问题是heroku这个远程目的地地址在那里呢？

十分有趣的是我在网页上找了一下竟然找不到，但这又不是什么秘密。在.git的config里面，一个项目名 `secret-shore-9329` 的url是：

    url = https://git.heroku.com/secret-shore-9329.git

再比如 `myheroku-1` 对应的是：

    git remote add heroku  https://git.heroku.com/myheroku-1.git

然后接下来你就随意进行git管理项目和推送了。

### 或者更简单直接通过github来管理<a id="orgheadline49"></a>

heroku还提供了一种更简单的项目管理方式，你可以在github上创建一个项目，然后在heroku上连接好这个github项目，然后推荐把下面那个 **自动部署** 也点上。然后你就可以如同往常一样 `git push origin master` ，将源码推送到github上即可。这样真是方便极了！

### 如何在heroku上加入数据库<a id="orgheadline50"></a>

    heroku  pg:psql --app cheminfo

    heroku addons:add heroku-postgresql:dev --app cheminfo

## 和apache2一起<a id="orgheadline56"></a>

apache2是支持flask框架编写网络服务器的，更确切的表达是，apache2有一个模块，安装上它apache2就支持wsgi接口了。对于python2，在ubuntu下要安装的是:

```bash
sudo apt-get install  libapache2-mod-wsgi
```

对于python3，要安装的是:

```bash
sudo apt-get install  libapache2-mod-wsgi-py3
```

默认那个 `wsgi` 模块安装之后就激活了，你可以通过 `a2dismod` 来看一下，就是那个 `wsgi` 模块。 这里参考了 [这个网页](http://stackoverflow.com/questions/28019310/running-django-python-3-4-on-mod-wsgi-with-apache2) 。

但是等一等，还有一些东西你需要配置，更多细节请参看flask框架官方文档的 [这里](http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi/) ，下面简单介绍之。

### 简单的apache2配置<a id="orgheadline52"></a>

最简单的配置就是如下所示:

    WSGIScriptAlias / /home/wanze/workspace/cdwanze/cdwanze.wsgi
    
        <Directory /home/wanze/workspace/cdwanze >
        Require all granted
        </Directory>

### Require做了什么<a id="orgheadline53"></a>

其中 `Require all granted` 前面我们已谈到，是对访问权限的控制。参看apache2.2到apache2.4的 [升级文档](http://httpd.apache.org/docs/current/upgrading.html) ，
原2.2配置:

    Order deny,allow
    Deny from all

等同于2.4配置:

    Require all denied

原2.2配置:

    Order allow,deny
    Allow from all

等同于2.4配置:

    Require all granted

原2.2配置:

    Order Deny,Allow
    Deny from all
    Allow from example.org

等同于2.4配置:

    Require host example.org

关于这里Order什么的配置更详细的说明参看 [这个网页](http://www.fwolf.com/blog/post/191) 。其第一行就两种写法，逗号之间不能有空格。

1.  `Order Deny,Allow`

在这个写法后面，后面先写允许谁谁谁访问，然后写禁止谁谁谁访问。如下所示:

    Order Deny,Allow
    Deny from all
    Allow from example.org

这个写法的意思是禁止谁，允许谁，禁止所有只允许example.org访问。

1.  `Order Allow,Deny`

和上面描述类似，除了先写允许再写禁止。

新的apache2.4的 `require` 语法更多细节请参看apache2官方文档的 [这里](http://httpd.apache.org/docs/2.4/howto/access.html) 。接着前面的描述，还有如下表达:

允许某个具体的ip地址:

    Require ip ip.address

在前面有了，全部都允许、全部都禁止、全部都禁止只允许谁。还有如下，全部都允许只禁止谁:
不允许某个ip地址。

    Require all granted
    Require not ip 10.252.46.165

### what.wsgi文件<a id="orgheadline54"></a>

这一行具体设置那个what.wsgi文件在那里，一般就放在flask框架源码第一目录下吧。

    WSGIScriptAlias / /home/wanze/workspace/cdwanze/cdwanze.wsgi

然后这个what.wsig文件主要就是说明flask程序app对象在哪里。

这里首先把python的搜索路径加上，然后从你的flask主app对象所谓的文件中引入app，然后 `as application` 。

    import sys
    sys.path.insert(0, '/home/wanze/workspace/cdwanze')
    
    from cdwanze import app as application

### 更复杂点的配置<a id="orgheadline55"></a>

flask官方文档给出了更复杂点的配置如下所示:

    WSGIDaemonProcess cdwanze user=wanze group=wanze threads=5
    WSGIScriptAlias / /home/wanze/workspace/cdwanze/cdwanze.wsgi
    
        <Directory /home/wanze/workspace/cdwanze >
        WSGIProcessGroup cdwanze
        WSGIApplicationGroup %{GLOBAL}
    
        Require all granted
        </Directory>

具体其似乎和用户权限管理有关，这里的细节我还不太懂，不过大体就是用户和群组的控制吧，然后还有一个线程控制。这里暂时先就这样了。

## 参考资料<a id="orgheadline57"></a>

1.  当然是flask的官方文档。 这里有一个 [中文翻译网页](http://docs.jinkan.org/docs/flask/) ，这个网页左侧写着译者，应该是原创吧。
2.  <https://exploreflask.com/> 这里有一个 [中文翻译网页](http://spacewander.github.io/explore-flask-zh/index.html) ，是原创者。
3.  Flask Web Development; Author:Miguel Grinberg; Version:2014（我并不太喜欢这本书的喜欢用各种flask第三方插件的编码风格，大体翻看了一下，主要是看flask官方文档和官方example。）