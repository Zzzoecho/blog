# WebStorm

## 快捷键

- 删除光标选中行 command + Delete
- 往后选中多个相同字符串 ctrl + G
- 代码片段 command + J 设置 Editor -> File and Code Templates -> 添加自定义片段
- 复制历史记录 shift + command + v

## 配置

Unused default export 右下角小人 -> configure inspection ->  取消勾选 javascript Unused global symbol

## 报错

### 1. Vue 引入组件别名路径错误

`Module is not installed`

For Vue-cli 3.x, you have to specify node_modules/@vue/cli-service/webpack.config.js as Preferences | Languages & Frameworks | JavaScript | Webpack value

### 2. sass 自定义变量报错

`Element  (variable name) is resolved only by name`

Editor -> Inspections in the left panel
Sass/SCSS in the right panel
uncheck Resolved by name only
