RePractise
===

在线阅读： [http://repractise.phodal.com/](http://repractise.phodal.com/)

你是否有过这样的经历：

 1. 创建了一个项目，发现有一些坑，你怎么填都填不好。
 2. 放置了一段时间，你又回来了
 3. 于是你用了一个更好的方法来填了这个坑？

> 无论怎样的Coding，都是不断的Practise。想要有所成果，你需要RePractise——总结和diff change，再Practise。

对于工程而言，一个技术都是不断练习出来的。

不同的人对于练习会有不同的方法，有的练习是没有必要的，它并不会增长我们的技术点；有的练习则会将一万小时缩短为一半，或者更短。

(ps:临时广告区，欢迎关注我的微信公众号——首发哦！搜索：phodal，或者扫描下面的二维码)

![Phodal 微信公众号](https://raw.githubusercontent.com/phodal/github-roam/gh-pages/img/qrcode.jpg)

目录
---------

*   [引言](http://repractise.phodal.com/#引言)
    *   [Re-Practise](http://repractise.phodal.com/#re-practise)
    *   [技术与业务](http://repractise.phodal.com/#技术与业务)
    *   [资讯爆炸](http://repractise.phodal.com/#资讯爆炸)
    *   [Lost](http://repractise.phodal.com/#lost)
*   [前端篇: 前端演进史](http://repractise.phodal.com/#前端篇-前端演进史)
    *   [什么是前端？](http://repractise.phodal.com/#什么是前端)
    *   [前端演进史](http://repractise.phodal.com/#前端演进史)
        *   [数据-模板-样式混合](http://repractise.phodal.com/#数据-模板-样式混合)
        *   [Model-View-Controller](http://repractise.phodal.com/#model-view-controller)
        *   [从桌面版到移动版](http://repractise.phodal.com/#从桌面版到移动版)
        *   [APP与过渡期API](http://repractise.phodal.com/#app与过渡期api)
        *   [过渡期SPA](http://repractise.phodal.com/#过渡期spa)
        *   [Hybird与ViewModel](http://repractise.phodal.com/#hybird与viewmodel)
        *   [一次构建，跨平台运行](http://repractise.phodal.com/#一次构建跨平台运行)
    *   [RePractise](http://repractise.phodal.com/#repractise)
*   [后台与服务篇](http://repractise.phodal.com/#后台与服务篇)
    *   [RESTful与服务化](http://repractise.phodal.com/#restful与服务化)
        *   [设计RESTful API](http://repractise.phodal.com/#设计restful-api)
        *   [资源](http://repractise.phodal.com/#资源)
    *   [微服务](http://repractise.phodal.com/#微服务)
        *   [微内核](http://repractise.phodal.com/#微内核)
    *   [混合微服务](http://repractise.phodal.com/#混合微服务)
    *   [其他](http://repractise.phodal.com/#其他)
*   [前后端篇](http://repractise.phodal.com/#前后端篇)
    *   [前后端分离](http://repractise.phodal.com/#前后端分离)
    *   [单页面应用后台渲染](http://repractise.phodal.com/#单页面应用后台渲染)
        *   [前后台渲染同一模板](http://repractise.phodal.com/#前后台渲染同一模板)
        *   [PreRender方式](http://repractise.phodal.com/#prerender方式)
        *   [React](http://repractise.phodal.com/#react)
*   [从真实世界到前后端](http://repractise.phodal.com/#从真实世界到前后端)
    *   [从真实世界到前后端](http://repractise.phodal.com/#从真实世界到前后端-1)
        *   [便利店与售货员](http://repractise.phodal.com/#便利店与售货员)
        *   [模型、领域、抽象](http://repractise.phodal.com/#模型领域抽象)
    *   [前后台分离：后台](http://repractise.phodal.com/#前后台分离后台)
    *   [前后台分离：前端](http://repractise.phodal.com/#前后台分离前端)
    *   [RePractise](http://repractise.phodal.com/#repractise-1)
*   [重构篇](http://repractise.phodal.com/#重构篇)
    *   [网站重构](http://repractise.phodal.com/#网站重构)
        *   [网站重构目的](http://repractise.phodal.com/#网站重构目的)
    *   [代码重构](http://repractise.phodal.com/#代码重构)
    *   [使用工具重构](http://repractise.phodal.com/#使用工具重构)
    *   [借助工具重构](http://repractise.phodal.com/#借助工具重构)
        *   [Code Climate](http://repractise.phodal.com/#code-climate)
    *   [测试驱动开发](http://repractise.phodal.com/#测试驱动开发)
        *   [一次测试驱动开发的故事](http://repractise.phodal.com/#一次测试驱动开发的故事)
        *   [说说测试驱动开发](http://repractise.phodal.com/#说说测试驱动开发)
        *   [思考](http://repractise.phodal.com/#思考)
*   [架构篇: CMS的重构与演进](http://repractise.phodal.com/#架构篇-cms的重构与演进)
    *   [动态CMS](http://repractise.phodal.com/#动态cms)
        *   [CMS简介](http://repractise.phodal.com/#cms简介)
        *   [CMS架构与Django](http://repractise.phodal.com/#cms架构与django)
        *   [编辑-发布分离](http://repractise.phodal.com/#编辑-发布分离)
        *   [基于Github的编辑-发布-开发分离](http://repractise.phodal.com/#基于github的编辑-发布-开发分离)
        *   [Repractise](http://repractise.phodal.com/#repractise-2)
    *   [构建基于Git为数据中心的CMS](http://repractise.phodal.com/#构建基于git为数据中心的cms)
        *   [用户场景](http://repractise.phodal.com/#用户场景)
    *   [Code: 生成静态页面](http://repractise.phodal.com/#code-生成静态页面)
    *   [Builder: 构建生成工具](http://repractise.phodal.com/#builder-构建生成工具)
    *   [Content：JSON格式](http://repractise.phodal.com/#contentjson格式)
        *   [从Schema到数据库](http://repractise.phodal.com/#从schema到数据库)
        *   [git作为NoSQL数据库](http://repractise.phodal.com/#git作为nosql数据库)
    *   [一键发布：编辑器](http://repractise.phodal.com/#一键发布编辑器)
    *   [移动应用](http://repractise.phodal.com/#移动应用)
        *   [小结](http://repractise.phodal.com/#小结)
        *   [其他](http://repractise.phodal.com/#其他-2)
*   [无栈篇：架构设计](http://repractise.phodal.com/#无栈篇架构设计)
    *   [博客与技术驱动](http://repractise.phodal.com/#博客与技术驱动)
        *   [技术组成](http://repractise.phodal.com/#技术组成)
    *   [Lan与架构设计](http://repractise.phodal.com/#lan与架构设计)
        *   [物联网层级结构](http://repractise.phodal.com/#物联网层级结构)
        *   [分层架构](http://repractise.phodal.com/#分层架构)
        *   [六边形架构](http://repractise.phodal.com/#六边形架构)
*   [模式篇：设计与架构](http://repractise.phodal.com/#模式篇设计与架构)
    *   [观察者模式](http://repractise.phodal.com/#观察者模式)
        *   [Ruby观察者模式](http://repractise.phodal.com/#ruby观察者模式)
        *   [PUB/SUB](http://repractise.phodal.com/#pubsub)
    *   [模板方法](http://repractise.phodal.com/#模板方法)
        *   [从基本的App说起](http://repractise.phodal.com/#从基本的app说起)
        *   [Template Method](http://repractise.phodal.com/#template-method)
        *   [Template Method实战](http://repractise.phodal.com/#template-method实战)
    *   [Pipe and Filters](http://repractise.phodal.com/#pipe-and-filters)
        *   [Unix Shell](http://repractise.phodal.com/#unix-shell)
        *   [Pipe and Filter模式](http://repractise.phodal.com/#pipe-and-filter模式)
        *   [Fluent API](http://repractise.phodal.com/#fluent-api)
        *   [DSL 表达式生成器](http://repractise.phodal.com/#dsl-表达式生成器)
        *   [Pipe and Filter模式实战](http://repractise.phodal.com/#pipe-and-filter模式实战)
*   [数据与模型篇](http://repractise.phodal.com/#数据与模型篇)
    *   [数据](http://repractise.phodal.com/#数据)
        *   [数据库](http://repractise.phodal.com/#数据库)
        *   [数据模型](http://repractise.phodal.com/#数据模型)
*   [领域篇](http://repractise.phodal.com/#领域篇)
    *   [DDD](http://repractise.phodal.com/#ddd)
    *   [DSL](http://repractise.phodal.com/#dsl)
        *   [DSL示例](http://repractise.phodal.com/#dsl示例)

License
---


[![Phodal's Book](http://brand.phodal.com/shields/book-small.svg)](https://www.phodal.com/)    [![Phodal's Idea](http://brand.phodal.com/shields/idea-small.svg)](http://ideas.phodal.com/)

© 2015~2016 [Phodal Huang](https://www.phodal.com). This code is distributed under the Creative Commons Attribution-Noncommercial-No Derivative Works 3.0  License. See `LICENSE` in this directory.

[待我代码编成，娶你为妻可好](http://www.xuntayizhan.com/blog/ji-ke-ai-qing-zhi-er-shi-dai-wo-dai-ma-bian-cheng-qu-ni-wei-qi-ke-hao-wan/)
