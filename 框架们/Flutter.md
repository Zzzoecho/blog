# Flutter

## 开发

1. 修改 `build_ios.sh` 下的 FLUTTER_ROOT 为本地 Flutter SDK 路径
2. 确认使用 debug 包
3. 运行 App
4. flutter attach



>  安卓如果执行了`flutter package get`, 那么就要进到`andoridx_compat`目录下执行一下 sh



## 使用 debug 包

- ios 更改 `build_ios.sh`中的`BUILD_MODE=debug` 并且 修改 `picku-ios` 下的 `podfile FLUTTER_DEBUG_APP=true` =>  `pod install`
- Android 运行 `build_debug_android.sh`

2020.03.18 目前使用的 flutter 版本: v1.9.1+hotfix.6



## 发布

build_ios.sh `BUILD_MODE` 改为 release
执行 `sh ./build_ios.sh` 会打包发布到 repo, ios 开发拉一下包即可

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

## JSON 序列化

```dart
import 'package:json_annotation/json_annotation.dart';

// user.g.dart 将在我们运行生成命令后自动生成
part 'user.g.dart';

///这个标注是告诉生成器，这个类是需要生成Model类的
@JsonSerializable()
class User{
  User(this.name, this.email);
  //显式关联JSON字段名与Model属性的对应关系
  @JsonKey(name: 'registration_date_millis')
  String name;
  String email;
  //不同的类使用不同的mixin即可
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
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
margin padding: EdgeInsets.only() | .all()
alignment: new FractionalOffset(0.5, 0) // 水平居中

// 获取时间戳
new DateTime.now().millisecondsSinceEpoch

// 色值 透明度
00%=FF（不透明）
5%=F2
10%=E5
15%=D8
20%=CC
25%=BF
30%=B2v
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

```dart
    /**
     * @description: 计算蒙层色值
     * @param {String} colorStr 颜色值 如：0xff000000
     * @return: 返回带透明度的颜色
     */
    calColorOpacity(String colorStr) {
      // colorString未带0xff前缀并且长度为6
      if (colorStr.startsWith('#') && colorStr.length == 9) {
        colorStr = colorStr.replaceRange(0, 1, '0xff');
      }

      // 分别获取色值的RGB通道
      Color color = Color(int.parse(colorStr));
      int red = color.red;
      int green = color.green;
      int blue = color.blue;
      return Color.fromRGBO(red, green, blue, 0.3);
    }
```

### 循环生成 widget

```dart
// 定义一个 widget List
List<Widget> pictureList = [];
for (var i = 0; i < picList.length; i++) {
  pictureList.add()
}
```

### 获取 widget 宽高和 position

注册一个 GlobalKey, 绑定在要查询的元素上. 等待 build 完成, 再去获取 currentContext

```dart
class StoryOwner extends StatefulWidget {
  final GlobalKey globalKey = GlobalKey();
  double offsetY = 0;

}

class _StoryOwnerState extends State<StoryOwner> {
  @override
  void initState() {
    super.initState();
    // 需要在 build 完之后再去获取 context
    WidgetsBinding.instance.addPostFrameCallback(_onBuildCompleted);
  }
  // 括号里的 _ 很关键 不能少
  _onBuildCompleted(_) {
    _getContainerPosition();
  }

  _getContainerPosition() {
    final RenderBox container = widget.globalKey.currentContext.findRenderObject();
    final position = container.localToGlobal(Offset.zero);

    setState(() {
      widget.offsetY = position.dy;
    });
  }
}
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
    resizeToAvoidBottomPadding: false, // 内容不随软键盘弹出而滚动
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
    width: double.maxFinite, //填充
    decoration: BoxDecoration( // 背景装饰 与 color 冲突
        shape: BoxShape.circle, // 圆形
        color: Colors.black,
        border: new Border(
          top: BorderSide(
            color: Color(0xFFAAAAAA),
            style: BorderStyle.solid
          )
        ),

        // 环形渲染
          gradient: RadialGradient(colors: [Color(0xFFFFFF00), Color(0xFF00FF00), Color(0xFF00FFFF)],radius: 1, tileMode: TileMode.mirror)
        //扫描式渐变
          gradient: SweepGradient(colors: [Color(0xFFFFFF00), Color(0xFF00FF00), Color(0xFF00FFFF)], startAngle: 0.0, endAngle: 1*3.14)
        // 线性渐变
          gradient: LinearGradient(colors: [Color(0xFFFFFF00), Color(0xFF00FF00), Color(0xFF00FFFF)], begin: FractionalOffset(1, 0), end: FractionalOffset(0, 1)),

        // Border.all()
        borderRadius: BorderRadius.all(
          Radius.circular(7.0),
        ),
        image: DecorationImage( // 背景图
          image: NetworkImage(data[index].photo.url), // 网络图片
          image: AssetImage('image/xxx.jpeg'), // 本地图片
          fit: BoxFit.contain,
        )
    ),
    child:
)

```

异步加载

```dart
// init 中初始化 future
@override
void initState() {
  super.initState();
  _future = _getData(_start);
}

Future<Map> _getData() async {
  var _storyData = await StoryDao.getStoryByLatest();

  return {};
}

FutureBuilder<Map>(
  future: _future,
  builder: (BuildContext context, AsyncSnapshot<Map> snapshot) {
    switch (snapshot.connectionState) {
      case ConnectionState.none:
        return Container(width: 0, height: 0);
      case ConnectionState.active:
      case ConnectionState.waiting:
        return LoadingWidget();
      case ConnectionState.done:
        if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}');
        }
        Map _homeData = snapshot.data['home'];
        return Container(

        );
    }
    return null; // unreachable
  },
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

Icon [material icon](https://material.io/resources/icons/?style=baseline)

```dart
Icon(Icons.close, color: Colors.black,)
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

可点击

```dart
GestureDetector(
  behavior: HitTestBehavior.opaque, // 响应空白区域的点击事件
  onTap: () {
  },
  child:
)
```

输入框

```dart
new TextField(
  controller: _controller,
  maxLines: null, // 最大行数 null 为无限制, 可自动换行, 1为 不换行
  decoration: new InputDecoration(
    contentPadding: EdgeInsets.all(14.0), // 内部 padding
    border: InputBorder.none, // 无边框
    border: OutlineInputBorder(
      borderRadius: BorderRadius.circular(15), // 圆角
      borderSide: BorderSide.none // 边框
    ),
    fillColor: Colors.white, // 背景色
    filled: true,
    hintText: '成为BGM的理由是…',
  ),
),
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

水平翻转!

```dart
import 'dart:math' as math;
import 'package:vector_math/vector_math_64.dart' as v;
// math.pi 180度
Transform(
  transform: Matrix4.compose(
    v.Vector3(width-40,0,0), // 平移
    v.Quaternion.axisAngle(v.Vector3(0.0,1.0,0.0), math.pi),
    v.Vector3(1.0, 1.0, 1.0)
  ),
  child:
)
```

## canvas

### 弧度制

| 角度 | 弧度 |
| ---- | ---- |
| 0°   | 0    |
| 30°  | π/6  |
| 45°  | π/4  |
| 60°  | π/3  |
| 90°  | π/2  |
| 120° | 2π/3 |
| 180° | π    |
| 270° | 3π/2 |
| 360° | 2π   |

画弧

```dart
addArc(
  Rect.fromCircle(center: Offset(4.0, 4.0), radius: 4.0), // 中点与弧度
  pi, // 起始角度
  pi / 2 // 弧度
)
```

## 我的血泪教训

flutter attach 卡在 waiting connection 那一步, 看看 flutter 页面右上角有没有 debug 标识! 看看 podfile 中 debug 是不是为 true

`const bool inProduction = const bool.fromEnvironment("dart.vm.product");`

安卓打包失败, 看下 `local.properties` 里的本地目录是不是对

```javascript
sdk.dir=/Users/zoe/Library/Android/sdk
flutter.sdk=/Users/zoe/Documents/PickUSpace/flutterSdk/flutter
```
