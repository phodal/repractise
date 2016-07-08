领域篇
===

DDD
---


DSL
---

DSL(domain-specific languages)即领域特定语言，唯一能够确定DSL边界的方法是考虑“一门语言的一种特定用法”和“该语言的设计者或使用者的意图。在试图设计一个DSL的时候，发现了一些有意思的简单的示例。

### DSL示例

#### jQuery 最流行的DSL

jQuery是一个Internal DSL的典型的例子。它是在一门现成语言内实现针对领域问题的描述。

```javascript
$('.mydiv').addClass('flash').draggable().css('color', 'blue')
```

这也就是其最出名的**链式方法调用**。

#### Cucumber.js

Cucumber, the popular Behaviour-Driven Development tool, brought to your JavaScript stack。它是使用通用语言描述该领域的问题。

```cucumber
Feature: Example feature
  As a user of cucumber.js
  I want to have documentation on cucumber
  So that I can concentrate on building awesome applications

  Scenario: Reading documentation
    Given I am on the Cucumber.js GitHub repository
    When I go to the README file
    Then I should see "Usage" as the page title
```

#### CoffeeScript

发明一门全新的语言描述该领域的问题。

```coffee
math =
  root:   Math.sqrt
  square: square
  cube:   (x) -> x * square x
```

#### JavaScript DSL 示例

所以由上面的结论我们可以知道的是，难度等级应该是

内部DSL < 外部DSL < 语言工作台(这是怎么翻译的)

接着在网上找到了一个高级一点的内部DSL示例，如果我们要做jQuery式的链式方法调用也是简单的，但是似乎没有足够的理由去说服其他人。

原文在: [http://alexyoung.org/2009/10/22/javascript-dsl/](http://alexyoung.org/2009/10/22/javascript-dsl/)，相当于是一个微测试框架。

```javascript
var DSLRunner = {
  run: function(methods) {
    this.ingredients = [];
    this.methods     = methods;

    this.executeAndRemove('first');

    for (var key in this.methods) {
      if (key !== 'last' && key.match(/^bake/)) {
        this.executeAndRemove(key);
      }
    }

    this.executeAndRemove('last');
  },

  addIngredient: function(ingredient) {
    this.ingredients.push(ingredient);
  },

  executeAndRemove: function(methodName) {
    var output = this.methods[methodName]();
    delete(this.methods[methodName]);
    return output;
  }
};

DSLRunner.run({
  first: function() {
    console.log("I happen first");
  },

  bakeCake: function() {
    console.log("Commencing cake baking");
  },

  bakeBread: function() {
    console.log("Baking bread");
  },
  last: function() {
    console.log("last");
  }
});
```

这个想法，看上去就是定义了一些map，然后执行。

接着，又看到了一个有意思的DSL，作者是在解决表单验证的问题[《JavaScript DSL Because I’m Tired of Writing If.. If…If…》](http://byatool.com/ui/javascript-dsl-because-im-tired-of-writing-if-if-if/)：

```javascript
 var rules =
    ['Username',
      ['is not empty', 'Username is required.'],
      ['is not longer than', 7, 'Username is too long.']],
    ['Name',
      ['is not empty', 'Name is required.']],
    ['Password',
      ['length is between', 4, 6, 'Password is not acceptable.']]];
```

有一个map对应了上面的方法

```javascript
 var methods = [
    ['is not empty', isNotEmpty],
    ['is not longer than', isNotLongerThan],
    ['length is between', isBetween]];
```

原文只给了一部分代码

```javascript
var methodPair = find(methods, function(method) {
    return car(method) === car(innerRule);
});

var methodToUse = peek(methodPair);

return function(obj) {
    var error = peek(innerRule);                           //error is the last index
    var values = sink(cdr(innerRule));                     //get everything but the error
    return methodToUse(obj, propertyName, error, values);  //construct the validation call
};
```
