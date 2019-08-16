# npm

## 指定版本号

指定版本：比如1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。

~ +指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，也就是说安装时不改变大版本号和次要版本号。

^+指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。

latest：安装最新版本。

## 切换淘宝源

`npm config set registry https://registry.npm.taobao.org/`

## 发布 npm 包

```js
// 先登录NPM账号：
npm login

// 会依次让你输入用户名、密码、和邮箱
Username: Zzzoe
Password: *****
Email: (this IS public) 531575302@qq.com

// 登录成功会出现以下提示信息：
Logged in as Zzzoe on https://registry.npmjs.org/.

#执行发布命令：
npm publish

// 发布成功后会出现以下提示信息：
+ react-components-calendar@0.0.7
// 这里react-components-calendar是我的NPM包名，0.0.7是包的版本号
```
