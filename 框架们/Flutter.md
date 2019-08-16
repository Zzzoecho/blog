# Flutter

## Dart 语法

声明:
var 自动判断数据类型，但赋值后不可改
dynamic 数据类型可变 (不要滥用，在变量无法用 Dart 的类型表示时用，比如 Native 和 Flutter 交互，从 Native 传来的数据。
Object 可定义任何变量，可更改类型
常量 final const (使用 const 时，变量是类里的变量，必须加 static ，是全局变量时不需要加)
支持的数据类型
int double num String Bool List Set Map Runes
函数

```js
返回类型 函数名(函数参数){
}
// 可选参数 { key: value}
// 必填参数
bool say(String msg , String from, int clock){
  print(msg+" from " + from + " at " + clock?.toString());
  return true;
}
```

---

## 常用代码片段

- stless StatelessWidget 无状态
- stfull StatefulWidget 有状态 实现步骤: 首先继承 StatefulWidget --> 实现 createState() 的方法，返回一个 State
- stanim StatefulWidget with AnimationController

State 的实现步骤：
首先继承 State，State 的泛型类型是上面定义的 Widget 的类型
实现 build() 的方法，返回一个 Widget
需要更改数据，刷新 UI 的话，调用 setState()

增加自己自定义的代码片段
在控制台输入 Configure User Snippets / 首选项:配置用户代码片段
选择 dart.json
编写自己的代码片段

```dart
import 'package:flutter/rendering.dart';
debugPaintSizeEnabled = true; // 页面样式调试 main 方法中加入

// 获取屏幕宽度
double width = MediaQuery.of(context).size.width;

// 常用属性
margin padding: EdgeInsets.only() | all
alignment: new FractionalOffset(0.5, 0) // 水平居中

// 色值 透明度
00%=FF（不透明）
5%=F2
10%=E5
15%=D8
20%=CC
25%=BF
30%=B2
35%=A5
40%=99
45%=8c
50%=7F
55%=72
60%=66
65%=59
70%=4c
75%=3F
80%=33
85%=21
90%=19
95%=0c
100%=00（全透明）

```

---

### 常用 Widget

Scaffold

```dart
Scaffold(
    appBar: new AppBar(
      title: _getTitle(),
      actions: <Widget>[]
    ), // 标题
    body: ,//主屏幕
    drawer: ,//抽屉
    bottomNavigationBar: new BottomNavigationBar(
        onTap: ,
        currentIndex:,
        type: ,
        iconSize: ,
        items:
    ),// Tab栏
),
```

Container

```dart
Container(
    decoration: BoxDecoration( // 背景装饰 与 color 冲突
        shape: BoxShape.circle, // 圆形
        color: Colors.black,
        borderRadius: BorderRadius.all(
            Radius.circular(7.0),
        ),
        image: DecorationImage( // 背景图
            image: NetworkImage(data[index].photo.url), // 网络图片
            fit: BoxFit.contain,
        )
    ),
    child:
)

```

Text

```dart
Text(
      'xxx',
      style: TextStyle(
          background: Paint()..color = Colors.black,
          color: Colors.xxx,
          fontSize, fontWeight[FontWeight],
  ),
)
```

RichText

```dart
RichText(
    text: TextSpan(
        children: [TextSpan(text: '', style: TextStyle())]
    )
)
```

ListView

```dart
ListView.builder(
    padding: EdgeInsets.all | only,
    itemCount: 1,
    itemBuilder: (BuildContext context, int index) { return }
)
```

Flex

```dart
Flex(
    direction: Axis.horizontal | vertical,
    mainAxisAlignment...,
    children: <widget>[
        Expanded(flex: int，child),
        Flexible(flex: int,child)
    ]
)
```

Row Column

```dart
Row(
    mainAxisAlignment: MainAxisAlignment.spaceBetween,
    crossAxisAlignment: CrossAxisAlignment.end,
    children:
)
```

旋转

```dart
RotationTransition(
    turns: new AlwaysStoppedAnimation(15 / 360),
    child: new Text("Lorem ipsum"),
)

Transform.rotate(
    angle: - math.pi / 4,
    child:
)
// 优点是作用于layout阶段
RotatedBox(
        quarterTurns: 1, // 最小角度90度 90 180 270 。。。
        child:
)
```

// TODO: mock token
// String authorizationCode = 'eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiI4Nl8xMzc2MTg4NDc0NiIsImF1dGgiOiJST0xFX1BJQ0tFUixST0xFX1VTRVIsUk9MRV9BRE1JTiIsImV4cCI6MTU3MjkzOTk5NywiaWF0IjoxNTY1MTYzOTk3LCJqdGkiOiIxMDAwMjAifQ.xlvsbCcVx95nMyQNz6OY_xZZOu9xONchXOKpJ80fSf3rQgKJKy_SaLgTce6M8ws2P9xsDDJpmMei0PF5TK72sQ';