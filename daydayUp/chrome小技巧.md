# chrome 小技巧

## $

`$0-4` -- 控制台会存储最近5次被选择的元素和对象, $index 来获得选中的元素
`$_` -- 获得控制台最近一次的输出结果
`$()` -- document.querySelector()的简化
`$$()` -- querySelectorAll()的简化
`$x()` -- 返回满足指定 XPath 的所有元素 `$x('html/body/div)`

## console

console.dir
console.table

## 其他

- copy()
- keys(object) / values(object)
- 函数监听器 `monitor(fn) / unmonitor(fn)` 定义函数后, 监听指定函数. 当函数被调用时, 会输出调用的函数的函数名和参数
-
