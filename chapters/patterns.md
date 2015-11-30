#模式、设计与架构

设计模式算是在OO中比较有趣的东西，特别是对于如我之类的用得不是很多的，虽然有时候也会用上，但是并不知道用的是怎样的模式。之前了解了几天的设计模式，实际上也就是将平常经常用到的一些东西进行了总结，如此而已，学习设计模式的另外一个重要的意义在于，我们使用了设计模式的时候我们会知道自己使用了，并且还会知道用了是怎样的设计模式。

至于设计模式这个东西和有些东西一样，是发现的而不是发明的，换句话说，我们可以将经常合到一起的几种模式用一个新的模式来命名，它是复合模式，但是也可以用别的模式来命名。

设计模式算是简化了我们在面向对象设计时候的诸多不足，这个在系统设计的初期有时候会有一定的作用，不过多数时候对于我来说，会用上他的时候，多半是在重构的时候，因为不是很熟悉。


##观察者模式

观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。

观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态上发生变化时，会通知所有观察者对象，使它们能够自动更新自己。

一个软件系统常常要求在某一个对象的状态发生变化的时候，某些其它的对象做出相应的改变。做到这一点的设计方案有很多，但是为了使系统能够易于复用，应该选择低耦合度的设计方案。减少对象之间的耦合有利于系统的复用，但是同时设计师需要使这些低耦合度的对象之间能够维持行动的协调一致，保证高度的协作（Collaboration）。观察者模式是满足这一要求的各种设计方案中最重要的一种。

简单的来说，就是当我们监测到一个元素变化的时候，另外的元素依照此而改变。


###Ruby观察者模式

Ruby中为实现Observer模式提供了名为observer的库，observer库提供了Observer模块。
其API如下所示

方法名  | 功 能 
-------|-----------------
add_observer(observer)   |               添加观察者
delete_observer(observer) |            删除特定观察者
delete_observer            |                     删除观察者
count_observer              |                    观察者的数目
change(state=true)           |                 设置更新标志为真
changed?                      |                        检查更新标志
notify_observer(*arg)          |              通知更新，如果更新标志为真，调用观察者带参数arg的方法

####Ruby观察者简单示例

这里要做的就是获取一个json数据，将这个数据更新出来。

获取json数据，同时解析。

```ruby
require 'net/http'
require 'rubygems'
require 'json'

class GetData
  attr_reader:res,:parsed

  def initialize(uri)
    uri=URI(uri)
    @res=Net::HTTP.get(uri)
    @parsed=JSON.parse(res)
  end

  def id
    @parsed[0]["id"]
  end

  def sensors1
    @parsed[0]["sensors1"].round(2)
  end

  def sensors2
    @parsed[0]["sensors2"].round(2)
  end

  def temperature
    @parsed[0]["temperature"].round(2)
  end

  def led1
    @parsed[0]["led1"]
  end

end
```

下面这个也就是重点，和观察者相关的，就是被观察者，由这个获取数据。
通过changed ，同时用notify_observer方法告诉观察者

```ruby
require 'rubygems'
require 'thread'
require 'observer'
require 'getdata'
require 'ledstatus'

class Led 
	include Observable
	
    attr_reader:data
	def initialize
		@uri='http://www.xianuniversity.com/athome/1'
	end
	def getdata
		loop do 
			changed()
	   		data=GetData.new(@uri)
	   		changed
	   		notify_observers(data.id,data.sensors1,data.sensors2,data.temperature,data.led1)
	   		sleep 1
	   	end
	end
end
```

然后让我们新建一个观察者

```ruby
class LedStatus
  def update(arg,sensors1,sensors2,temperature,led1)
    puts "id:#{arg},sensors1:#{sensors1},sensors2:#{sensors2},temperature:#{temperature},led1:#{led1}"
  end
end
```

测试

```ruby
require 'spec_helper'

describe LedStatus do
  let(:ledstatus){LedStatus.new()}

  describe "Observable" do
    it "Should have a result" do 
      led=Led.new
      led.add_observer(ledstatus)
      led.getdata
    end
  end
end
```

测试结果如下所示

```bash
phodal@linux-dlkp:~/tw/observer&gt; rake
/usr/bin/ruby1.9 -S rspec ./spec/getdata_spec.rb ./spec/ledstatus_spec.rb
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:0
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:1
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:0
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:1
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:1
id:1,sensors1:22.0,sensors2:11.0,temperature:10.0,led1:1
```

使用Ruby自带的Observer库的优点是，让我们可以简化相互之间的依赖性。同时，也能简化程序的结构，相比于自己写observer的情况下。

##Node.js 简单工厂模式

> 从设计模式的类型上来说，简单工厂模式是属于创建型模式，又叫做静态工厂方法（Static Factory Method）模式，但不属于23种GOF设计模式之一。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现，学习了此模式可以为后面的很多中模式打下基础。

当我发现我在代码中重复写了很多个if来判断选择那个数据库的时候。于是，我就想着似乎这就可以用这个简单工厂模式来实现SQLite3与MongoDB的选择。

###MongoDB Helper与SQLite Helper类重复

对于我们的类来说是下面这样子的:

```javascript
function MongoDBHelper() {
    'use strict';
    return;
}

MongoDBHelper.deleteData = function (url, callback) {
    'use strict';
    ...    
};

MongoDBHelper.getData = function (url, callback) {
    'use strict';
    ...
};

MongoDBHelper.postData = function (block, callback) {
    'use strict';
    ...
};

MongoDBHelper.init = function () {
    'use strict';
    ...
};

module.exports = MongoDBHelper;
```

然而，我们可以发现的是，对于我们的SQLiteHelper来说也是类似的

```javascript
SQLiteHelper.init = function () {
    'use strict';
    ...
};

SQLiteHelper.postData = function (block, callback) {
    'use strict';   
    ...
};

SQLiteHelper.deleteData = function (url, callback) {
    'use strict';
    ...
};

SQLiteHelper.getData = function (url, db_callback) {
    'use strict';
    ...
};

module.exports = SQLiteHelper;
```

想来想去觉得写一个父类似乎是没有多大意义的，于是用了简单工厂模式来解决这个问题。

总之，就是我们可以用简单工厂模式来做一个DB Factory，于是便有了

```javascript
var MongoDBHelper   = require("./mongodb_helper");
var SQLiteHelper    = require("./sqlite_helper");
var config          = require('../../iot').config;

function DB_Factory() {
    'use strict';
    return;
}

DB_Factory.prototype.DBClass = SQLiteHelper;

DB_Factory.prototype.selectDB = function () {
    'use strict';
    if (config.db === 'sqlite3') {
        this.DBClass = SQLiteHelper;
    } else if (config.db === "mongodb") {
        this.DBClass = MongoDBHelper;
    }
    return this.DBClass;
};

module.exports = DB_Factory;
```

这样我们在使用的时候，便可以:

```javascript
var DB_Factory      = require("./lib/database/db_factory");

var db_factory = new DB_Factory();
var database = db_factory.selectDB();
database.init();
```

由于是直接由配置中读取进去的，这里的selectDB就不需要参数。

##Java Template Method(模板方法)

原本对于设计模式的写作还不在当前的计划中，然而因为在写TWU作业的时候，觉得代码写得不好，于是慢慢试着一点点重构，重新看着设计模式。也开始记录这一点点的方法，至少这些步骤是必要的。

###从基本的App说起

对于一个基本的C/C++/Java/Python的Application来说，他只需要有一个Main函数就够了。对于一个好一点的APP来说，他可能是下面的步骤，

```c
main(){
	init();
	while(!condition()){
   		do();
	}
}
```

上面的代码是我在学51/AVR等各式嵌入式设备时，经常是按上面的写法写的，对于一个更符合人性的App来说他应该会有一个退出函数。

```c
main(){
	init();
	while(!condition()){
   		do();
	}
	exit();
}
```

于是很幸运地我找到了这样的一个例子。

过去看过Arduino的代码，了解过他是如何工作的，对于一个Arduino的代码来说，必要的两个函数就是。

```cpp
void setup() {
}

void loop() {
}
```

setup()函数相当于上面的init()，而loop()函数刚相当于上面的do()。似乎这就是我们想要的东西，看看Arduino目录中的Arduino.h就会发现，如下的代码(删减部分代码)

```cpp
#include <Arduino.h>

int main(void)
{
	init();
	setup();	    
	for (;;) {
		loop();
		if (serialEventRun) serialEventRun();
	}
        
	return 0;
}
```

代码中的for(;;)看上去似乎比while(True)容易理解得多，这也就是为什么嵌入式中经常用到的是for(;;)，从某种意义上来说两者是等价的。再有不同的地方，就是gcc规定了,main()函数不能是void。so,两者是差不多的。只是没有，并没有在上面看到模板方法，等等。我们在上面所做的事情，便是创建一个框架。

##Template Method

> **模板方法**： 在一方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

对于我来说，我就是在基本的App中遇到的情况是一样的，在我的例子中，一开始我的代码是这样写的。

```java
public static void main(String[] args) throws IOException {
    initLibrary();
    while(!isQuit){
        loop();
    }
    exit;
}

protected void initLibrary(); {
    System.out.println(welcomeMessage);
}

protected void loop() {
    String key = "";
    Scanner sc = new Scanner(System.in);
    key = sc.nextLine();

    System.out.println(results);
    if(key.equals("Quit")){
        setQuit();
    }
}

protected void exit() {
    System.out.println("Quit Library");
}
```

只是这样写感觉很是别扭，看上去一点高大上的感觉，也木有。于是，打开书，找找灵感，就在《敏捷软件开发》一书中找到了类似的案例。Template Method模式可以分离能用的算法和具体的上下文，而我们通用的算法便是。

```c
main(){
	init();
	while(!condition()){
   		do();
	}
	exit();
}
```

看上去正好似乎我们当前的案例，于是便照猫画虎地来了一遍。

###Template Method实战

创建了一个名为App的抽象基类，

```java
public abstract class App {
    private boolean isQuit = false;

    protected abstract void loop();
    protected abstract void exit();

    private boolean quit() {
        return isQuit;
    }

    protected boolean setQuit() {
        return isQuit = true;
    }

    protected abstract void init();

    public void run(){
        init();
        while(!quit()){
            loop();
        }
        exit();
    }
}
```

而这个也和书中的一样，是一个通用的主循环应用程序。从应用的run函数中，可以看到主循环。而所有的工作也都交付给抽象方法，于是我们的LibraryApp就变成了


```java
public class LibraryApp extends App {
    private static String welcomeMessage = "Welcome to Biblioteca library";

    public static void main(String[] args) throws IOException {
        (new LibraryApp()).run();
    }

    protected void init() {
        System.out.println(welcomeMessage);
    }

    protected void loop() {
        String key = "";
        Scanner sc = new Scanner(System.in);
        key = sc.nextLine();

        if(key.equals("Quit")){
            setQuit();
        }
    }

    protected void exit() {
        System.out.println("Quit Library");
    }
}
```

然而，如书中所说``这是一个很好的用于示范TEMPLATE METHOD模式的例子，却不是一个合适的例子。``


##Hadoop Pipe and Filters模式

继续码点关于架构设计的一些小心得。架构是什么东西并没有那么重要，重要的是知道它存在过。我会面对不同的架构，有一些不同的想法。一个好的项目通常是存在一定的结构，就好像人们在建造房子的时候也都会有结构有一样。

我们看不到的架构，并不意味着这个架构不存在。

###Unix Shell

最出名的Pipe便是Unix中的Shell

**管道（英语：Pipeline）是原始的软件管道：即是一个由标准输入输出链接起来的进程集合，所以每一个进程的输出（stdout）被直接作为下一个进程的输入（stdin）。 每一个链接都由未命名管道实现。过滤程序经常被用于这种设置。**

所以对于这样一个很好的操作便是，统计某种类型的文件的个数:

```bash
ls -alh dot | grep .dot | wc -l
```

在执行

```bash
ls -alh dot
```

的输出便是下一个的输入，直至最后一个输出。

这个过程有点类似于工厂处理废水，

![pipe and filter][1]

上图是一个理想模型~~。

一个明显地步骤是，水中的杂质越来越少。

###Pipe and Filter模式

**Pipe and Filter**适合于处理数据流的系统。每个步骤都封装在一个过滤器组件中，数据通过相邻过滤器之间的管道传输。

- **pipe**: 传输、缓冲数据。
- **filter**: 输入、处理、输出数据。

这个处理过程有点类似于我们对数据库中数据的处理，不过可不会有这么多步骤。

###Fluent API

这个过程也有点类似于Fluent API、链式调用，只是这些都是DSL的一种方式。


流畅接口的初衷是构建可读的API，毕竟代码是写给人看的。

类似的，简单的看一下早先我们是通过方法级联来操作DOM

```javascript
var btn = document.createElement("BUTTON");        // Create a <button> element
var t = document.createTextNode("CLICK ME");       // Create a text node
btn.appendChild(t);                                // Append the text to <button>
document.body.appendChild(btn);                    // Append <button> to <body>
```

而用jQuery写的话，便是这样子

```javascript
$('<span>').append("CLICK ME");
```

等等

于是回我们便可以创建一个简单的示例来展示这个最简单的DSL

```javascript
Func = (function() {
    this.add = function(){
        console.log('1');
        return this;
    };
    this.result = function(){
        console.log('2');
        return this;
    };
    return this;
});

var func = new Func();
func.add().result();
```

然而这看上去像是表达式生成器。

###DSL 表达式生成器

> 表达式生成器对象提供一组连贯接口，之后将连贯接口调用转换为对底层命令-查询API的调用。

这样的API，我们可以在一些关于数据库的API中看到:

```javascript
var query =
  SQL('select name, desc from widgets')
   .WHERE('price < ', $(params.max_price), AND,
          'clearance = ', $(params.clearance))
   .ORDERBY('name asc');
```

链式调用有一个问题就是收尾，同上的代码里面我们没有收尾，这让人很迷惑。。加上一个query和end似乎是一个不错的结果。

###Pipe and Filter模式实战

所以，这个模式实际上更适合处理数据，如用Hadoop处理数据的时候，我们会用类似于如下的方法来处理我们的数据:

```pig
A = FOREACH LOGS_BASE GENERATE ToDate(timestamp, 'dd/MMM/yyyy:HH:mm:ss Z') as date, ip, url,(int)status,(int)bytes,referrer,useragent;
B = GROUP A BY (timestamp);
C = FOREACH B GENERATE FLATTEN(group) as (timestamp), COUNT(A) as count;
D = ORDER C BY timestamp,count desc;
```

每一次都是在上一次处理完的结果后，再处理的。


##其他

参考书目

 - 《Head First 设计模式》
 - 《设计模式》
 - 《敏捷软件开发 原则、模式与实践》
 - 《 面向模式的软件架构:模式系统》
 - 《Java应用架构设计》

