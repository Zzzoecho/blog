# es6

## Class

语法糖，简化`prototype`的写法。类中的所有方法都定义在`prototye`属性上

```js
class Point {
//类的默认方法，默认返回实例对象(this)
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
```
