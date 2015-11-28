#架构篇一: CMS的重构与演进

重构系统是一项非常具有挑战性的事情。通常来说，在我们的系统是第二个系统的时候才需要重构，即这个系统本身已经很臃肿。我们花费了太量的时间在代码间的逻辑，开发新的功能变得越来越慢。这不仅仅可能只是因为我们之前的架构没有设计好，而且在我们开发的过程中没有保持着原先设计时的一些原则。如果是这样的情况，那么这就是一个复杂的过程。

还有一种情况是我们发现了一种更符合我们当前业务的框架。

##动态CMS

###CMS简介

CMS是Content Management System的缩写，意为"内容管理系统".它可以做很多的事情，但是总的来说就是Page和Blog——即我们要创建一些页面可以用于写一些About US、Contact Me，以及持续更新的博客或者新闻，以及其他子系统——通常更新不活跃。通过对这些博客或者新闻进行分类，我们就可以有不同的信息内容，如下图：

![不同分类的内容](http://repractise.phodal.com/img/cms/cms-blogs.png)

CMS是政府和企业都需要的系统，他们有很多的信息需要公开，并且需要对其组织进行宣传。在我有限的CMS交付经验里（大学时期），一般第一次交付CMS的时候，已经创建了大部分页面。有时候这些页面可能直接存储在数据库中，后来发现这不是一个好的方案，于是很多页面变成了静态页面。随后，在CMS的生命周期里就是更新内容。

因而，CMS中起其主导的东西还是Content，即内容。而内容是一些持续可变的东西。这也就是为什么WordPress这么流行于CMS界，它是一个博客系统，但是多数时候我们只需要更新内容。除此不得不提及的一个CMS框架是Drupal，两者一对比会发现Drupal比较强大。通常来说，强大的一个负作用就是——复杂。

WordPress和Drupal这一类的系统都属于发布系统，而其后台可以称为编辑系统。

一般来说CMS有下面的特点：
 
 - 支持多用户。
 - 角色控制-内容管理。如InfoQ的编辑后台就会有这样的机制，社区编辑负责创建内容，而审核发布则是另外的人做的。
 - 插件管理。如WordPress和Drupal在这一方面就很强大，基本可以满足日常的需要。
 - 快捷简便地存储内容。简单地来说就是所见即所得编辑器，但是对于开发者来说，Markdown似乎是好的选择。
 - 预发布。这是一个很重要的特性，特别是如果你的系统后台没有相对应的预览机制。
 - 子系统。由于这属于定制化的系统，并不方便进行总结。
 - ...

CMS一直就是这样一个紧耦合的系统。

###CMS架构与Django

说起来，我一直是一个CMS党。主要原因还在于我可以随心所欲地去修改网站的内容，修改网站的架构。好的CMS总的来说都有其架构图，下图似乎是Drupal的模块图

![Drupal 框架](http://repractise.phodal.com/img/cms/drupal-modular.png)

一般来说，其底层都会有：

 - ORM
 - User Management
 - I18n / L10n
 - Templates

我一直在使用一个名为Django的Python Web框架，它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的，即是CMS（内容管理系统）软件。它是一个MTV框架——与多数的框架并没有太大的区别。


层次                     | 职责
------------------------|---------------
模型（Model），即数据存取层 | 处理与数据相关的所有事务：如何存取、如何验证有效性、包含哪些行为以及数据之间的关系等。
模板(Template)，即表现层   | 处理与表现相关的决定： 如何在页面或其他类型文档中进行显示。
视图（View），即业务逻辑层   | 存取模型及调取恰当模板的相关逻辑。模型与模板之间的桥梁。

从框架本身来上看它和别的系统没有太大的区别。

![Django Architecture](http://repractise.phodal.com/img/cms/django-architecture.jpg)

但是如果我们已经有多外模块（即Django中app的概念），那么系统的架构就有所不同了。

![Django App架构 ](http://repractise.phodal.com/img/cms/django-apps.jpg)

这就是为何我喜欢用这个CMS的原因了，我的每个子系统都以APP的形式提供服务——博客是一个app，sitemap是一个app，api是一个app。系统直接解耦为类似于混合服务的架构，即不像微服务一样多语言化，又不会有宏应用的紧耦合问题。

##编辑-发布分离

我们的编辑和发布系统在某种意义上紧耦合在一起了，当用户访问量特别大的时候，这样会让我们的应用变得特定慢。有时候编辑甚至发布不了新的东西，如下图引示:

![发布-编辑](http://repractise.phodal.com/img/cms/editor-publisher.png)

或者你认识出了上图是源自Martin Folwer的[编辑-发布分离](http://martinfowler.com/bliki/EditingPublishingSeparation.html)

编辑-发布分离是几年前解耦复杂系统游来开来带来的一个成果。今天这个似乎已经很常见了，编辑的时候是在后台进行的，等到发布的时候已经变成了一个静态的HTML。

已经有足够多的CMS支持这样的特性，运行起来似乎特别不错，当然这样的系统也会有缓存的问题。有了APP这后，这个趋势就更加明显了——人们需要提供一个API。到底是在现有的系统里提供一个新的API，还是创建一个新的API。

这时候，我更愿意选择后者——毕竟紧耦合一个系统总会在后期带来足够多的麻烦。而且基于数据库构建一个只读的RESTful API并不是一个复杂的过程，而且也危险。这时候的瓶颈就是数据库，但是似乎数据库都是多数系统的瓶颈。人们想出了各种各样的技术来解决这个瓶颈。

于是之前我试着用Node.js + RESTify将我的博客重构成了一个SPA，当然这个时候CMS还在运行着。出于SEO的原因我并没有在最后采用这个方案，因为[我网站](https://www.phodal.com)的主要流量来源是Google和是百度。但是我在另外的网站里混合了SPA与MPA，其中的性能与应用是相当的，除了第一次加载页面的时候会带来一些延时。

除了Node.js + RESTify，也试了试Python + Falcon（一个高性能的RESTful框架）。这个API理论上也应该可以给APP直接使用，并且可以直接拿来生成静态页面。

##编辑-发布-开发分离：静态站点生成

如React一样解决DOM性能的问题就是跳过DOM这个坑，要跳过动态网站的性能问题就是让网站变成静态。

越来越多的开发人员开始在使用Github Pages作为他们的博客，这是一个很有意思的转变。主要的原因是这是免费的，并且基本上可以保证24x7小时是可用的——当且仅当Github发现故障的时候才会不可访问。

在这一类静态站点生成器(Github)里面，比较流行的有下面的内容（数据来源： [http://segmentfault.com/a/1190000002476681](http://segmentfault.com/a/1190000002476681)）:

1. Jekyll / OctoPress。Jekyll和OctoPress是最流行的静态博客系统。
2. Hexo。Hexo是NodeJS编写的静态博客系统，其生成速度快，主题数量相对也比较丰富。是OctoPress的优秀替代者。
3. Sculpin。Sculpin是PHP的静态站点系统。Hexo和Octopress专注于博客，而有时候我们的需求不仅仅是博客，而是有类似CMS的页面生成需求。Sculpin是一个泛用途的静态站点生成系统，在支持博客常见的分页、分类tag等同时，也能较好地支持非博客的一般页面生成。
4. Hugo。Hugo是GO语言编写的静态站点系统。其生成速度快，且在较好支持博客和非博客内容的同时提供了比较完备的主题系统。无论是自己写主题还是套用别人的主题都比较顺手。

通常这一类的工具里会有下面的内容：

1. 模板
2. 支持Markdown
3. 元数据

如Hexo这样的框架甚至提供了``一键部署``的功能。

在我们写了相关的代码之后，随后要做的就是生成HTML。对于个人博客来说，这是一个非常不错的系统，但是对于一些企业级的系统来说，我们的要求就更高了。如下图是Carrot采用的架构：

![Editor Develoepr](http://repractise.phodal.com/img/cms/carrot.png)

这与我们在项目上的系统架构目前相似。作为一个博主，通常来说我们修改博客的主题的频率会比较低， 可能是半年一次。如果你经常修改博客的主题，你博客上的文章一定是相当的少。

上图中的编辑者通过一个名为Contentful CMS来创建他们的内容，接着生成RESTful API。而类似的事情，我们也可以用Wordpress + RESTful 插件来完成。如果做得好，那么我想这个API也可以直接给APP使用。

上图中的开发者需要不断地将修改的主题或者类似的东西PUSH到版本管理系统上，接着会有webhook监测到他们的变化，然后编译出新的静态页面。

最后通过Netlify，他们编译到了一起，然后部署到生产环境。除了Netlify，你也可以编写生成脚本，然后用Bamboo、Go这类的CI工具进行编译。

通常来说，生产环境可以使用CDN，如CloudFront服务。与动态网站相比，静态网站很容易直接部署到CDN，并可以直接从离用户近的本地缓存提供服务。除此，直接使用AWS S3的静态网站托管也是一个非常不错的选择。

###基于Github的编辑-发布-开发分离

尽管我们已经在项目上实施了基于Github的部分内容管理已经有些日子里，但是由于找不到一些相关的资料，便不好透露相关的细节。直到我看到了《[An Incremental Approach to Content Management Using Git 1](https://www.thoughtworks.com/insights/blog/incremental-approach-content-management-using-git)》，我才意识到这似乎已经是一个成熟的技术了。看样子这项技术首先已经应用到了ThoughtWorks的官网上了。

文中提到了使用这种架构的几个点：

1. 快速地开始项目，而不是学习或者配置框架。
2. 需要使用我们信奉的原则，如TDD。而这是大部分CMS所不支持的。
3. 基于服务的架构。
4. 灵活的语言和工具
5. 我们是开发人员。

So，so，这些开发人员做了些什么：

1. 内容存储为静态文件
2. 不是所有的内容都是平等的
3. 引入内容服务
4. 使用Github。所有的content会提交到一个repo里，同时在我们push内容的时候，可以实时更新这些内容。
5. 允许内容通过内容服务更新
6. 使用Github API

于是，有了一个名为[Hacienda](https://github.com/haciendaio/hacienda)的框架用于管理内容，并存储为JSON。这意味着什么？

![基于Github的编辑-发布-开发分离](http://repractise.phodal.com/img/cms/github-edit-publish-code.png)

因为使用了Git，我们可以了解到一个文件内容的历史版本，相比于WordPress来说更直观，而且更容易 上手。

开发人员修改完他们的代码后，就可以直接提交，不会影响到Editor使用网站。Editor通过一个编辑器添加内容，在保存后，内容以JSON的形式出现直接提交代码到Github上相应的代码库中。CI或者Builder监测到他们的办法，就会生成新的静态页面。在这时候，我们可以选择有一个预览的平台，并且可以一键部署。那么，事情似乎就完成得差不多了。

如果我们有APP，那么我们就可以使用Content Servies来做这些事情。甚至可以直接拿其搭建一个SPA。

如果我们需要全文搜索功能，也变得很简单。我们已经不需要直接和数据库交互，我们可以直接读取JSON并且构建索引。这时候需要一个简单的Web服务，而且这个服务还是只读的。

在需要的时候，如手机APP，我们可以通过Content Servies来创建博客。

##Repractise

 > 动态网页是下一个要解决的难题。我们从数据库中读取数据，再用动态去渲染出一个静态页面，并且缓存服务器来缓存这个页面。既然我们都可以用Varnish、Squid这样的软件来缓存页面——表明它们可以是静态的，为什么不考虑直接使用静态网页呢？
 
思考完这些后，我想到了一个符合学习的场景。

![基于Travis CI的编辑-发布-开发分离](http://repractise.phodal.com/img/cms/travis-edit-publish-code.png)

我们构建的核心都可以基于Travis CI来完成，唯一存在风险的环节是我们似乎需要暴露我们的Key。

##其他

参考文章:

1. [静态网站生成器将会成为下一个大热门](http://www.infoq.com/cn/news/2015/11/LAMP-CDN)
2. [EditingPublishingSeparation](http://martinfowler.com/bliki/EditingPublishingSeparation.html)
3. [An Incremental Approach to Content Management Using Git 1](https://www.thoughtworks.com/insights/blog/incremental-approach-content-management-using-git)
4. [Part 2: Implementing Content Management and Publication Using Git](https://www.thoughtworks.com/insights/blog/implementing-content-management-and-publication-using-git)





