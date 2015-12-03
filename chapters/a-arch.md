#全栈篇： 架构设计

##博客

我尚不属于那些技术特别好的人——我只是广度特别广，从拿电烙铁到所谓的大数据。不过相比于所谓的大数据，我想我更擅长于焊电路板，笑~~。由于并非毕业于计算机专业，毕业前的实习过程中，我发现在某些特殊领域的技术比不上科班毕业的人，这意味着需要更多的学习。但是后来受益于工作近两年来从没有加班过，朝九晚六的生活带来了大量的学习时间。在这个漫长的追赶过程中，我发现开发博客相关的应用带来了很大的进步。

##现在，我的博客是如何工作的？

###HTTP服务器

当你开发在网页上访问我的博客的时候，你可能会注意到上面的协议是HTTPS。

![blog-mobile][1]

但是并不会察觉到它是HTTP2.0。而这需要一个可以支持HTTP2.0的HTTP服务器，在不改变现在程序配置的情况下，你需要重新编译你的HTTP服务器。在这里，我的博客用的是Nginx，所以它在还只是试验版的时候，就已经被编译进去了。为了隐藏服务器的版本，还需要在编译的时候做了些手脚。除此，为了浏览器上的那个小绿锁，我们还需要一个HTTPS证书，并在Nginx上配置它。

在这时，我们还需要配置一个缓存服务器。过去，我在上面用过Varinsh、Nginx Cache。尽管对于个人博客来说，可能意义不是很大，但是总需要去尝试。于是用到了ngx_pagespeed，它会帮我们做很多事，从合并CSS、JS，到转图片转为webp格式等等。

Nginx对请求转发给了某个端口，就来到了WSGI。

###WSGI

接着，我们就来到了Web服务器网关接口——是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。现在，你或许已经知道了这个博客是基于Python语言的框架。但是在我们揭晓这个答案之前，我们还需要介绍个小工具——New Relic。如果你在Chrome浏览器上使用Ghosty插件，你就会看到下面的东西。

![New Relic][2]

New Relic是一个网站监测工具，Google Analytics是一个分析工具。但是，不一样的是New Relic需要在我们启动的时候加进去：

```
nohup /PATH/bin/newrelic-admin run-program /PATH/bin/gunicorn --workers=2 MK_dream.wsgi -b 0.0.0.0:8080 --timeout=300& 
```

现在这个请求总算来到了8080端口，接着到了Gunicorn的世界里，它是一个高效的Python WSGI Server。

过了上面几步这个请求终于交给了Django。

###Django

Django这个天生带Admin的Web框架，就是适合CMS和博客。这并不意味着它的工作范围只限于此，它还有这么多用户:

![Who Use Django][3]

请求先到了Django的URL层，这个请求接着交给了View层来处理，View层访问Model层以后，与Template层一起渲染出了HTML。Django是一个MTV框架(类似于MVC之于Spring MVC)。接着，HTML先给了浏览器，浏览器继续去请求前端的内容。

它也可以用Farbic部署哦~~。

###Angluar & Material Design Lite vs Bootstrap & jQuery Mobile

这是一个现代浏览器的前端战争。最开始，博客的前端是Bootstrap框架主导的UI，而移动端是jQuery Mobile做的(PS: Mezzanine框架原先的结构)。

接着，在我遇到了Backbone后，响应了下Martin Folwer的**编辑-发布分离模式**。用Node.js与RESTify直接读取博客的数据库做了一个REST API。Backbone就负责了相应的Detail页和List页的处理。

尽管这样做的方式可以让用户访问的速度更快，但是我相信没有一个用户会一次性的把技术博客看完。而且我博客流量的主要来源是Google和百度。

然后，我试着用Angular去写一些比较特殊的页面，如[全部文章](https://www.phodal.com/all/)。但是重写的过程并不是很顺畅，这意味着我需要重新考虑页面的渲染方式。

最后，出现了Material Design Lite，也就是现在这个丑丑的页面，还不兼容新IE（微信浏览器）。

作为一个技术博客，它也用到了HighLight.js的语法加亮。

###API

在构建SPA的时候，做了一些API，然后就有了一个Auto Sugget的功能：

![Auto Suggest][4]

或者说，它是一个Auto Complete，可以直接借助于jQuery AutoComplete插件。

或许你已经猜到了，既然我们已经有博客详情页和列表页的API，并且我们也已经有了Auto Suggestion API。那么，我们就可以有一个APP了。

###APP

偶然间发现了Ionic框架，它等于 = Angluar + Cordova。于是，在测试Google Indexing的时候，花了一个晚上做了博客的APP。

![Blog App][5]

我们可以在上面做搜索，搜索的时候也会有Auto Suggestion。上面的注销意味着它有登录功能，而Hybird App的登录通常可以借用于JSON Web Token。即在第一次登录的时候生成一个Token，之后的请求，如发博客、创建事件，都可以用这个Token来进行，直到Token过期。如果你是第一次在手机上访问，也许你会遇到这个没有节操的广告：

![Install Phodal Blog App][6]

然并卵，作为我的第七个Hybird应用，它只发布在Google Play上——因为不需要审核。

随后，我意识到了我需要将我的博客推送给读者，但是需要一个渠道。

###微信公众平台

借助于Wechat-Python-SDK，花了一个下午做了一个基础的公众平台。除了可以查询最新的博客和搜索，它的主要作用就是让我发我的博客了。

![Phodal QRCode][7]

对了，如果你用Python写代码，可以试试PyCharm。除了WebStorm以外，我最喜欢的IDE。因为WebStorm一直在与时俱进。

###微信编辑器

作为下一个项目，我开始打算去做一个微信编辑器。一方面可以给我的女朋友用，她正在我们公司实习——新媒体运营。她写了之前的《[极客爱情](http://www.ituring.com.cn/book/1665)》系列的文章，或许你对实验室约会吧、我真的不是修电脑的、极客的神逻辑、技术宅不解风情等等的文章。如果可以的话，也可以给个相关关注哈哈~~，打个小小广告，下面是它的微信公众号——白米粥。

![Baimizhou][8]

极客爱情是一段热恋的故事，白米粥更像是平淡生活的故事。

扯远了，这个编辑器主要是基于CKEDITOR，因为微信默认的是UEDITOR。尽管UEditor开源，但是已经很长时间没有维护了。这样的项目是被政治因素，以及加班的工程师文化的影响吧？？？

##技术组成

So，在这个博客里会有三个用户来源，Web > 公众号 > App。

在网页上，每天大概会400个PV，其中大部分是来自Google、百度，接着就是偶尔推送的公众号，最后就是只有我一个人用的APP。。。

> Web架构

服务器：

1. Nginx(含Nginx HTTP 2.0、PageSpeed 插件)
2. Gunicorn(2 Workers) 
3. New Relic(性能监测)

DevOps: 

1. Farbic（自动部署）

Web应用后台： 

1. Mezzaine（基于Django的CMS）
2. REST Framework (API) 
3. REST Framework JWT (JSON Web Token) 
4. Wechat Python SDK
5. Mezzanine Pagedown （Markdown

Web应用前台: 

1. Material Design Lite (用户) 
2. BootStrap (后台) 
3. jQuery + jQuery.autocomplete + jquery.githubRepoWidget
4. HighLight.js 
5. Angluar.js
6. Backbone (已不维护)

移动端: 

1. Ionic
2. Angular + ngCordova
3. Cordova
4. highlightjs
5. showdown.js(Markdown Render) 
6. Angular Messages + Angular-elastic

微信端: 

1. Wechat-Python-SDK

That's All...

##前后台分离

##服务


  [1]: http://repractise.phodal.com/img/a-arch/blog-mobile.jpg
  [2]: http://repractise.phodal.com/img/a-arch/ga-newrelic.png
  [3]: http://repractise.phodal.com/img/a-arch/who-use-django.jpg
  [4]: http://repractise.phodal.com/img/a-arch/autosuggest.jpg
  [5]: http://repractise.phodal.com/img/a-arch/blog-app.jpg
  [6]: http://repractise.phodal.com/img/a-arch/install-app.jpg
  [7]: http://repractise.phodal.com/img/a-arch/phodal-qrcode.jpg

