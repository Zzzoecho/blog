# 堆糖Config

## info

### 用户id

雪糕 -- 10379401
雪糕：18351013611 - 725611
波波： bali107 - duitang100424 id：563600
一二呦 123456 id: 15040027

### 测试环境

大熊 -- id：2369
复古米米(加v用户) - 233432

### blog状态

status 5 - 删除 6 - 屏蔽 0 - 正常 7 - 待审

## vienna-op

环境: node 7.6.0
开发流程:

1. cd crow
2. npm run pc // 监听 js 文件改动
3. gulp watch // 监听 css 文件改动

打包流程:

npm run build // webpack 打包 输出到 static 目录, babel 转义, chunk 拼接, scss 转 css, postCss 生成 css hack 等

gulp build

1. clean rev dist cdn build 目录
2. 打包 'vienna','mdt','pgc','guide2'
3. gulp-rev-all 生成 hash 号
4. copy cdn 和 rev
5. 提取字体 img 和 sakura 公共资源至 build/vienna-op_static 目录
6. 清除 dist rev cdn 目录

### 路由对照

#### 文章

- PC `https://www.duitang.com/article/?id=374852`
- h5 `https://www.duitang.com/life_artist/article/?id=374852`
- 原生 `duitang://www.duitang.com/life_artist/article/?id=374852`

#### atlas

- PC `https://www.duitang.com/p/atlas/?id=374852`
- h5 `https://www.duitang.com/atlas/?id=374852`
- 原生 `duitang://www.duitang.com/atlas/detail/?id=374852`

---

## sakura

线上调试 CMS 页面: Sakura映射到本地，创建预发布的测试页面 -> 配置在雪糕的秘密基地 -> 就可以charles到本地直接改模块代码调试了

## doctor

创建新页面

1. 决定路由 创建同名
2. app.js 注册控制器 监听路由
3. template 添加js文件打包

---

## Scram

### 后端

日志都记录在根目录下 duitang/logs/usr 里
10.1.2.199 --> 10.1.2.51 --> docker ps --> docker logs -f {spiderserver id}
日常重启命令  -- `sh ./bin/docker_restart.sh`
日常更新然后重启 应该会502一小会

```bash
cd /duitang/dist/app/main/scram
git pull
sh ./bin/docker_restart.sh
```

命令在bin文件夹下

### bay

npm run build  && ./bin/deploy.sh

### TODO

- [ ] 首页进入页面时自动加载的数据 不是默认bilibili
- [ ] 编辑发布内容时 限制图片数量9
- [ ] 请求统一入口 错误处理
- [ ] 表格 传表头 & 内容
- [ ] 编辑文章时检查登录态 并 提示登录堆糖生活家账号

---

## athena-op

- siberia发布时是发布athena-op-release
- 新页面：js/containers -- 创建页面 写入依赖（React,redux,fetchRegiserStatusActions) -- 在containers/RootRoutes中注册，记得import你的入口文件
- redux/reducers -- 创建新js文件，并在index中注册props名
- constants -- 常量 ApiServer -- 请求路由 ActionTypes -- 数据获取

---
手机测试：banner后台随便找一个charles到本地的nav文件 -- nav文件的a链接修改你需要的链接 -- ok