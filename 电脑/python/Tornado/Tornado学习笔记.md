<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline5">1. 前言</a>
<ul>
<li><a href="#orgheadline1">1.1. 安装</a></li>
<li><a href="#orgheadline2">1.2. 第一个例子</a></li>
<li><a href="#orgheadline3">1.3. 一个简单的异步例子</a></li>
<li><a href="#orgheadline4">1.4. tornado.gen.coroutine vs asyncio.coroutine</a></li>
</ul>
</li>
<li><a href="#orgheadline9">2. Application对象</a>
<ul>
<li><a href="#orgheadline6">2.1. url分发部分</a></li>
<li><a href="#orgheadline7">2.2. 配置部分</a></li>
<li><a href="#orgheadline8">2.3. listen方法</a></li>
</ul>
</li>
<li><a href="#orgheadline10">3. RequestHandler对象</a></li>
<li><a href="#orgheadline11">4. 单元测试</a></li>
<li><a href="#orgheadline12">5. tornado异常输出完全json风格化</a></li>
<li><a href="#orgheadline15">6. 附录</a>
<ul>
<li><a href="#orgheadline13">6.1. tornado plus flask</a></li>
<li><a href="#orgheadline14">6.2. 参考资料</a></li>
</ul>
</li>
</ul>
</div>
</nav>


# 前言<a id="orgheadline5"></a>

## 安装<a id="orgheadline1"></a>

```sh
pip3 install tornado
```

## 第一个例子<a id="orgheadline2"></a>

```python
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")

def make_app():
    return tornado.web.Application([
        (r"/", MainHandler),
    ])

if __name__ == "__main__":
    app = make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```

这是一个同步的webapp框架，可能后面会用的较少，这里主要了解一下tornado编写的webapp的基本结构:

1.  开启tornado的事件驱动循环:

    tornado.ioloop.IOLoop.current().start()

这个大体类似于asyncio的:

    asyncio.get_event_loop().run_forever()

所以后面我们会谈论到将tornado的事件驱动挂载到asyncio的主事件驱动循环上，这样tornado webapp的事件处理部分就变成了:

    from tornado.platform.asyncio import AsyncIOMainLoop
    if __name__ == '__main__':
        AsyncIOMainLoop().install()
        app = make_app()
        app.listen(9999)
    
        asyncio.get_event_loop().run_forever()

这里的 `AsyncioIOMainLoop().install()` 的用途就是上面提及的将tornado的事件循环挂载到asyncio的主事件驱动循环上。

1.  make\_app:

我们需要创建一个Application对象，这个Application对象后面再详细讨论:

    def make_app():
        return tornado.web.Application([
            (r"/", MainHandler),
        ])

然后从上面的代码我们就已经看到，根据不同的URL将分发不同的Handler，这个类似于flask的URL和视图函数分发过程。所以我们也可以把这里的 MainHandler 看作一个视图处理类。

1.  编写各个视图处理类:

    class MainHandler(tornado.web.RequestHandler):
        def get(self):
            self.write("Hello, world")

视图处理类里面很多细节后面再详细讨论之，我们从上面的例子可以看到大体分发了HTTP `get` 请求，然后write了什么信息。

## 一个简单的异步例子<a id="orgheadline3"></a>

```python
import asyncio
from tornado.platform.asyncio import AsyncIOMainLoop
import tornado.web
import aiohttp
from expython.web.coroutine import aiowget_images


class MainHandler(tornado.web.RequestHandler):
    @asyncio.coroutine
    def get(self):
        res = yield from aiowget_images('http://tu.duowan.com/m/meinv')
        for img in res:
            self.write("&lt;img src={} /&gt;".format(img))

def make_app():
    return tornado.web.Application([
        (r"/", MainHandler),
    ],debug=True)



if __name__ == '__main__':
    AsyncIOMainLoop().install()
    app = make_app()
    app.listen(9999)

    asyncio.get_event_loop().run_forever()
```

这个例子将tornado的事件循环和asyncio的事件循环合并了，然后我们看到程序结构大体和同步风格没有太大差异，除了视图处理类里面的某些函数变成了协程函数，从而其可以不阻塞的接受多个网络对应的方法的请求了。上面的aiowget\_images是我写的一个异步的抓取某个网页的图片链接函数，读者有兴趣可以了解下。

## tornado.gen.coroutine vs asyncio.coroutine<a id="orgheadline4"></a>

请参看官方文档的 [这里](http://www.tornadoweb.org/en/stable/guide/coroutines.html) ，实际上我试了一下，将 `@gen.coroutine` 替换为 `@asyncio.coroutine` webapp也是可以正常运行的。

正如官方文档所说，这里主要是兼容性的考虑，Tornado的coroutine runner更通用，可以适应其他框架，而asyncio的coroutine runner并不接受其他框架的corotine。一般还是推荐使用tornado提供的装饰器 `@gen.coroutine` 吧。

# Application对象<a id="orgheadline9"></a>

    tornado.web.Application(handlers=None, default_host='', transforms=None, **settings)

## url分发部分<a id="orgheadline6"></a>

上面的handlers参数主要进行url分发工作，其是一个列表，里面是一些所谓的 `URLSpec` 对象:

    tornado.web.URLSpec(pattern, handler, kwargs=None, name=None)

-   pattern就是一个匹配url分发的正则表达式
-   handler是 `RequestHandler` 的子类，定义了具体url分发过来之后做些什么。
-   kwargs，字典值，这个值将传递给handler的 `initialize` 方法，这个后面再说。
-   name，确切来说是给这个url分发规则取个名字，等下可以用 `Application.reverse_url(name,*args)` 来解析出具体的某个url，这个大体类似于flask的 url\_for 和 endpoint的概念。

然后pattern正则表达式我们知道有那个圆括号包围起来的group的概念，比如:

    r"/story/([0-9]+)"

这里group匹配到的参数，将作为入口参数传递个 `RequestHandler` 对象的 HTTP method，也就是 `get` `post` 等。

然后我们看到 `Application.reverse_url(name,*args)` 其后接受的一些参数也对应这里的正则表达式匹配，其反向解析url将进行匹配子group的替换操作。

然后kwargs这个字典值，传递给 `initialize` 方法大致如下所示:

```python
class StoryHandler(RequestHandler):
    def initialize(self, db):
        self.db = db

    def get(self, story_id):
        self.write("this is story %s" % story_id)

app = Application([
    url(r"/story/([0-9]+)", StoryHandler, dict(db=db), name="story")
    ])
```

## 配置部分<a id="orgheadline7"></a>

`**settings` 收集一系列的有关Application的配置信息，具体有很多，不一而足，下面列出一些:

-   **debug:** 是否开启debug模式
-   **template\_path:** 定义模板文件夹所在位置
-   **static\_path:** 定义静态文件夹所在位置

## listen方法<a id="orgheadline8"></a>

为这个application开启一个HTTP Server，然后指定监听端口。

# RequestHandler对象<a id="orgheadline10"></a>

每一个请求过来都将创建一个 `RequestHandler` 对象，然后其将执行 `initialize` 方法；然后其将执行 `prepare` 方法，prepare方法是HTTP协议具体方法无关的；然后其将执行具体某个HTTP协议的方法，比如 `get` `post` `put` 等等，url正则表达式匹配的子group也将作为参数传进去，这个前面有所提及的；然后其将执行 `on_finish` 方法。

# 单元测试<a id="orgheadline11"></a>

tornado的单元测试样例如下:

这是 `api.py` 文件:

```python
import tornado
import logging
from tornado.web import RequestHandler
import time


class AnalyticsBWSpecificHour(RequestHandler):
    def get(self):
        return self.write({'message':'no get method'})


class Application(tornado.web.Application):
    def __init__(self,**kwargs):
        api_handlers = [
            (r"/", AnalyticsBWSpecificHour),
        ]

        logging.debug(api_handlers)

        super(Application, self).__init__(api_handlers, **kwargs)
```

这是 `test_api.py` 文件:

```python
from api import Application

from tornado.testing import AsyncHTTPTestCase
import tornado
import logging
logging.basicConfig(level=logging.DEBUG)
import unittest

class ApiTestCase(AsyncHTTPTestCase):
   def get_app(self):
        self.app = Application()
        return self.app

    def test_status(self):
        response = self.fetch('/',method='GET')
        self.assertEqual(response.code,200)

if __name__ == '__main__':
    unittest.main()
```

请参看 [这个网页](http://stackoverflow.com/questions/36928232/how-to-do-unit-test-on-tornado) ，这里讲到 `self.fetch` 方法已经默认会进行 `self.get_url` 操作了。

# tornado异常输出完全json风格化<a id="orgheadline12"></a>

tornado app 如果发生异常了，其会调用对应的 RequestHandler 的 `write_error` 方法，你可以自定义这个方法，从而使其返回 json 格式的信息。更多信息请参看 [这个网页](http://nanvel.name/2014/12/handle-errors-in-tornado-application) 。

# 附录<a id="orgheadline15"></a>

## tornado plus flask<a id="orgheadline13"></a>

主要参考了 [这个网站](http://stackoverflow.com/questions/8143141/using-flask-and-tornado-together/8247457#8247457) 。

```python
from tornado.wsgi import WSGIContainer
from tornado.ioloop import IOLoop
from tornado.web import FallbackHandler, RequestHandler, Application
from flasky import app

class MainHandler(RequestHandler):
  def get(self):
    self.write("This message comes from Tornado ^_^")

tr = WSGIContainer(app)

application = Application([
(r"/tornado", MainHandler),
(r".*", FallbackHandler, dict(fallback=tr)),
])

if __name__ == "__main__":
  application.listen(8000)
  IOLoop.instance().start()
```

## 参考资料<a id="orgheadline14"></a>

1.  [官方文档](http://www.tornadoweb.org/en/stable/index.html) ，其他的中文翻译版本可能有点过时了。