#重构篇

##网站重构

与上述相似的是:在不改变外部行为的前提下，简化结构、添加可读性，而在网站前端保持一致的行为。也就是说是在不改变UI的情况下，对网站进行优化，在扩展的同时保持一致的UI。

###基础网站重构

过去人们所说的``网站重构``

> 把"未采用CSS，大量使用HTML进行定位、布局，或者虽然已经采用CSS，但是未遵循HTML结构化标准的站点"变成"让标记回归标记的原本意义。通过在HTML文档中使用结构化的标记以及用CSS控制页面表现，使页面的实际内容与它们呈现的格式相分离的站点。"的过程就是网站重构(Website Reconstruction)

依照我做过的一些案例，对于传统的网站来说重构通常是
 
 - 表格(table)布局改为DIV+CSS
 - 使网站前端兼容于现代浏览器(针对于不合规范的CSS、如对IE6有效的)
 - 对于移动平台的优化
 - 针对于SEO进行优化

###高级网站重构
过去的网站重构

> 所谓的网站重构就是“DIV+CSS”，想法固然极度局限。但也不是另一部分的人认为是“XHTML+CSS”，因为“XHTML+CSS”只是页面重构。

而真正的网站重构

> 应包含结构、行为、表现三层次的分离以及优化，行内分工优化，以及以技术与数据、人文为主导的交互优化等。

深层次的网站重构应该考虑的方面

 - 减少代码间的耦合
 - 让代码保持弹性
 - 严格按规范编写代码
 - 设计可扩展的API
 - 代替旧有的框架、语言(如VB)
 - 增强用户体验

通常来说对于速度的优化也包含在重构中

 - 压缩JS、CSS、image等前端资源(通常是由服务器来解决)
 - 程序的性能优化(如数据读写)
 - 采用CDN来加速资源加载
 - 对于JS DOM的优化
 - HTTP服务器的文件缓存

可以应用的的方面
 
 - [使用Ngx_pagespeed优化前端](http://www.phodal.com/blog/nginx-with-ngx-pagespeed-module-improve-website-cache/)
 - 解耦复杂的模块
 - 对缓存进行优化
 - 针对于内容创建或预留API
 - 需要添加新API，如(weChat等的支持)
 - 用新的语言、框架代码旧的框架(如VB.NET，C#.NET)

###网站重构目的

希望自己的网站

 - 成本变得更低
 - 运行得更好
 - 访问者更多
 - 维护愈加简单
 - 功能更强

##代码重构

##使用工具重构

###重构之提炼函数

Intellij IDEA带了一些有意思的快捷键，或者说自己之前不在意这些快捷键的存在。重构作为单独的一个菜单，显然也突显了其功能的重要性，说说**提炼函数**，或者说提出方法。

快捷键

Mac:  ``alt``+``command``+``M``

Windows/Linux: ``Ctrl``+``Alt``+``M``

鼠标: Refactor | Extract | Method

####重构之前

以重构一书代码为例，重构之前的代码

```java
public class extract {
    private String _name;

    void printOwing(double amount){
        printBanner();
        
        System.out.println("name:" + _name);
        System.out.println("amount" + amount);
    }

    private void printBanner() {
    }
}
```

####重构

选中

```java
System.out.println("name:" + _name);
System.out.println("amount" + amount);
```

按下上述的快捷键，会弹出下面的对话框

![Extrct Method](http://repractise.phodal.com/img/refactor/extract-method.png)

输入

     printDetails

那么重构就完成了。

####重构之后

IDE就可以将方法提出来

```java
public class extract {
    private String _name;

    void printOwing(double amount){
        printBanner();
        printDetails(amount);
    }

    private void printDetails(double amount) {
        System.out.println("name:" + _name);
        System.out.println("amount" + amount);
    }

    private void printBanner() {
    }
}
```

####重构

还有一种就以Intellij IDEA的示例为例，这像是在说其的智能。

```java
public class extract {
    public void method() {
        int one = 1;
        int two = 2;
        int three = one + two;
        int four = one + three;
    }
}
```

只是这次要选中的只有一行，

```
int three = one + two;
``

以便于其的智能，它便很愉快地告诉你它又找到了一个重复

     IDE has detected 1 code fragments in this file that can be replaced with a call to extracted method...

便返回了这样一个结果

```java
public class extract {

    public void method() {
        int one = 1;
        int two = 2;
        int three = add(one, two);
        int four = add(one, three);
    }

    private int add(int one, int two) {
        return one + two;
    }

}
```

然而我们就可以很愉快地继续和它玩耍了。当然这其中还会有一些更复杂的情形，当学会了这一个剩下的也不难了。

###重构之内联函数

继续走这重构一书的复习之路，接着便是内联，除了内联变量，当然还有内联函数。

快捷键

Mac:  ``alt``+``command``+``M``

Windows/Linux: ``Ctrl``+``Alt``+``M``

鼠标: Refactor | Inline

####重构之前

以之前的[提炼函数](http://www.phodal.com/blog/intellij-idea-refactor-extract-method/)为例

```java
public class extract {

    public void method() {
        int one = 1;
        int two = 2;
        int three = add(one, two);
        int four = add(one, three);
    }

    private int add(int one, int two) {
        return one + two;
    }

}
```

在``add(one,two)``很愉快地按上个快捷键吧，就会弹出

![Inline Method](http://repractise.phodal.com/img/refactor/inline.jpg)

再轻轻地回车，Refactor就这么结束了。。

####Intellij Idea 内联临时变量

以书中的代码为例

```java
double basePrice = anOrder.basePrice();
return (basePrice > 1000);
```

同样的，按下``Command``+``alt``+``N``

```java
return (anOrder.basePrice() > 1000);
```

对于python之类的语言也是如此

```python
def inline_method():
    baseprice = anOrder.basePrice()
    return baseprice > 1000
```

###重构之以查询取代临时变量

继续看看有木有什么木有试过的功能，从之前的[内联函数](http://www.phodal.com/blog/intellij-idea-refactor-inline-method/)、[提炼函数](http://www.phodal.com/blog/intellij-idea-refactor-extract-method/)再到现在的[Replace Temp With Query](http://www.phodal.com/blog/intellij-idea-refactor-replace-temp-with-query)(以查询取代临时变量)

快捷键

Mac:  木有

Windows/Linux:  木有

或者: ``Shift``+``alt``+``command``+``T`` 再选择  ``Replace Temp with Query``

鼠标: **Refactor** | ``Replace Temp with Query``

####重构之前

过多的临时变量会让我们写出更长的函数，函数不应该太多，以便使功能单一。这也是重构的另外的目的所在，只有函数专注于其功能，才会更容易读懂。

以书中的代码为例

```java
import java.lang.System;

public class replaceTemp {
    public void count() {
        double basePrice = _quantity * _itemPrice;
        if (basePrice > 1000) {
            return basePrice * 0.95;
        } else {
            return basePrice * 0.98;
        }
    }
}
```

####重构

选中``basePrice``很愉快地拿鼠标点上面的重构

![Replace Temp With Query](http://repractise.phodal.com/img/refactor/replace.jpg)

便会返回

```java
import java.lang.System;

public class replaceTemp {
    public void count() {
        if (basePrice() > 1000) {
            return basePrice() * 0.95;
        } else {
            return basePrice() * 0.98;
        }
    }

    private double basePrice() {
        return _quantity * _itemPrice;
    }
}
```

而实际上我们也可以

1. 选中

    _quantity * _itemPrice

2. 对其进行``Extrace Method``

3. 选择``basePrice``再``Inline Method``

在Intellij IDEA的文档中对此是这样的例子

```java
public class replaceTemp {

    public void method() {
        String str = "str";
        String aString = returnString().concat(str);
        System.out.println(aString);
    }

}
```

接着我们选中``aString``，再打开重构菜单，或者

``Command``+``Alt``+``Shift``+``T`` 再选中Replace Temp with Query

便会有下面的结果:

```java
import java.lang.String;

public class replaceTemp {

    public void method() {
        String str = "str";
        System.out.println(aString(str));
    }

    private String aString(String str) {
        return returnString().concat(str);
    }

}
```

##借助工具重构

 - 当你写了一大堆代码,你没有意识到里面有一大堆重复。
 - 当你写了一大堆测试,却不知道覆盖率有多少。

这就是个问题了，于是偶然间看到了一个叫code climate的网站。

###Code Climate

> Code Climate consolidates the results from a suite of static analysis tools into a single, real-time report, giving your team the information it needs to identify hotspots, evaluate new approaches, and improve code quality.

Code Climate整合一组静态分析工具的结果到一个单一的，实时的报告，让您的团队需要识别热点，探讨新的方法，提高代码质量的信息。

简单地来说:

- 对我们的代码评分
- 找出代码中的坏味道

于是，我们先来了个例子

Rating 	| Name |	Complexity   |	Duplication	| Churn    |	C/M    	| Coverage |	Smells 
--------|------|--------------|-------------|----------|---------|----------|----------
A |	lib/coap/coap_request_handler.js |	24 |	0 |	6 |	2.6 |	46.4% |	0
A |	lib/coap/coap_result_helper.js |	14	| 0	| 2 |	3.4 |	80.0% |	0
A	| lib/coap/coap_server.js |	16 |	0 |	5 |	5.2 |	44.0% |	0
A	| lib/database/db_factory.js |	8 |	0 |	3 |	3.8 |	92.3% |	0
A |	lib/database/iot_db.js |	7 |	0 |	6 |	1.0 |	58.8% |	0
A |	lib/database/mongodb_helper.js |	63 | 0 |	11 |	4.5	 | 35.0%	| 0
C |	lib/database/sqlite_helper.js |	32 |	86 |	10 |	4.5 |	35.0% |	2
B |	lib/rest/rest_helper.js	 | 19	| 62 |	3 |	4.7	| 37.5% |	2
A |	lib/rest/rest_server.js |	17 |	0 |	2 |	8.6	| 88.9% |	0
A |	lib/url_handler.js |	9 |	0	| 5 |	2.2	| 94.1% |	0

分享得到的最后的结果是:

![Coverage](http://repractise.phodal.com/img/refactor/coverage.png)

####代码的坏味道

于是我们就打开``lib/database/sqlite_helper.js``，因为其中有两个坏味道

> Similar code found in two :expression_statement nodes (mass = 86)

在代码的 ``lib/database/sqlite_helper.js:58…61 < >``

```javascript
SQLiteHelper.prototype.deleteData = function (url, callback) {
    'use strict';
    var sql_command = "DELETE FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    SQLiteHelper.prototype.basic(sql_command, callback);
```

lib/database/sqlite_helper.js:64…67 < >

与

```javascript
SQLiteHelper.prototype.getData = function (url, callback) {
    'use strict';
    var sql_command = "SELECT * FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    SQLiteHelper.prototype.basic(sql_command, callback);
```

只是这是之前修改过的重复。。

原来的代码是这样的

```javascript
SQLiteHelper.prototype.postData = function (block, callback) {
    'use strict';
    var db = new sqlite3.Database(config.db_name);
    var str = this.parseData(config.keys);
    var string = this.parseData(block);

    var sql_command = "insert or replace into " + config.table_name + " (" + str + ") VALUES (" + string + ");";
    db.all(sql_command, function (err) {
        SQLiteHelper.prototype.errorHandler(err);
        db.close();
        callback();
    });
};

SQLiteHelper.prototype.deleteData = function (url, callback) {
    'use strict';
    var db = new sqlite3.Database(config.db_name);
    var sql_command = "DELETE FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    db.all(sql_command, function (err) {
        SQLiteHelper.prototype.errorHandler(err);
        db.close();
        callback();
    });
};

SQLiteHelper.prototype.getData = function (url, callback) {
    'use strict';
    var db = new sqlite3.Database(config.db_name);
    var sql_command = "SELECT * FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    db.all(sql_command, function (err, rows) {
        SQLiteHelper.prototype.errorHandler(err);
        db.close();
        callback(JSON.stringify(rows));
    });
};
```

说的也是大量的重复，重构完的代码

```javascript
SQLiteHelper.prototype.basic = function(sql, db_callback){
    'use strict';
    var db = new sqlite3.Database(config.db_name);
    db.all(sql, function (err, rows) {
        SQLiteHelper.prototype.errorHandler(err);
        db.close();
        db_callback(JSON.stringify(rows));
    });

};

SQLiteHelper.prototype.postData = function (block, callback) {
    'use strict';
    var str = this.parseData(config.keys);
    var string = this.parseData(block);

    var sql_command = "insert or replace into " + config.table_name + " (" + str + ") VALUES (" + string + ");";
    SQLiteHelper.prototype.basic(sql_command, callback);
};

SQLiteHelper.prototype.deleteData = function (url, callback) {
    'use strict';
    var sql_command = "DELETE FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    SQLiteHelper.prototype.basic(sql_command, callback);
};

SQLiteHelper.prototype.getData = function (url, callback) {
    'use strict';
    var sql_command = "SELECT * FROM  " + config.table_name + "  where " + URLHandler.getKeyFromURL(url) + "=" + URLHandler.getValueFromURL(url);
    SQLiteHelper.prototype.basic(sql_command, callback);
};
```

重构完后的代码比原来还长，这似乎是个问题~~

##一次测试驱动开发

###故事

之前正在重写一个[物联网](http://www.phodal.com/iot)的服务端，主要便是结合CoAP、MQTT、HTTP等协议构成一个物联网的云服务。现在，主要的任务是集中于协议与授权。由于，不同协议间的授权是不一样的，最开始的时候我先写了一个http put授权的功能，而在起先的时候是如何测试的呢?

    curl --user root:root -X PUT -d '{ "dream": 1 }' -H "Content-Type: application/json" http://localhost:8899/topics/test

我只要顺利在request中看有无``req.headers.authorization``，我便可以继续往下，接着给个判断。毕竟，我们对HTTP协议还是蛮清楚的。

    if (!req.headers.authorization) {
      res.statusCode = 401;
      res.setHeader('WWW-Authenticate', 'Basic realm="Secure Area"');
      return res.end('Unauthorized');
    }
       
       
可是除了HTTP协议，还有MQTT和CoAP。对于MQTT协议来说，那还算好，毕竟自带授权，如:

    mosquitto_pub -u root -P root -h localhost -d -t lettuce -m "Hello, MQTT. This is my first message."
       
便可以让我们简单地完成这个功能，然而有的协议是没有这样的功能如CoAP协议中是用Option来进行授权的。现在的工具如libcoap只能有如下的简单功能

    coap-client -m get coap://127.0.0.1:5683/topics/zero -T

于是，先写了个测试脚本来验证功能。

	var coap     = require('coap');
	var request  = coap.request;
	var req = request({hostname: 'localhost',port:5683,pathname: '',method: 'POST'});

	...
	
	req.setHeader("Accept", "application/json");
	req.setOption('Block2',  [new Buffer('phodal'), new Buffer('phodal')]);
	
	...

	req.end();
	
写完测试脚本后发现不对了，这个不应该是测试的代码吗? 于是将其放到了spec中，接着发现了上面的全部功能的实现过程为什么不用TDD实现呢？

###说说测试驱动开发

测试驱动开发是一个很"古老"的程序开发方法，然而由于国内的开发流程的问题——即开发人员负责功能的测试，导致这么好的一项技术没有在国内推广。

测试驱动开发的主要过程是:

1. 先写功能的测试
2. 实现功能代码
3. 提交代码(commit -> 保证功能正常)
4. 重构功能代码

而对于这样的一个物联网项目来说，我已经有了几个有利的前提:

1. 已经有了原型
2. 框架设计

###思考

通常在我的理解下，TDD是可有可无的。既然我知道了我要实现的大部分功能，而且我也知道如何实现。与此同时，对Code Smell也保持着警惕、要保证功能被测试覆盖。那么，总的来说TDD带来的价值并不大。

然而，在当前这种情况下，我知道我想要的功能，但是我并不理解其深层次的功能。我需要花费大量的时候来理解，它为什么是这样的，需要先有一些脚本来知道它是怎么工作的。TDD变显得很有价值，换句话来说，在现有的情况下，TDD对于我们不了解的一些事情，可以驱动出更多的开发。毕竟在我们完成测试脚本之后，我们也会发现这些测试脚本成为了代码的一部分。

在这种理想的情况下，我们为什么不TDD呢?
