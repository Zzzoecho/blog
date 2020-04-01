# iTerm使用技巧

- 开启代理 proxy-start
- 关闭代理 proxy-stop

| 快捷键              | 说明                        |
| ------------------- | --------------------------- |
| ⌘  ;                | 查看历史命令                |
| ^ r                 | 搜索命令历史                |
| ⌘ ⇧  h              | 查看剪贴板历史              |
| ⌘⌥space             | 显示or隐藏iterm             |
| ^ u                 | 清除当前行                  |
| ⌘  k                | 清屏                        |
| ^ a                 | 到行首                      |
| ^ e                 | 到行尾                      |
| ⌘  d                | 垂直分屏                    |
| ⌘ ⇧  d              | 水平分屏                    |
| command + ;         | 查看历史命令                |
| command + shift + h | 查看剪贴板历史              |
| ctrl + a            | 到行首                      |
| ctrl + e            | 到行尾                      |
| ctrl + f/b          | 前进后退 (相当于左右方向键) |
| ctrl + p            | 上一条命令                  |
| ctrl + r            | 搜索命令历史                |
| ctrl + d            | 删除当前光标的字符          |
| ctrl + h            | 删除光标之前的字符          |
| ctrl + w            | 删除光标之前的单词          |
| ctrl + k            | 删除到文本末尾              |

## ohMyZsh

git插件配置文件 `~/.oh-my-zsh/plugins/git/git.plugin.zsh`

```js
vim ~/.zshrc
plugins=( [plugins...] zsh-syntax-highlighting)
source ~/.zshrc
```

zsh-autosuggestions

## jq

JSON格式化

## prep

## Tree

自动生成目录结构

`tree [options] [directory]`

- -a 显示所有文件和目录
- -C 在文件和目录清单加上色彩，便于区分各种类型。
- -d 显示目录名称而非内容。
- -D 列出文件或目录的更改时间。
- -i 不以阶梯状列出文件或目录名称。
- -I 忽略
- -n 不在文件和目录清单加上色彩。
- -p 列出权限标示。
- -t 用文件和目录的更改时间排序。
- -u 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。

## homebrew

- brew --version或者brew -v 显示brew版本信息
- brew install <formula> 安装指定软件
- brew uninstall <formula> 卸载指定软件
- brew list  显示所有的已安装的软件
- brew search text 搜索本地远程仓库的软件，已安装会显示绿色的勾
- brew search /text/ 使用正则表达式搜软件
