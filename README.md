# Excel Micro TypeScript Style Guide

*基于[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)的最合理的TypeScript方法*

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [Type Annotations](#type-annotations)
  1. [Interfaces](#interfaces)
  1. [Organization](#organization)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6 Styles](#ecmascript-6-styles)
  1. [Typescript 1.5 Styles](#typescript-1.5-styles)
  1. [License](#license)

## Types

  - [1.1](#1.1) <a name='1.1'></a> **基本类型**: 访问基本类型时,直接使用其值.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **复杂类型**: 访问复杂类型时,需要引用其值.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  - [2.1](#2.1) <a name='2.1'></a> 所有的赋值都用`const`,避免使用`var`.

    > why? 这样可以确保你不能重新分配引用(变异),这可能导致bug和难以理解的代码.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> 如果你一定要对参数重新赋值,那就用`let`,而不是`var`.

    > Why? 因为`let`是块级作用域,而`var`是函数级作用域.

    ```javascript
    // bad
    var count = 1;
    if (true) {

      count += 1;

    }

    // good, use the let.
    let count = 1;
    if (true) {

      count += 1;

    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> 注意:`let`、`const`都是块级作用域.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - [3.1](#3.1) <a name='3.1'></a> 使用字面量创建对象.

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> 不要使用[保留字](http://es5.github.io/#x7.6.1)作为键. 在IE8中将无法使用.[更多信息](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    const superman = {
      default: { clark: 'kent' },
      private: true,
    };

    // good
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> 使用可读的同义词代替保留字.

    ```javascript
    // bad
    const superman = {
      class: 'alien',
    };

    // bad
    const superman = {
      klass: 'alien',
    };

    // good
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> 使用动态属性名称创建对象时,用计算后属性名称.

    > Why? 这可以使你将定义的所有属性放在对象的一个地方.

    ```javascript

    const getKey = function(k) {

      return `a key named ${k}`;

    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> 使用箭头函数代替对象属性或匿名函数的对象方法.

    ```javascript
    // bad
    const atom = {
      value: 1,
      addValue: function (value) {
        return atom.value + value;
      },
    };

    // bad
    const atom = {
      value: 1,
      addValue(value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,
      addValue: (value) => atom.value + value
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> 用属性值缩写.

    > Why? It is shorter to write and descriptive.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> 将你的所有缩写放在对象声明的开始.

    > Why? 这样也是为了更方便的知道有哪些属性用了缩写.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> 使用字面量创建数组.

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> 用Array#push代替直接向数组中添加一个值.

    ```javascript
    const someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> 用扩展运算符 `...` 做数组浅拷贝.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> 用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 将一个类数组对象转成一个数组.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a>用对象的解构赋值来获取和使用对象某个或多个属性值.

    > Why? 解构保存了这些属性的临时值/引用.

    ```javascript
    // bad
    const getFullName = function(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    const getFullName = function(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    const getFullName = function({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> 用数组解构.

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> 多个返回值用对象的解构,而不是数据解构.

    > Why? 你可以在后期添加新的属性或者变换变量的顺序而不会打破原有的调用.

    ```javascript
    // bad
    const processInput = function(input) {
      // 然后奇迹发生了
      return [left, right, top, bottom];

    }

    // 调用者需要想一想返回值的顺序
    const [left, __, top] = processInput(input);

    // good
    const processInput = function(input) {
      // 然后奇迹发生了
      return { left, right, top, bottom };

    }

    // 调用者只需要选择他想用的值就好了
    const { left, right } = processInput(input);
    ```


**[⬆ back to top](#table-of-contents)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> 对string用单引号 `''`.

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> 长度超过80个字符的字符串应使用字符串串联写在多行中.
  - [6.3](#6.3) <a name='6.3'></a> 注意:如果过度使用,带有连接的长字符串可能会影响性能. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> 用字符串模板而不是字符串拼接来组织可编程字符串.

    > Why? 模板字符串更具可读性、语法简洁、字符串插入参数.

    ```javascript
    // bad
    const sayHi = function(name) {

      return 'How are you, ' + name + '?';

    }

    // bad
    const sayHi = function(name) {

      return ['How are you, ', name, '?'].join();

    }

    // good
    const sayHi = function(name) {

      return `How are you, ${name}?`;

    }
    ```

**[⬆ back to top](#table-of-contents)**


## Functions

  - [7.1](#7.1) <a name='7.1'></a> 用命名函数表达式而不是函数声明.

    > Why? 错误放置的函数声明会引起误解,在少数情况下(如果有)您不能使用分配给变量的函数表达式. See [function-declarations-vs-function-expressions](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/).

    ```javascript
    // bad
    function foo() {
    }

    // good
    const foo = function() {
    };

    // good
    const foo = () => {
    };
    ```

  - [7.2](#7.2) <a name='7.2'></a> 函数表达式:

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> 不要在非函数块(if、while等等)内声明函数.把这个函数分配给一个变量.浏览器会允许你这样做,但浏览器解析方式不同,这是一个坏消息.
  - [7.4](#7.4) <a name='7.4'></a> **Note:** ECMA-262将 `block` 定义为语句列表. 函数声明不是语句. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {

      const test = function() {

        console.log('Nope.');

      }

    }

    // good
    let test;
    if (currentUser) {

      test = () => {

        console.log('Yup.');

      };

    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> 不要用`arguments`命名参数.它的优先级高于每个函数作用域自带的 `arguments` 对象, 这会导致函数自带的 `arguments` 值被覆盖.

    ```javascript
    // bad
    const nope = function(name, options, arguments) {
      // ...stuff...
    }

    // good
    const yup = function(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> 不要使用`arguments`,用rest语法`...`代替.

    > Why? `...`明确你想用哪个参数.而且rest参数是真数组,而不是类似数组的`arguments`.

    ```javascript
    // bad
    const concatenateAll = function() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    const concatenateAll = function(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> 用默认参数语法而不是在函数里对参数重新赋值.

    ```javascript
    // really bad
    const handleThings = function(opts) {
      // 不, 我们不该改arguments
      // 第二: 如果 opts 的值为 false, 它会被赋值为 {}
      // 虽然你想这么写, 但是这个会带来一些细微的bug
      opts = opts || {};
      // ...
    }

    // still bad
    const handleThings = function(opts) {

      if (opts === void 0) {

        opts = {};

      }
      // ...
    }

    // good
    const handleThings = function(opts = {}) {
      //opts为undefined时,会被赋值为{}
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> 默认参数避免副作用.

    > Why? 它会令人迷惑不解.

    ```javascript
    var b = 1;
    // bad
    const count = function(a = b++) {

      console.log(a);

    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```


**[⬆ back to top](#table-of-contents)**

## Arrow Functions

  - [8.1](#8.1) <a name='8.1'></a> 当你必须使用函数表达式时(如传递匿名函数时),请使用箭头函数符号.

    > Why? 它创建了一个`this`的当前执行上下文的函数的版本,这通常就是你想要的;而且箭头函数是更简洁的语法.

    > Why not? 如果函数相当复杂,则可以将该逻辑移到其自己的函数声明中.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {

      return x * x;

    });

    // good
    [1, 2, 3].map((x) => {

      return x * x;

    });

    // good
    [1, 2, 3].map((x) => x * x;);
    ```

  - [8.2](#8.2) <a name='8.2'></a> 如果函数主体适合一行且只有一个参数,则可以忽略大括号和括号,并使用隐式返回. 否则,添加括号,大括号并使用`return`语句.

    > Why? 语法糖,当多个函数链在一起的时候易读.

    > Why not? 当你计算返回一个对象.

    ```javascript
    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].reduce((total, n) => {
      return total + n;
    }, 0);
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - [9.1](#9.1) <a name='9.1'></a> 常用`class`,避免直接操作`prototype`.

    > Why? `class` 语法更简洁更易理解.

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }


    // good
    class Queue {

      constructor(contents = []) {
        this._queue = [...contents];
      }

      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
    ```

  - [9.2](#9.2) <a name='9.2'></a> 用`extends`实现继承.

    > Why? 它是一种内置的方法来继承原型功能而不打破 `instanceof`.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> 方法可以返回`this`来实现方法链.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {

      this.jumping = true;
      return true;

    };

    Jedi.prototype.setHeight = function(height) {

      this.height = height;

    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {

      jump() {

        this.jumping = true;
        return this;

      }

      setHeight(height) {

        this.height = height;
        return this;

      }

    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - [9.4](#9.4) <a name='9.4'></a> 写一个定制的toString()方法是可以的,只要保证它是可以正常工作且没有副作用的.

    ```javascript
    class Jedi {

      contructor(options = {}) {

        this.name = options.name || 'no name';

      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }

    }
    ```

<a name="ts-classes"></a>
  - [9.5](#9.5) <a name='9.5'></a> Typescript classes placeholder.

**[⬆ back to top](#table-of-contents)**


## Modules

  - [10.1](#10.1) <a name='10.1'></a> 用(`import`/`export`) 模块而不是无标准的模块系统.你可以随时转到你喜欢的模块系统.

    > Why? 模块化是未来,让我们现在就开启未来吧.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> 不要直接从 `import` 中导出.

    > Why? 虽然一行是简洁的,有一个明确的方式进口和一个明确的出口方式来保证一致性.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.3](#10.3) <a name='10.3'></a> 将TypeScript模块导入用于具有类型定义的非ES6库.  检查[DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped) 中可用的类型定义文件.

    > Why? 这将在可用时提供来自外部模块的类型信息

    ```javascript
    // bad
    /// <reference path="lodash/lodash.d.ts" />
    var lodash = require('lodash')

    // good
    /// <reference path="lodash/lodash.d.ts" />
    import lodash = require('lodash')
    ```

  - [10.4](#10.4) <a name='10.4'></a> 按类型导入组模块,然后按变量名称按字母顺序. 请遵循以下规则来排序模块导入:
     + 具有类型定义的外部库
     + 具有通配符导入功能的内部typescript模块
     + 不带通配符导入的内部typescript模块
     + 没有类型定义的外部库

    > Why? 这样可以使导入部分在所有模块中保持一致.

    ```javascript
    // bad
    /// <reference path="../typings/tsd.d.ts" />
    import * as Api from './api';
    import _ = require('lodash');
    var Distillery = require('distillery-js');
    import Partner from './partner';
    import * as Util from './util';
    import Q = require('Q');
    var request = require('request');
    import Customer from './customer';

    // good
    /// <reference path="../typings/tsd.d.ts" />
    import _ = require('lodash');
    import Q = require('Q');
    import * as Api from './api';
    import * as Util from './util';
    import Customer from './customer';
    import Partner from './partner';
    var Distillery = require('distillery-js');
    var request = require('request');
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  - [11.1](#11.1) <a name='11.1'></a> 不要使用迭代器.用JavaScript高级函数(例如 `map()` and `reduce()`) 代替 `for-of` 这样的循环.

    > Why? 这强调了我们不可变的规则. 处理返回值的纯函数比副作用更容易.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {

      sum += num;

    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> 现在不要用generator.

    > Why? 它在ES5上支持的不好.

**[⬆ back to top](#table-of-contents)**


## Properties

  - [12.1](#12.1) <a name='12.1'></a> 访问属性时使用点符号.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> 当获取的属性是变量时用方括号`[]`取.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    const getProp = function(prop) {

      return luke[prop];

    }

    const isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - [13.1](#13.1) <a name='13.1'></a> 始终使用const声明变量. 不这样做将导致全局变量. 我们要避免污染全局名称空间.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> 每个变量使用一个 `const` 声明.

    > Why? 这种方式很容易去声明新的变量,你不用去考虑把`;`调换成`,`,或者引入一个只有标点的不同的变化.这种做法也可以是你在调试的时候单步每个声明语句,而不是一下跳过所有声明.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> `const` 放一起,`let` 放一起.

    > Why? 在你需要分配一个新的变量, 而这个变量依赖之前分配过的变量的时候,这种做法是有帮助的.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> 在你需要的地方声明变量,但是要放在合理的位置.

    > Why? `let` 和 `const` 都是块级作用域而不是函数级作用域.

    ```javascript
    // good
    function() {

      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {

        return false;

      }

      return name;

    }

    // bad - unnessary function call
    function(hasName) {

      const name = getName();

      if (!hasName) {

        return false;

      }

      this.setFirstName(name);

      return true;

    }

    // good
    function(hasName) {

      if (!hasName) {

        return false;

      }

      const name = getName();
      this.setFirstName(name);

      return true;

    }
    ```

**[⬆ back to top](#table-of-contents)**


## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> `var`声明会被提前到他的作用域的最前面,它分配的值还没有提前.`const` 和 `let`被赋予了新的调用概念 [时效区 —— Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). 重要的是要知道为什么 [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // 我们知道这个不会工作,假设没有定义全局的notDefined
    function example() {

      console.log(notDefined); // => throws a ReferenceError

    }

    // 在你引用的地方之后声明一个变量,他会正常输出是因为变量作用域上升.
    // 注意: declaredButNotAssigned的值没有上升
    function example() {

      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;

    }

    // 解释器把变量声明提升到作用域最前面,
    // 可以重写成如下例子, 二者意义相同
    function example() {

      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;

    }

    // 用 const, let就不一样了
    function example() {

      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;

    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> 匿名函数表达式将使用其变量名,而不是函数分配.

    ```javascript
    function example() {

      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {

        console.log('anonymous function expression');

      };

    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> 已命名函数表达式提升他的变量名,不是函数名或函数体.

    ```javascript
    function example() {

      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {

        console.log('Flying');

      };

    }

    // 函数名和变量名一样是也如此
    function example() {

      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {

        console.log('named');

      }

    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> 函数声明则提升了函数名和函数体.

    ```javascript
    function example() {

      superPower(); // => Flying

      function superPower() {

        console.log('Flying');

      }

    }
    ```

  - 详情请见 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**


## Comparison Operators & Equality

  - [15.1](#15.1) <a name='15.1'></a>  用 `===` 和 `!==` 而不是 `==` 和 `!=`.
  - [15.2](#15.2) <a name='15.2'></a> 条件语句如'if'语句使用强制`ToBoolean'抽象方法来评估它们的表达式,并且始终遵循以下简单规则:

    + **Objects** 计算成 **true**
    + **Undefined** 计算成 **false**
    + **Null** 计算成 **false**
    + **Booleans** 计算成 **the value of the boolean**
    + **Numbers** 
      + **+0, -0, or NaN** 计算成 **false**
      + 其他 **true**
    + **Strings**
      + `''` 计算成 **false**
      + 其他 **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> 布尔值用缩写.

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

  - [15.4](#15.4) <a name='15.4'></a> 更多信息请见 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

  - [16.1](#16.1) <a name='16.1'></a> 将大括号与多行块一起使用,或者将大括号省略给两个行块.

    ```javascript
    // bad
    if (test) return false;

    // ok
    if (test)
      return false;

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

  - [16.2](#16.2) <a name='16.2'></a> `if`表达式的`else`和`if`的关闭大括号在一行.

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

    - [16.3](#16.3) <a name='16.3'></a> 如果您使用带有`if`和`else`的多行块,请不要省略花括号.

      > Why? 在多行块中省略花括号很容易导致意外行为.

      ```javascript
      // bad
      if (test)
        thing1();
        thing2();
      else
        thing3();

      // good
      if (test) {
        thing1();
        thing2();
      } else {
        thing3();
      }
      ```


**[⬆ back to top](#table-of-contents)**


## Comments

  - [17.1](#17.1) <a name='17.1'></a> 对多行注释使用`/ ** ... * /`. 包括说明,为所有参数指定类型和值并返回值.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    const make = function(tag) {

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
    const make = function(tag) {

      // ...stuff...

      return element;

    }
    ```

  - [17.2](#17.2) <a name='17.2'></a> 单行注释用`//`. 将单行注释放在被注释区域上面.如果注释不是在第一行,那么注释前面就空一行.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    const getType = function() {

      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;

    }

    // good
    const getType = function() {

      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;

    }
    ```

  - [17.3](#17.3) <a name='17.3'></a> 给您的注释加上FIXME或TODO前缀可以帮助其他开发人员快速了解您是否指出需要重新审视的问题,或者是否建议解决需要实施的问题. 这些与常规评论不同,因为它们是可行的. 这些动作是" FIXME-需要弄清楚"或" TODO-需要实现".

  - [17.4](#17.4) <a name='17.4'></a> 使用`// FIXME:`注释问题.

    ```javascript
    class Calculator {

      constructor() {
        // FIXME: shouldn't use a global here
        total = 0;
      }

    }
    ```

  - [17.5](#17.5) <a name='17.5'></a> 使用`// TODO:`注释问题的解决方案.

    ```javascript
    class Calculator {

      constructor() {
        // TODO: total should be configurable by an options param
        this.total = 0;
      }

    }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - [18.1](#18.1) <a name='18.1'></a> 两个空格用tab.

    ```javascript
    // bad
    function() {

    ∙∙∙∙const name;

    }

    // bad
    function() {

    ∙const name;

    }

    // good
    function() {

    ∙∙const name;

    }
    ```

  - [18.2](#18.2) <a name='18.2'></a> 在大括号前空一格.

    ```javascript
    // bad
    const test = function(){

      console.log('test');

    }

    // good
    const test = function() {

      console.log('test');

    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [18.3](#18.3) <a name='18.3'></a> 在控制语句(`if`, `while` 等)的圆括号前空一格.在函数调用和定义时,参数列表和函数名之间不空格.

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
    const fight = function () {

      console.log ('Swooosh!');

    }

    // good
    const fight = function() {

      console.log('Swooosh!');

    }
    ```

  - [18.4](#18.4) <a name='18.4'></a> 用空格来隔开运算符.

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name='18.5'></a> 用单个换行符结束文件.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - [18.5](#18.5) <a name='18.5'></a> 当出现长的方法链（>2个)时用缩进.用点开头强调该行是一个方法调用,而不是一个新的语句.

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
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [18.6](#18.6) <a name='18.6'></a> 在块打开之后和块关闭之前留空行

  ```javascript
  // bad
  if (foo) {
    return bar;
  }

  // good
  if (foo) {

    return bar;

  }

  // bad
  const baz = function(foo) {
    return bar;
  }

  // good
  const baz = function(foo) {

    return bar;

  }
  ```

  - [18.7](#18.7) <a name='18.7'></a> 在一个代码块后下一条语句前空一行.

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
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;
    ```


**[⬆ back to top](#table-of-contents)**

## Commas

  - [19.1](#19.1) <a name='19.1'></a> 不要前置逗号.

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [19.2](#19.2) <a name='19.2'></a> 额外结尾逗号: **Yup.**

    > Why? 这导致git diffs更清洁. 此外,像Babel这样的转换器会删除转换代码中的额外的逗号,这意味着你不必担心旧版浏览器中的[ [trailing comma problem](es5/README.md#commas) in legacy browsers.

    ```javascript
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'mordern nursing']
    }

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'mordern nursing'],
    }

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - [20.1](#20.1) <a name='20.1'></a> **Yup.**

    ```javascript
    // bad
    (function() {

      const name = 'Skywalker'
      return name

    })()

    // good
    (() => {

      const name = 'Skywalker';
      return name;

    })();

    // good (行首加分号,避免文件被连接到一起时立即执行函数被当做变量来执行.)
    ;(() => {

      const name = 'Skywalker';
      return name;

    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - [21.1](#21.1) <a name='21.1'></a> 在语句开始执行强制类型转换.
  - [21.2](#21.2) <a name='21.2'></a> Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name='21.3'></a> 使用`parseInt`表示数字,并始终使用基数进行类型转换.

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - [21.4](#21.4) <a name='21.4'></a> 如果出于某种原因您正在做一些疯狂的事情,而" parseInt"是您的瓶颈,并且出于[性能原因]（http://jsperf.com/coercion-vs-casting/3)而需要使用移位运算,请在注释中说明原因以及 你在做什么.

    ```javascript
    // good
    /**
      * parseInt是代码运行慢的原因
     * 用Bitshifting将字符串转成数字使代码运行效率大幅增长
     */
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name='21.5'></a> **注意:** 用移位运算要小心. 数字使用[64-位](https://es5.github.io/#x4.3.19)表示的,但移位运算常常返回的是32为整形[source](https://es5.github.io/#x11.7)).移位运算对大于32位的整数会导致意外行为.[Discussion](https://github.com/airbnb/javascript/issues/109). 最大的32位整数是 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [21.6](#21.6) <a name='21.6'></a> 布尔:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - [22.1](#22.1) <a name='22.1'></a> 避免用一个字母命名,让你的命名可描述.

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

  - [22.2](#22.2) <a name='22.2'></a> 命名对象、函数和实例时,使用小驼峰式.

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    const c = function() {}

    // good
    const thisIsMyObject = {};
    const thisIsMyFunction = function() {}
    ```

  - [22.3](#22.3) <a name='22.3'></a> 命名构造函数、类、模块或接口时,使用大驼峰式.

    ```javascript
    // bad
    function user(options) {

      this.name = options.name;

    }

    const bad = new user({
      name: 'nope',
    });

    // good
    module AperatureScience {

      class User {

        constructor(options) {

          this.name = options.name;

        }

      }

    }

    const good = new AperatureScience.User({
      name: 'yup',
    });
    ```

  - [22.4](#22.4) <a name='22.4'></a> 命名对象属性时,使用snake_case.

    ```javascript
    // bad
    const panda = {
      firstName: 'Mr.',
      LastName: 'Panda'
    }

    // good
    const panda = {
      first_name: 'Mr.',
      Last_name: 'Panda'
    }
    ```

  - [22.5](#22.5) <a name='22.5'></a> 命名私有属性时,请使用下划线" _".

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [22.6](#22.6) <a name='22.6'></a> 不要保存对"this"的引用. 使用箭头函数或Function＃bind.

    ```javascript
    // bad
    function foo() {

      const self = this;
      return function() {

        console.log(self);

      };

    }

    // bad
    function foo() {

      const that = this;
      return function() {

        console.log(that);

      };

    }

    // good
    function foo() {

      return () => {
        console.log(this);
      };

    }
    ```

  - [22.7](#22.7) <a name='22.7'></a> 如果文件导出单个类,则文件名应与该类的名称完全相同.

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - [22.8](#22.8) <a name='22.8'></a> 当你export-default一个函数时,函数名用小驼峰,文件名需要和函数名一致.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [22.9](#22.9) <a name='22.9'></a> 当你export一个结构体/类/单例/函数库/对象 时用大驼峰.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ back to top](#table-of-contents)**


## Accessors

  - [23.1](#23.1) <a name='23.1'></a> 不需要使用属性的访问器函数.
  - [23.2](#23.2) <a name='23.2'></a> 不要使用JavaScript的getters/setters,因为他们会产生副作用,并且难以测试、维护和理解.相反的,你可以用 getVal()和setVal('hello')去创造你自己的accessor函数.

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

  - [23.3](#23.3) <a name='23.3'></a> 如果属性/方法是`boolean`, 用 `isVal()` 或 `hasVal()`.

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

  - [23.4](#23.4) <a name='23.4'></a> 用get()和set()函数是可以的,但是要一起用.

    ```javascript
    class Jedi {

      constructor(options = {}) {

        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);

      }

      set(key, val) {

        this[key] = val;

      }

      get(key) {

        return this[key];

      }

    }
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  - [24.1](#24.1) <a name='24.1'></a> 通过哈希而不是原始值向事件装载数据时(不论是DOM事件还是像Backbone事件的很多属性). 这使得后续的贡献者（程序员)向这个事件装载更多的数据时不用去找或者更新每个处理器.例如:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**


## jQuery

  - [25.1](#25.1) <a name='25.1'></a> jQuery对象用`$`变量表示.

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');
    ```

  - [25.2](#25.2) <a name='25.2'></a> 暂存jQuery查找.

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

      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });

    }
    ```

  - [25.3](#25.3) <a name='25.3'></a> DOM查找用层叠式`$('.sidebar ul')` 或 父节点 > 子节点 `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [25.4](#25.4) <a name='25.4'></a> 用jQuery对象查询作用域的`find`方法查询.

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

**[⬆ back to top](#table-of-contents)**


## Type Annotations

<a name="ts-type-annotations"></a>
  - [26.1](#26.1) <a name='26.1'></a> Type annotations placeholder.

<a name="ts-generics"></a>
  - [26.2](#26.2) <a name='26.2'></a> 如果只需要一个,则使用" T"作为类型变量.

```javascript
function identify<T>(arg: T): T {

    return arg;
    
}
```

  - [26.3](#26.3) <a name='26.3'></a> 如果需要多个类型变量,请以字母"T"开头,并按字母顺序命名变量.

```javascript
function find<T, U extends Findable>(needle: T, haystack: U): U {

  return haystack.find(needle)

}
```

  - [26.4](#26.4) <a name='26.4'></a> 如果可能,允许编译器推断变量的类型.

```javascript
// bad
const output = identify<string>("myString");

// good
const output = identity("myString");
```

  - [26.5](#26.5) <a name='26.5'></a> 使用泛型创建函数时,请确保在类型中包含构造函数.

```javascript
function create<t>(thing: {new(): T;}): T {

  return new thing();

}
```

**[⬆ back to top](#table-of-contents)**


## Interfaces

<a name="ts-interfaces"></a>
  - [27.1](#27.1) <a name='27.1'></a> Interface placeholder.

**[⬆ back to top](#table-of-contents)**


## Organization

<a name="ts-modules"></a>
  - [28.1](#28.1) <a name='28.1'></a> 每个逻辑组件1个文件,每个文件应通过模块划分为逻辑分区. 

  ```javascript
  module Automobile {

    module Honda {

    }

  }
  ```
  
  - [28.2](#28.2) <a name='28.2'></a> 每个文件导出一个主模块,以便其他文件可以使用.

  ```javascript
  module Automobile {

    // 隐藏的模块,将无法通过“ require”访问
    Honda {

    }
  
    // 公共模块,可以通过“ require”访问
    export Ford {

      export function vroom() {

        console.log('vroom!');

      }

    }

  }

  export default Automobile;
  ```

- [28.3](#28.3) <a name='28.3'></a> 在每个模块中按以下顺序对代码进行排序（字母顺序)：
   - var
   - export var
   - let
   - export let
   - const
   - export const
   - interface
   - export interface
   - function
   - export function
   - class
   - export class
   - module
   - export module

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  - [29.1](#29.1) <a name='29.1'></a> Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**

## ECMAScript 6 Styles

  - [30.1](#30.1) <a name='30.1'></a> 这是收集到的各种ES6特性的链接.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[⬆ back to top](#table-of-contents)**

## Typescript 1.5 Styles

  - [31.1](#31.1) <a name='31.1'></a> 这是收集到的各种ES6特性的链接.

1. [Type Annotations](#ts-type-annotations)
1. [Interfaces](#ts-interfaces)
1. [Classes](#ts-classes)
1. [Modules](#ts-modules)
1. [Generics](#ts-generics)

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
