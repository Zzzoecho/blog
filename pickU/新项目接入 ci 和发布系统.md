# 新项目接 ci 和发布系统(ops)

## CI

1. ci中 新建item --> 起名 --> 复制以前项目
2. 修改git地址
3. 项目中添加 `jenkinsFile`
4. git项目里配置hooks --> 打开`Post receive`Enabled --> 配置Jenkins URL: `http://ci-new.duitang.net/` --> Repo Clone Url 改为SSH --> 勾选 Omit SHA1 Hash Code
5. 第一次需要手动点击立即构建，后续master有更新会自动打包。
6. ci打包成功后，确认是否上传到[hdfs](hdfs.duitang.net) -> Utilities Browse the file system -> apps/ci/[项目名]

## 发布系统

1. dansible
