# Angular

ng-repeat 循环生成会创建子作用域，内部绑定时要用`$parent`

```js
<label ng-repeat="n in [0,1,2,3]">
     <input type="radio" name="pageNumber" ng-model="$parent.pageNumber" ng-value="n" /> {{n}}
 </label>
```
