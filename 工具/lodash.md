# lodash

## 常用方法

| 方法                                   | 作用                                          |
| -------------------------------------- | --------------------------------------------- |
| _.pick(obj, ['xx', 'bb'])              | 从对象中挑出对应属性 返回新对象               |
| _.omit(obj, ['xx'])                    | 与pick相反，从对象中剔除对应属性 返回新对象   |
| _.map()                                ||
| _.cloneDeep()                          |深拷贝|
| _.debounce()                           ||
| _.get(obj, '')                         | 获取对象中指定path的值                        |
| _.has(obj, 'xx' / a.b / [a,b])         | 判断对象层级中是否有该属性 返回布尔值         |
| _.hasIn(obj, path)                     | 同.has 但包括继承属性                         |
| _.indexOf(array, value, [fromIndex=0]) | 返回找到的index值，没找到返回-1               |
| _.filter()                             |
| _.reject(collection, ['xx', xx])       | 与filter 返回未通过校验的值                   |
| _.keys                                 | 返回object中的所有key                         |
| _.find                                 | 查找数组 未找到会返回undefined                |
| _.sum(arr)                             | 求和                                          |
| _.sumBy(arr)                           | 根据数组属性求和                              |
| _.sortBy(arr,fn(item))                 | 排序                                          |
| _.pickBy(params, _.identity)           | 过滤一切不为真的值                            |
| _.compact()                            | 过滤假值false, null, 0, "", undefined, 和 NaN |
| _.get(fruit, 'name', 'unknown')        | 获取属性，如果不存在 则给一个默认值 unknown   |



