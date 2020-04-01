# ReactNative

## 添加 modules

侧边栏右键 new -> module -> import -> 选择 RN node_module 下对应包的 android 文件夹 -> Finish
`PkReactPackage` 注册 NativeModules (第三方 & 自定义)

## 调试

真机

192.168.160.104:10110

1. 连 usb, adb devices 确认连接正常
2. adb reverse tcp:8081 tcp:8081
3. npx react-native run-android

模拟器

1. 打开模拟器, 运行 npx react-native run-android
2. 连接电脑 cmd+m -> setting -> dev host(127.0.0.1:8081) -> 确认

## 语法

flex 布局默认竖轴是主轴

### shadow

shadowColor —–阴影的颜色，颜色色值以 rgba 取色值
shadowOffset —-阴影的偏移量,{width:0, height:2}
shadowOpacity —设置阴影的不透明度
shadowRadius —-设置阴影的模糊半径

## 不同平台的代码

## 静态资源

RN 中不会由于图片加载滞后, 而出现页面由于图片突然出现的高度而跳动的问题.

可自动选择不同平台或不同像素比的图片
`xxx.ios.png / xxx.android.png / xxx@2x.png` 引入时统一都是 `<Image source={require('./xxx.png')} />`

因为这个特性, 图片引用时必须是静态的字符串.
`require('./' + icon + '.png')` 这样是不行的

会自动根据图片大小填 width 和 height, 如果想要图片自适应, 需要手动指定 `width / height: undefined`

视频不能使用 flex, 要用绝对定位. 因为非图片的静态资源没有指定宽高

混合开发中使用图片

1. 使用`Xcode asset` or `Android drawable` 目录下的图片, 不用带扩展名 `<Image source={{uri: 'xxx'}} />`
2. `Android assets` 目录下的, 加上 `asset:/` 的前缀 `<Image source={{uri: 'asset:/xxx.png'}} />`

网络图片, 指定宽高

## Text 宽度自适应

父元素加上 `alignSelf:'flex-start'`

加入剧本的 api

```
curl -X "POST" "http://pluto.duitang.net/playbooks/users/joins/" \
     -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiI4Nl8xNzc4ODg4ODg4OCIsImF1dGgiOiJST0xFX1NFUlZJQ0UsUEVSTUlTU0lPTl9NT0RJRllfVVNFUl9ST0xFLFJPTEVfVVNFUixST0xFX1VTRVJfQURNSU4sUEVSTUlTU0lPTl9PUEVSQVRFX1JCQUMsUk9MRV9BRE1JTixST0xFX1NVUEVSX0FETUlOIiwiZXhwIjoxNTkwMDcyMjYxLCJpYXQiOjE1ODIyOTYyNjEsImp0aSI6IjEwMDAyMCJ9.QSEJcIyh0lx_KuySaq-lJomMhFARwOsOKEHviXnL14wSmq4ewfefcH9TA4lUiO0xysRi0ixe7m1t32gUI6ZJDQ' \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "batchNo": "0",
  "playbookId": "5e4d5043405dd9b040b52483"
}'
```

## 获取屏幕宽高

```js
import { Dimensions } from 'react-native

const {width, height, scale} = Dimensions.get('window');
```

## 坑

Touchable 系列组件, 如果点击无效, 看看组件有没有超出父元素区域

## codePush

```bash
code-push app add <appName> <os> <platform>
```

### PickYou-Android

Production: XH3rDnSoL9Cc3cCnWV73PW4ifsLZkkMll1t1qo
Staging: yZT11rgW2MZf1ntp5-JZ36H_D4HXT0HtwWLEg

### PickYou-Ios

Production: iI9a5gaYpCE_UzxOdg0xAhcs\_\_tF6Wm2c8jUWo
Staging: y-tmvF4odd-yPUfznIZsmFDRW11ZCBDC6AVnn
