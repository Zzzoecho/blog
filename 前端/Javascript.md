# Javascript

## 基本类型和引用类型

基本类型(值类型): Number, boolean, string, null, undefined
引用类型: object, array, function



基本类型

> 相当于互相独立, 互不影响的两个副本

1. 一但创建就不可更改
2. 不能添加属性和方法
3. 直接将值存在栈中, 占用的内存空间大小固定. 并由系统自动分配和释放
4. 比较时, 比的是值, == 时会做类型转换



引用类型

> 相当于拥有同一个房间钥匙的管理员

1. 值可变
2. 可以拥有属性和方法, 并且可以动态改变
3. 栈: 保存变量标识符和指向堆内存的指针, 堆: 实际的数据
4. 比较时, 比的是引用地址



Javascript 不允许直接访问内存中的位置, 也就是说不能直接操作对象的内存空间

JS 只能通过**指针**(保存在栈中的变量)去操作堆内存中的引用类型的值即对象，如果给变量赋值另一个保存引用类型的值的变量，实际上只是创建了一个新指针，指向同一个堆内存中的对象



**为什么给对象添加的方法能用在基本类型上 ?**

Number, String, Boolean, Symbol 在对象中都有一个亲戚
例: `3`--Number 类型 `new Number(3)`--对象类型

`'abc'.charAt(0)`为什么能运行, .运算符提供了装箱操作, 会根据基础类型构造一个临时对象, 让我们能在基础类型上调用对应对象的方法



## 内存空间

栈内存

- 存储的值大小固定
- 空间较小
- 可以直接操作其保存的变量, 运行效率高
- 由系统自动分配存储空间

堆内存

- 存储的值大小不定, 可动态调整
- 空间较大, 运行效率低
- 无法直接操作其内部存储, 使用引用地址读取
- 通过代码进行分配空间

### 装箱 拆箱

> 装箱转换: 把基础类型转换为对应的对象. 装箱会频繁产生临时对象, 对性能要求较高的场景下, 应该尽量避免
> Symbol 函数直接用 new 调用会报错, 可以利用函数的 call 方法来强迫产生 装箱.

```js
var symbolObject = function() {
  return this
}.call(Symbol('a'))

console.log(typeof symbolObject) //object
console.log(symbolObject instanceof Symbol) //true
console.log(symbolObject.constructor == Symbol) //true
```

`Object.prototype.toString` 可以准确识别对象对应的基本类型, 比`instanceof`更准确

> 拆箱转换: ToPrimitive 函数, 对象类型转换为基础类型

对象到 String 和 Number 的转换都遵循"先拆箱再转换"的规则. 对象 -> 基本类型 -> 对应的 String 或 Number

#### ToPrimitive

执行时, 会被传递一个参数 hint, 表明运算的期望值.

hint 参数的取值

- string
- number
- default

```js
// 一个没有提供 Symbol.toPrimitive 属性的对象，参与运算时的输出结果
var obj1 = {}
console.log(+obj1) // NaN
console.log(`${obj1}`) // "[object Object]"
console.log(obj1 + '') // "[object Object]"

// 接下面声明一个对象，手动赋予了 Symbol.toPrimitive 属性，再来查看输出结果
var obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint == 'number') {
      return 10
    }
    if (hint == 'string') {
      return 'hello'
    }
    return true
  }
}
console.log(+obj2) // 10      -- hint 参数值是 "number"
console.log(`${obj2}`) // "hello" -- hint 参数值是 "string"
console.log(obj2 + '') // "true"  -- hint 参数值是 "default"
```

#### 检测类型

> typeof: 返回未经计算的操作数的类型。
> 变量是基本类型 -- 返回基本类型, null 除外
> 变量是引用类型 -- 返回`object`, function 返回 function

| 类型      | 结果      |
| --------- | --------- |
| Number    | number    |
| String    | string    |
| Boolean   | boolean   |
| symbol    | symbol    |
| undefined | undefined |
| null      | object    |
| object    | object    |
| array     | array     |
| function  | function  |

```js
typeof NaN === 'number'
typeof new Date() === 'object'
typeof /regex/ === 'object' // 历史结果请参阅正则表达式部分
typeof new Boolean(true) === 'object'
typeof new Number(1) === 'object'
typeof new String('abc') === 'object'
typeof null === 'object'
```

> instanceof 操作符: 主要用来检查构造函数的原型是否在对象的原型链上。
> 语法: `实例对象 instanceof 构造函数`

没有经过装箱操作的基本类型检测不出来, 有装箱操作的类型可以检测出多层继承关系

> constructor: 不能判断 null 和 undefined, 并且指向可以改变, 不安全
>
> Object.prototype.toString.call()

<img src="https://user-gold-cdn.xitu.io/2019/5/28/16afa4ee855cfa98" alt="16afa4ee855cfa98 (1038×936)" style="zoom:50%;" />



jquery 源码类型判断

```js
var class2type = {};
jQuery.each( "Boolean Number String Function Array Date RegExp Object Error Symbol".split( " " ),
// 去除多余字符串
function( i, name ) {
  class2type[ "[object " + name + "]" ] = name.toLowerCase();
} );

type: function( obj ) {
  if ( obj == null ) {
    return obj + "";
  }
  return typeof obj === "object" || typeof obj === "function" ?
    // 引用类型
    class2type[Object.prototype.toString.call(obj) ] || "object" :
    // 原始类型直接 typeof
    typeof obj;
}

isFunction: function( obj ) {
  return jQuery.type(obj) === "function";
}

```

---

### 数据类型

简单数据类型:

1. Undefined
2. Null
3. String
4. Number
5. Symbol
6. Boolean
7. Object (复杂数据类型)
8. BigInt

Function 是 Object 的子类, 继承自 Object

#### Undefined

> 未定义
> 和 not defined 的区别:

同: 使用`typeof`操作符都会返回`undefined`
异: 使用`undefined`变量是安全的, 已经声明但未赋值. 使用`not defined`变量会报错

**void 0 与 undefined**
Undefined 在 js 中不是关键字, 是 js 公认的设计失误之一.
为了避免被篡改用 void 运算符来把任意一个表达式变成 undefined .用 void 0 只是因为 0 最短, 用 void (1+1), void 'hello'都是可以的

tips: 用 void 0 代替 undefined 能节省不少字节, 许多 js 压缩工具在压缩过程中就将 undefined 用 void 0 代替了

虽然在 ES5 中已经是全局对象的一个只读属性了(不能被重写),但在局部作用域中还是可以被重写.

#### Null

> 定义了但是为空

`typeof` 操作符返回`Object` 是 JS 的 一个悠久BUG, 在 JS 最初版本中用的是32位系统, 为了性能考虑使用低位存储变量的类型信息, 000开头代表是对象, 然而 null 表示为全0, 所以 null 被错误地判断为 Object

undefined 值是派生自 null 值的，所以它们宽松相等`undefined == null`

#### Number

非整数的 Number 不能用 == 或 === 来比较, 浮点数运算的精度问题导致等号两边不严格相等

主要原因是数字转化为二进制后变成了一个无限循环的数字, 计算机进行了舍入处理,结果转换成十进制时就会有误差.
**`0.1+0.2 != 0.3`**

- NaN 属于 Number 类型 且 NaN 不等于自身
- window.isNaN 判断参数是不是数字, 但会先做隐式转换, 将参数转为 Number 类型.所以`isNaN('foo')`返回 `true`
- Number.isNaN 判断参数是不是 NaN
- `parseInt` 和 `Number` 区别: 前者逐个字符串解析, 后者直接转换

```js
console.log(NaN === NaN) // false

console.log(isNaN(NaN)) // true
console.log(isNaN('foo')) // true 但这是不合理的，因为 'foo' 并不是 NaN
console.log(Number.isNaN('foo')) // false

console.log(parseInt('123foo')) // 123
console.log(Number('123foo')) // NaN
```

#### String

> 字符串的 UTF16 编码
> 最大长度是 2^53-1, 长度受字符串的编码长度影响

字符串一旦创建就不可改变

```javascript
var str1 = 'test' // 创建了一片内存空间把 test 放进去了, str实际是指针地址 指向内存空间
var str2 = str1 // 把指针赋值给了 str2
str1 = str1 + 'hello' // 分配了新的内存空间放 str+'hello', 更改了 str 的指针
console.log(str2) // test str1 和 str2 互不影响
```

#### Symbol

特性:

- 独一无二, 即使用两个相同的字符串创建 `Symbol`, 他们也是不相等的, 传字符串主要是为了便于区分.  可以用`Symbol.for(key)`找到相等的`symbol`
- 是原始类型, 用 new 会直接报错, 用`Symbol()`函数创建
- 不可枚举, 用 for...in , `Object.getOwnPrepertyNames`, `Object.keys(obj)` 不会出现 symbol 属性, 可以用`Object.getOwnPropertySymbols()`专门获取 Symbol 属性
- instanceof 的结果是 false
- 如果参数是一个对象, 会先调用对象的 `toString` 方法, 再生成 Symbol
- `Symbol.keyFor(symbol)` 返回 symbol  的key

应用场景:

1. 防止 XSS: 在 React 的 ReactElement 对象中，有一个`$$typeof`属性，它是一个 Symbol 类型的变量, React 渲染时会把没有 `$$typeof` 标识的组件过滤掉,
2. 私有属性: 可以在类中模拟私有属性
3. 防止属性污染: 为对象添加新属性时有可能将原有属性覆盖, 用 Symbol 作为属性名可以保证永远不会出现同名属性

> `Symbol.for(key)`: 使用给定的 key 搜索现有的 symbol，如果找到则返回该 symbol。否则将使用给定的 key 在全局 symbol 注册表中创建一个新的 symbol。

#### Object

属性的集合.  分 数据属性 和 访问器属性

**数据属性**

- writable:  value 是否可以被改写
- enumerabel: 对象属性是否可枚举
- value: 默认值
- configurable: 如果该值为 `false`，则该属性不能被删除，并且 除了 [[Value]] 和 [[Writable]] 以外的特性都不能被改变。

**访问器属性**

- get
- Set



Object的键名只能是字符串和Symbol类型

对象键名的转换: 其他类型会转换成字符串类型; 对象会调用 toString 方法

类数组(伪数组): 对象有length属性, 且值是正整数 & 有splice属性, 且值是函数

---

## 原型和原型链



![关系图](/Users/zoe/Desktop/FrontEnd/docs/前端/image-20200408183202743.png)

## this

### call, apply, bind

> 三者都是用于改变函数执行时的上下文, 既 this 的指向.

不同:

- call 和 apply 会直接调用函数, bind 会返回绑定了作用域的新函数
- call 和 apply 的参数不同, `fn.call(this, param1, param2, ...)` `fn.apply(this, [param1, param2, ...])`

call的性能更好

应用

- 将伪数组转化为数组, 如 `document.querySelectorAll()`获取的元素, 有 length 属性并且可以通过下标访问. 但没有 push, pop 等数组的方法

```js
let domList = document.getElementsByTagName('div')
let arr = Array.prototype.slice.call(domList)
```

## DOM 与 BOM

> DOM(Document Object Model) 是 HTML 和 XML 文档的编程接口。它给文档（结构树）提供了一个结构化的表述并且定义了一种方式—程序可以对结构树进行访问，以改变文档的结构，样式和内容。在 DOM 中，文档中的各个组件（component），可以通过 object.attribute 这种形式来访问。一个 DOM 会有一个根对象，这个对象通常就是 document。
>
> BOM(Browser Object Model) 是指浏览器对象模型，是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM 由多个对象组成，其中代表浏览器窗口的 Window 对象是 BOM 的顶层对象，其他对象都是该对象的子对象。

### 操作符

#### 一元操作符

后置递增/递减 和 前置递增/递减的重要区别: 执行顺序

```js
let num1 = 2
let num2 = 20
// 先 num - 1 再计算和
let num3 = --num1 + num2 // 21
let num4 = num1 + num2 // 21

let num1 = 2
let num2 = 20
// 先计算和 再 num - 1
let num3 = num1-- + num2 // 22
let num4 = num1 + num2 // 此时 num = 1 21
```

#### 布尔操作符

假值: `false`, `null`, `undefined`, `0`, `NaN`, ''

#### 加性操作符

- 涉及到 NaN 的四则运算结果都是 NaN
- 有字符串, 则字符串拼接
- 两边都没有字符串, 则
  1. 对象, 调用[toPrimitive](###ToPrimitive)转为原始值, 如果原始值是字符串则字符串拼接
  2. 不是对象, 转为 Number 类型再计算

```js
console.log('123' + 123) // "123123"
console.log('123' + NaN) // "123NaN"
console.log('123' + {}) // "123[object Object]"
console.log('123' + undefined) // "123undefined"

console.log(123 + true) // 124
console.log(123 + undefined) // NaN 因为 undefined 被转为 NaN
console.log(NaN + {}) // "NaN[object Object]" 含有对象会转为原始值，因为是字符串所以视为拼接
```

### 乘性操作符

- 如果有一边不是数值, 会调用Number()转为数值

#### 关系操作符

- 两边都是数值, 数值比较. 如果有一个是 NaN 则始终返回 NaN
- 两边都是字符串, 逐个比较字符串的编码值
- 其中一个是对象, [toPrimitive](###ToPrimitive)转为原始值
- 其中一个是布尔值, 转为 Number 类型, 比较

#### 相等操作符

- 如果类型相同, 直接判断.不同类型才会发生隐式转换
- 涉及到对象, 会调用`toPrimitive`
- NaN 和任何都不相等, 包括自身
- 等式两边都会尽可能转为 Number 类型，如果在转为数字的途中，已经是同一类型则不会进一步转换
- null 和 undefined 宽松相等, 除此之外任何值都不会和他俩宽松 / 严格相等

### 流程控制语句

#### for in 语句

返回属性的顺序可能会因浏览器而异, 没有规范, 不要依赖他的返回顺序

Reflect.ownKeys ，Object.getOwnPropertyNames，Object.getOwnPropertySymbols 是由 ES6 规范 [[OwnPropertyKeys]] 算法定义的:

- 首先顺序返回整数的属性(数组的属性)
- 依次按照创建顺序返回字符串属性
- 最后返回所有符号属性

#### label

多用于多层循环嵌套, 可以指定标签来退出更外层的循环

#### switch 语句

switch 语句中 case 的判断条件是严格相等

### 函数

函数的参数被保存在一个叫`arguments` 的类数组对象中, length 代表执行函数时传入的参数个数, 而不是函数定义时的参数个数

非严格模式下, 当参数被修改时会反映到 arguments 上. 严格模式不会建立这种链接

虽然现在仍可以使用, 但 ES6 已经将他废弃, 推荐使用剩余运算符(...)

---

## 执行机制

> Javascript 是单线程语言, Event Loop 是 Javascript 的执行机制

事件循环:

1. 同步进入主线程 异步进入 Event Table 并注册回调函数, 等待完成回调移入 Event Queue
2. 主线程内的任务执行完毕(monitoring process 进程, 持续检查主线程执行栈是否为空), 去 Event Queue 读取对应的函数, 进入主线程执行

宏任务(macro-task): setTimeout, setInterval
微任务(micro-task): promise, process.nextTick

## 模块化

1. IIFE: 立即执行函数
2. CDN-Based: `<script>`引入. 弊端: 污染全局作用域; 文件需按顺序载入; 需要开发者自行判断模组和函数库之间的依赖关系
3. AMD (RequireJS): define() 依赖必须提前声明
4. CommonJS: `require` 载入, `export`, `module.export`输出, Node.js自带 和 浏览器 Browserify
5. CMD: Sea.js, 支持动态引入依赖文件
6. UMD: 兼容AMD, CommonJs
7. ES6 Module:
