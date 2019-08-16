# Javascript

## 基本概念

### 数据类型

简单数据类型:

1. Undefined
2. Null
3. String
4. Number
5. Symbol
6. Boolean
7. Object (复杂数据类型)

Function 是 Object 的子类, 继承自 Object

#### Undefined

和 not defined 的区别:

同: 使用`typeof`操作符都会返回`undefined`
异: 使用`undefined`变量是安全的, 已经声明但未赋值. 使用`not defined`变量会报错

#### Null

`typeof` 操作符返回`Object` 语言上的 BUG

undefined 值是派生自 null 值的，所以它们宽松相等`undefined == null`

#### Number

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

字符串一旦创建就不可改变

## DOM 与 BOM

> DOM(Document Object Model) 是HTML和XML文档的编程接口。它给文档（结构树）提供了一个结构化的表述并且定义了一种方式—程序可以对结构树进行访问，以改变文档的结构，样式和内容。在 DOM 中，文档中的各个组件（component），可以通过 object.attribute 这种形式来访问。一个 DOM 会有一个根对象，这个对象通常就是 document。

> BOM(Browser Object Model) 是指浏览器对象模型，是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。

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
console.log("123" + 123) // "123123"
console.log('123' + NaN) // "123NaN"
console.log("123" + {}) // "123[object Object]"
console.log("123" + undefined) // "123undefined"

console.log(123 + true) // 124
console.log(123 + undefined) // NaN 因为 undefined 被转为 NaN
console.log(NaN + {}) // "NaN[object Object]" 含有对象会转为原始值，因为是字符串所以视为拼接
```

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

------

## 内置对象

### ToPrimitive

> JS 对象转换到原始值

执行时, 会被传递一个参数 hint, 表明运算的期望值.

hint 参数的取值

- string
- number
- default

```js
// 一个没有提供 Symbol.toPrimitive 属性的对象，参与运算时的输出结果
var obj1 = {};
console.log(+obj1);     // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ""); // "[object Object]"

// 接下面声明一个对象，手动赋予了 Symbol.toPrimitive 属性，再来查看输出结果
var obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint == "number") {
      return 10;
    }
    if (hint == "string") {
      return "hello";
    }
    return true;
  }
};
console.log(+obj2);     // 10      -- hint 参数值是 "number"
console.log(`${obj2}`); // "hello" -- hint 参数值是 "string"
console.log(obj2 + ""); // "true"  -- hint 参数值是 "default"
```

