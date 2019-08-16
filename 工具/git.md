# Git

```table
ohmyzsh git    |    缩写
git status | gst
git add | ga
git add --all | gaa
git commit -a -m    |    gcam
git commit -m | gcmsg
git branch|gb
git branch -d |gbd
git branch -D|gbD
git diff | gf
|git checkout|gco|
|git checkout master|gcm|
|git checkout -b|gcb|
|git checkout -t origin/xxx|gcb|
|git add|ga|
|git fetch origin|gfo|
|git push|gp|
|git push origin $(git_current_branch)|ggpush|
|git pull origin $(git_current_branch)|ggpull|
|git reset --hard [commit-hashcode] ||
git remote -v|grv
git remote add|gra
git remote rename|grmv
git remote remove|grrm
```

git stash 存入储藏不会commit
git stash list 储藏列表
git stash apply [stash@{idx}] 应用储藏
git stash drop [stash@{idx}]移除储藏

git remote -v
git remote add [name] [url]
git remote rm [name]
远程仓库有不同的history
git pull origin master --allow-unrelated-histories

查看单个文件修改历史
`git log [文件路径]`
回退单个文件
`git checkout [hash] [文件路径]`

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
