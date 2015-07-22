# JavaScriptStyleGuide
javascript代码规范
create by 不下 2015.07.21

###本代码规范参照idiomatic.js、Airbnb JavaScript Style Guide和谷歌代码规范，结合自己的经验编写而成，以供参考。


# Sublime Text 插件
jsFormat
方便开发者美化javascript代码。


# Object
使用如下语法创建Object对象
```javascript
// bad
var item = new Object();
var item2 = new Object();
item.a = 0;
item.b = 1;
item.c = 2;
item['strange key'] = 3;

// good
var item = {};
var item2 = {
  a: 0,
  b: 1,
  c: 2,
  'strange key': 3
};

```
不要使用保留关键字作为key，IE8中会报错。
```javascript
// bad
var superman = {
  default: { clark: 'kent' },
  private: true
};

// good
var superman = {
  defaults: { clark: 'kent' },
  hidden: true
};
```
使用可辨识的同义词替换保留关键词
```javascript
// bad
var superman = {
  class: 'alien'
};

// bad
var superman = {
  klass: 'alien'
};

// good
var superman = {
  type: 'alien'
};
```

# Array
使用如下语法创建数组
```javascript
// bad
// Length is 3.
var a1 = new Array(x1, x2, x3);

// Length is 2.
var a2 = new Array(x1, x2);

// 如果 x1 是一个数字，并且是个自然数，那么所创建的数组的长度为 x1。
// 如果 x1 是一个数字，但不是个自然色，则会抛出一个异常。
// 其他情况下，会创建一个长度为 1 ，元素为 x1的数组。
var a3 = new Array(x1);

// Length is 0.
var a4 = new Array();

// good
var a = [x1, x2, x3];
var a2 = [x1, x2];
var a3 = [x1];
var a4 = [];
```
使用数组的push方法为数组添加新的元素。
```javascript
var someStack = [];


// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```
当你需要拷贝一个数组时，请使用数组方法slice()
```javascript
var len = items.length;
var itemsCopy = [];
var i;

// bad
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```
将一个类数组对象转制为数组，也可以使用数组方法slice()
```javascript
var len = items.length;
var itemsCopy = [];
var i;

// bad
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```

# Strings
使用单引号定义字符串
```javascript
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;
```
字符串长度超过80个字符是，应该分成多行，通过字符串连接符进行连接。</br>
注：如果过度使用连接符，长字符串拼接会有性能问题。
```javascript
// bad
var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

// bad
var errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// good
var errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';
```
如果是需要用程序生成字符串，可以使用数组方法join替代连接符，尤其是在IE中。
```javascript
var items;
var messages;
var length;
var i;

messages = [{
  state: 'success',
  message: 'This one worked.'
}, {
  state: 'success',
  message: 'This one worked as well.'
}, {
  state: 'error',
  message: 'This one did not work.'
}];

length = messages.length;

// bad
function inbox(messages) {
  items = '<ul>';

  for (i = 0; i < length; i++) {
    items += '<li>' + messages[i].message + '</li>';
  }

  return items + '</ul>';
}

// good
function inbox(messages) {
  items = [];

  for (i = 0; i < length; i++) {
    // use direct assignment in this case because we're micro-optimizing.
    items[i] = '<li>' + messages[i].message + '</li>';
  }

  return '<ul>' + items.join('') + '</ul>';
}
```

# Functions
定义 Function:
```javascript
// 匿名 function 表达式
var anonymous = function() {
  return true;
};

// 命名 function 表达式
var named = function named() {
  return true;
};

// 立即执行 function 表达式 (IIFE: immediately-invoked function expression)
(function() {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

绝对不要在“非功能”块中申明方法（如，if、while等），而是通过将方法赋值给一个变量实现。浏览器会允许你这么做，但是他们会有各自的解析。</br>
注：ECMA-262 定义一个代码块是一组语句，但申明方法并不算是语句。
```javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
var test;
if (currentUser) {
  test = function test() {
    console.log('Yup.');
  };
}
```
绝对不用使用arguments作为方法的参数名。它会高优先级的覆盖掉每个方法中默认都有的arguments对象。
```javascript
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```

# Properties
使用 . 访问对象的属性
```javascript
var luke = {
  jedi: true,
  age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```
当属性名放在一个变量中时，使用下标 [] 的形式访问属性
```javascript
var luke = {
  jedi: true,
  age: 28
};

function getProp(prop) {
  return luke[prop];
}

var isJedi = getProp('jedi');
```

# 变量 Variables
申明变量时，必须使用 var 。如果不这么做，所申明的变量将会是一个全局变量，我们要劲量避免申明全局变量。
```javascript
// bad
superPower = new SuperPower();

// good
var superPower = new SuperPower();
```
使用一个 var 打头申明一组变量，或者每个变量都用一个 var 申明都可以，不做强求。
```javascript
// bad
// (compare to above, and try to spot the mistake)
var items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';


// good
var items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// good
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';
```
在作用域的顶部申明变量，这样有主意避免变量申明和赋值提升相关的问题。
```javascript
// bad
function() {
  test();
  console.log('doing stuff..');

  //..other stuff..

  var name = getName();

  if (name === 'test') {
    return false;
  }

  return name;
}

// good
function() {
  var name = getName();

  test();
  console.log('doing stuff..');

  //..other stuff..

  if (name === 'test') {
    return false;
  }

  return name;
}

// bad - unnecessary function call
function() {
  var name = getName();

  if (!arguments.length) {
    return false;
  }

  this.setFirstName(name);

  return true;
}

// good
function() {
  var name;

  if (!arguments.length) {
    return false;
  }

  name = getName();
  this.setFirstName(name);

  return true;
}
```

# 比较操作符
优先使用 === 和 !== 替换 == 和 != </br>
* 像if这样的条件判断语句，会遵循下面简单的规则，将表达式转换为布尔型的值。</br>
  * Objects 等价于 true</br>
  * Undefined 等价于 false</br>
  * Null 等价于 false</br>
  * Booleans 等价于 对应的布尔值</br>
  * Numbers 如果是 0 或者 NaN 等价于 false，其他的等价于 true</br>
  * Strings 除了空字符串 '' 等价于 false，其他的等价于 true</br>
```javascript
if ([0]) {
  // true
  // An array is an object, objects evaluate to true
}
```
使用短判断
```javascript
// bad
if (name !== '') {
  // ...stuff...
}

// good
if (name) {
  // ...stuff...
}

// bad
if (collection.length > 0) {
  // ...stuff...
}

// good
if (collection.length) {
  // ...stuff...
}
```

# 代码块
所有的多行代码的代码块，都要使用大括号括起来。
```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}
```
如果使用多行的if、else语句，else 应该跟在前一个 if 代码块的 } 后面。
```javascript
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```
将for循环中的变量提升到上面进行定义
```javascript
// bad
for ( i = 0; i < 100; i++ ) {
  // 语句
}

// normal
for ( var i = 0; i < 100; i++ ) {
  // 语句
}

// good
var i,
  length = 100;

for ( i = 0; i < length; i++ ) {
  // 语句
}

// good
var i = 0,
  length = 100;

for ( ; i < length; i++ ) {
  // 语句
}

// good
var prop;
for ( prop in object ) {
  // 语句
}

```
# 注释
多行注释使用 ```javascript/** ... */ ```注释内容包括描述，所有参数、返回值的具体类型和值
```javascript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param {String} tag
 * @return {Element} element
 */
function make(tag) {

  // ...stuff...

  return element;
}
```
  使用 ```javascript// ```
进行单行注释。注释时，在需要注释的语句上面新起一行，并在注释的上面保留一行空行。
```javascript
// bad
var active = true;  // is current tab

// good
// is current tab
var active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  var type = this._type || 'no type';

  return type;
}
```
使用前缀 ```javascriptFIXME 和 TODO``` 标注注释，方便开发人员快速理解需要解决的问题，或着提出解决的建议。

```javascript

// 使用 FIXME 标注一个问题
function Calculator() {

  // FIXME: shouldn't use a global here
  total = 0;

  return this;
}
```
```javascript

// 使用 TODO 标注问题的解决方案
function Calculator() {

  // TODO: total should be configurable by an options param
  this.total = 0;

  return this;
}
```

# 空格
永远都不要混用空格和Tab
* 编辑中设置两个空格代替Tab
* 总是打开 “显示不可见字符” 这个设置
```javascript
// bad
function() {
∙∙∙∙var name;
}

// bad
function() {
∙var name;
}

// good
function() {
∙∙var name;
}
```
在 { 前放着一个空格
```javascript
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});
```
在 if 、while等语句的 ( 前放置一个空格。命名方法、调用方法时，不需要这么做。
```javascript
// bad
if(isJedi) {
  fight ();
}

// good
if (isJedi) {
  fight();
}

// bad
function fight () {
  console.log ('Swooosh!');
}

// good
function fight() {
  console.log('Swooosh!');
}
```
用空格将操作符隔开
```javascript
// bad
var x=y+5;

// good
var x = y + 5;
```
使用锯齿状缩进将函数连分割成多行，并以 . 开头，以强调只是针对一个函数的调用，而不是一个新的语句。
```javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// good
var leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);
```
在每个代码块后面保留一个空行，然后再写后面的语句。
```javascript
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;

// bad
var obj = {
  foo: function() {
  },
  bar: function() {
  }
};
return obj;

// good
var obj = {
  foo: function() {
  },

  bar: function() {
  }
};

return obj;
```

# 逗号
如下使用逗号
```javascript
// bad
var story = [
    once
  , upon
  , aTime
];

// good
var story = [
  once,
  upon,
  aTime
];

// bad
var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};

// good
var hero = {
  firstName: 'Bob',
  lastName: 'Parr',
  heroName: 'Mr. Incredible',
  superPower: 'strength'
};
```
注：多余的逗号，在ie6/ie7和ie9 quirksmode会引发错误。同样的在一些执行ES3标准的浏览器中，多余的逗号会使数组的长度增加。这个问题在ES5中已经修复。
```javascript
// bad
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn',
  };

  var heroes = [
    'Batman',
    'Superman',
  ];

  // good
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn'
  };

  var heroes = [
    'Batman',
    'Superman'
  ];
```

# 分号
如下，需要在每个语句结束的时候都添加分号。
```javascript
// bad
(function() {
  var name = 'Skywalker'
  return name
})()

// good
(function() {
  var name = 'Skywalker';
  return name;
})();

// good (两个立即执行的函数文件拼接时，在函数前面添加一个分号，防止后面的函数变成前面函数的参数)
;(function() {
  var name = 'Skywalker';
  return name;
})();
```

# 类型转换
在申明开头进行强制转换
Strings
```javascript
//  => this.reviewScore = 9;

// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```
在使用parseInt转换成数字是需要标注出进制。
```javascript
var inputValue = '4';

// bad
var val = new Number(inputValue);

// bad
var val = +inputValue;

// bad
var val = inputValue >> 0;

// bad
var val = parseInt(inputValue);

// good
var val = Number(inputValue);

// good
var val = parseInt(inputValue, 10);
```
如果由于某种原因，你正在做的事情中，parseInt函数成为的你的瓶颈，出于性能的考虑需要使用Bitshift，则需要写好注释解释为什么你这么做。
```javascript
// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
var val = inputValue >> 0;
```
注：在使用Bitshifting时要注意，当转换一个表示64位的数字时，由于Bitshifting总是返回一个32位的数字，会导致转换错误。
```javascript
'2147483647' >> 0 //=> 2147483647
'2147483648' >> 0 //=> -2147483648
'2147483649' >> 0 //=> -2147483647
```
布尔
```javascript
var age = 0;

// bad
var hasAge = new Boolean(age);

// good
var hasAge = Boolean(age);

// good
var hasAge = !!age;
```

# 命名规范
避免使用单个字母进行命名。尽量描述清楚你的命名
```javascript
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```
使用全字母大写，下划线分隔的方式命名不变量。不要使用 const 关键词命名常量，ie不支持。
```javascript
// bad
const md5_key = 'ya23adfuodf4sd';

// good
var MD5_key = 'ya23adfuodf4sd';
```
使用驼峰命名法（camelCase），命名你的对象、方法和实例
```javascript
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
var o = {};
function c() {}

// good
var thisIsMyObject = {};
function thisIsMyFunction() {}
```
正则表达式变量使用 r 作为前缀
```javascript
// good
rNumber = /(\d)+/;
```
使用帕斯卡命名法（PascalCase），命名你的构造函数或类
```javascript
// bad
function user(options) {
  this.name = options.name;
}

var bad = new user({
  name: 'nope'
});

// good
function User(options) {
  this.name = options.name;
}

var good = new User({
  name: 'yup'
});
```
用下划线打头命名私有属性
```javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```
用 _this 保存 this 的引用
```javascript
// bad
function() {
  var self = this;
  return function() {
    console.log(self);
  };
}

// bad
function() {
  var that = this;
  return function() {
    console.log(that);
  };
}

// good
function() {
  var _this = this;
  return function() {
    console.log(_this);
  };
}
```

# 属性存取函数
属性存取函数不是必须的。
如果你需要使用属性存取函数，请用getVal()和setVal('hello')的方式命名
```javascript
// bad
dragon.age();

// good
dragon.getAge();

// bad
dragon.age(25);

// good
dragon.setAge(25);
```
如果属性是布尔值，使用isVal()或者hasVal()的形式命名
```javascript
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

# 构造函数
分配而不是覆盖新对象的prototype。覆盖prototype会导致不能继承：通过复位prototype，会重写base的prototype
属性要定义在构造函数中，函数定义在prototype上
```javascript

// jedi.js
(function(global){
  function Jedi(foo) {
    console.log('new jedi');
    // good
    this.foo = foo;
    return this;
  }

  // bad
  Jedi.prototype.foo = 'foo';

  // bad
  Jedi.prototype = {
    fight: function fight() {
      console.log('fighting');
    },

    block: function block() {
      console.log('blocking');
    }
  };

  // good
  Jedi.prototype.fight = function fight() {
    console.log('fighting');
  };

  Jedi.prototype.block = function block() {
    console.log('blocking');
  };

  // To call constructor's without `new`, you might do this:
  var jedi = function( foo ) {
    return new Jedi( foo );
  };


  // expose our constructor to the global object
  global.jedi = jedi;

})(this)

```
函数可以返回 this ，以帮助函数连的使用
```javascript
// bad
Jedi.prototype.jump = function() {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
};

var luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
Jedi.prototype.jump = function() {
  this.jumping = true;
  return this;
};

Jedi.prototype.setHeight = function(height) {
  this.height = height;
  return this;
};

var luke = new Jedi('ulo');

luke.jump()
  .setHeight(20);
```
重写toString方法是可以的，只要保证可以正确执行，并不会产生其他的影响。
```javascript
function Jedi(options) {
  options || (options = {});
  this.name = options.name || 'no name';
}

Jedi.prototype.getName = function getName() {
  return this.name;
};

Jedi.prototype.toString = function toString() {
  return 'Jedi - ' + this.getName();
};
```

# Modules
```javascript

// module.js
(function( global ) {
  var Module = (function() {

    var data = "secret";

    return {
      // This is some boolean property
      bool: true,
      // Some string value
      string: "a string",
      // An array property
      array: [ 1, 2, 3, 4 ],
      // An object property
      object: {
        lang: "en-Us"
      },
      getData: function() {
        // get the current value of `data`
        return data;
      },
      setData: function( value ) {
        // set the value of `data` and return it
        return ( data = value );
      }
    };
  })();

  // Other things might happen here

  // expose our module to the global object
  global.Module = Module;

})( this );
```


# Events
当把数据传递给event时（无论是DOM event或者是自定义 event），传递的值都用哈希值代替。这样即使以后会新增加数据字段，也不用更新每个事件处理函数。
```javascript
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});
```
更好：
```javascript
// good
$(this).trigger('listingUpdated', { listingId : listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```

# jQuery
命名jQuery对象的变量时，用 $ 作为前缀。
```javascript
// bad
var sidebar = $('.sidebar');

// good
var $sidebar = $('.sidebar');
```
缓存 jQuery 查询
```javascript
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...stuff...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  var $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...stuff...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```
使用层叠方式查找DOM，如$('.sidebar ul') 或者 parent > child $('.sidebar > ul') 
需要调用作用域内的已经缓存的jQuery对象进行查询时，使用 find
```javascript
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar.find('ul').hide();
```

# 闭包
闭包，也许是js中最有用和最经常被忽略的功能。</br>
但是，有一点要特别注意，闭包会在它封闭的作用域中保存变量引用。其结果是，附带着会创建DOM对象的循环引用，从而产生内存泄露。
例如下面的代码：
```javascript

// bad
function foo(element, a, b) {
  element.onclick = function() { /* uses a and b */ };
}
```
在这个匿名方法中，闭包保存了element、a和b的引用，即使它不会用到element。我们构建了一个闭环，导致不会被垃圾清理回收。
在这种情况下，可以做如下重构：
```javascript

// good
function foo(element, a, b) {
  element.onclick = bar(a, b);
}

function bar(a, b) {
  return function() { /* uses a and b */ };
}
```

# delete
在现代的js引擎中，修改对象的元素个数比重新分配对应的值要慢的多。</br>
所以要尽量避免使用delete删除键，除非有必须删除一个键，如在迭代器中，或者是for in循环中。
```javascript

// bad
Foo.prototype.dispose = function() {
  delete this.property_;
};

// good
Foo.prototype.dispose = function() {
  this.property_ = null;
};
```


# eval
仅在代码加载器或者REPL（交互式的开发环境）中使用eval，通常情况下使用JSON.parse替代eval</br>
例如，如果从服务端返回如下字符串
```javascript
{
  "name": "Alice",
  "id": 31502,
  "email": "looking_glass@example.com"
}
```
```javascript

// bad
var userInfo = eval(feed);
var email = userInfo['email'];

// good
var userInfo = JSON.parse(feed);
var email = userInfo['email'];
```

# with(){}
不要使用with，因为：
在with语句中的函数定义和变量初始化可能产生令人惊讶，和直觉相抵触的行为。</br>
使用with语句速度要比不使用with语句的等价代码的速度慢得多。</br>
90%(或者更高比例)的with应用场景都可以用其他更好的方式代替</br>

# 参考
[idiomatic.js](https://github.com/rwaldron/idiomatic.js/)</br>
[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript/tree/master/es5)</br>
[google javascriptguide](http://google.github.io/styleguide/javascriptguide.xml)</br>


