# Git

| ohmyzsh git                            | 缩写   |
| -------------------------------------- | ------ |
| git status                             | gst    |
| git add                                | ga     |
| git add --all                          | gaa    |
| git commit -a -m                       | gcam   |
| git commit -m                          | gcmsg  |
| git branch                             | gb     |
| git branch -d                          | gbd    |
| git branch -D                          | gbD    |
| git diff                               | gf     |
| git checkout                           | gco    |
| git checkout master                    | gcm    |
| git checkout -b                        | gcb    |
| git checkout -t origin/xxx             | gcb    |
| git add                                | ga     |
| git fetch origin                       | gfo    |
| git push                               | gp     |
| git push origin \$(git_current_branch) | ggpush |
| git pull origin \$(git_current_branch) | ggpull |
| git reset --hard [commit-hashcode]     |        |
| git remote -v                          | grv    |
| git remote add                         | gra    |
| git remote rename                      | grmv   |
| git remote remove                      | grrm   |

---

| 常用命令                                           | 意义                                             |
| -------------------------------------------------- | ------------------------------------------------ |
| git stash                                          | 存入储藏不会 commit                              |
| git stash list                                     | 储藏列表                                         |
| git stash apply [stash@{idx}]                      | 应用储藏                                         |
| git stash drop [stash@{idx}]                       | 移除储藏                                         |
| git remote -v                                      |
| git remote add [name][url]                         |
| git remote rm [name]                               |
| git pull origin master --allow-unrelated-histories | 远程仓库有不同的 history                         |
| `git log [文件路径]`                               | 查看单个文件修改历史                             |
| `git checkout [hash] [文件路径]`                   | 回退单个文件                                     |
| git branch -m <old branch > <new branch>           | 重命名分支                                       |
| `git rm -r --cached [文件路径]`                    | 删除, --cached 只删除 git 仓库上的文件, 本地还在 |

## tag 命令

| git                             | 含义                    |
| ------------------------------- | ----------------------- |
| git tag                         | 查看现有标签            |
| git tag -l 'v1.4.\*'            | 查看 1.4 系列的标签     |
| git tag -a v0.1 -m 'desc'       | 新建含附注的标签        |
| git tag v1.0                    | 新建轻量级标签          |
| git tag -d v0.1                 | 本地删除指定标签        |
| git push origin :refs/tags/v0.9 | 删除远程指定标签        |
| git show v1.0                   | 查看指定标签            |
| git tag -a v1.0 9fceb02         | 后期在 9fceb02 加上标签 |
| git push origin v1.5            | 推送指定 tag            |
| git push origin --tags          | 推送所有 tag            |

---

## submodule

添加
`git submodule add <url> <path>`

url -- 子模块的路径
path -- 子模块存储的目录

添加成功后项目会多一个`.gitmodules` 文件, 该文件列出了子模块列表, 并且指定了本地路径和远程仓库地址, 还可以在此为子模块配置指定分支, 默认 master

使用

```bash
git submodule init
git submodule update

or

git submodule update --init --recursive
```

删除

```bash
git submodule deinit [moduleName]
vim .gitmodules # 移除要删除的子模块
git add .gitmodules
git rm --cached [moduleName]
rm -rf .git/modules/[moduleName]
rm -rf [moduleName]
git commit -m "Remove submodule [moduleName]"
```

## subtree

```js
//prefix 相对路径
git subtree add   --prefix=<prefix> <commit>
git subtree add   --prefix=<prefix> <repository> <ref>
git subtree pull  --prefix=<prefix> <repository> <ref>
git subtree push  --prefix=<prefix> <repository> <ref>
git subtree merge --prefix=<prefix> <commit>
git subtree split --prefix=<prefix> [OPTIONS] [<commit>]

//sakura
git subtree pull --prefix crow/src/sakura ssh://git@git.duitang.net:7999/fe/sakura.git master
//ninja
git subtree pull --prefix crow/src/ninja ssh://git@git.duitang.net:7999/fe/ninja.git master
//提交更改到子项目Git服务器
https://www-t005.duitang.net/site/login/?
```

## commit 规范

> 参考: [我的 Git commit 规范](https://mp.weixin.qq.com/s?__biz=MzIyMzEyMDQ5MQ==&mid=2656091498&idx=1&sn=8e15c57c3c1cb6c7579208aaad5ae256&chksm=f387006ec4f08978606e1168d417869a02c42945eed246a310c6bc06e7538888484d857eaffc&scene=4&subscene=126&ascene=0&devicetype=android-26&version=26070337&nettype=WIFI&abtest_cookie=BQABAAoACwANABMAFAAFACOXHgBZmR4AhJkeAImZHgCNmR4AAAA%3D&lang=zh_CN&pass_ticket=AAI4HCiVr%2FcBu7J0BPB0hr%2Fv6WUoYnaJG1b7tytCnHA4Oeh9WeRwR8brLOUDmOO2&wx_header=1)

`<type>: <subject>`

type -- commit 的类别:

- feat：新功能（feature）
- fix：修补 bug
- doc：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

subject -- 简短描述, 动词开头, 现在时, 小写

body -- 详情描述, 改变详情, 改变动机
