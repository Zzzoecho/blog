# CodePush

## 概念

Staging -- 测试环境
Production -- 生产环境

## 打包部署流程

1. 本地打包(区分客户端 -> 打包至 RN 项目下的 bundle 文件夹中)
2. code-push 发布更新包 `code-push release <App Name(区分客户端)> ./bundle/android <需更新的客户端版本号> --description '<描述>'`
3. 这时默认发布至 Staging 环境
4. RN 更新测试, 可用`code-push deployment ls PickYou-Android` 命令查看是否发布成功
5. 测试完成后, 用`code-push promote PickYou-Android Staging Production` 推送到生产环境

## 常用命令

### release

```shell
code-push release <appName> <updateContents> <targetBinaryVersion>
[--deploymentName <deploymentName>]
[--description <description>]
[--disabled <disabled>]
[--mandatory]
[--rollout <rolloutPercentage>]
```

updateContents: 指定打包文件位置
targetBinaryVersion: 客户端版本号, 规则如下
| 范围表达式     | 更新范围                          |
| -------------- | --------------------------------- |
| 1.2.0          | 只有 1.2.0 版本                   |
| \*             | 所有版本                          |
| 1.2.x          | 主版本为 1, 小版本为 2 的任何版本 |
| 1.2.0 - 1.2.5  | 1.2.0(包含)-1.2.7(包含)           |
| >=1.2.3 <1.2.7 | 1.2.0(包含) - 1.2.7(不包含)       |
| ~1.2.3         | 等于 >=1.2.3 <1.3.0               |
| ^1.2.3         | 等于 >=1.2.3 <2.0.0               |

Mandatory: 是否强制更新. 动态转换的过程: 假设用户安装了 v1, 服务端更新了强制更新的 v2, 不久后又更新了非强制更新的 v3. 这时用户检查更新, 下载 v3. v3会变成强制更新
rollout: 灰度发布, 指定更新的用户比例, 1-100. 默认100

### release-react

一键发布, 合并了`react-native bundle`命令和`code-push release`命令

```bash
code-push release-react <appName> <platform>
[--bundleName <bundleName>]
[--deploymentName <deploymentName>]
[--description <description>]
[--development <development>]
[--disabled <disabled>]
[--entryFile <entryFile>]
[--gradleFile <gradleFile>]
[--mandatory]
[--noDuplicateReleaseError]
[--outputDir <outputDir>]
[--plistFile <plistFile>]
[--plistFilePrefix <plistFilePrefix>]
[--sourcemapOutput <sourcemapOutput>]
[--targetBinaryVersion <targetBinaryVersion>]
[--rollout <rolloutPercentage>]
[--privateKeyPath <pathToPrivateKey>]
[--config <config>]
```

### patch

补丁更新: 发布更新后, 想要修改此次更新的参数

```shell
code-push patch <appName> <deploymentName>
[--label <releaseLabel>]
[--mandatory <isMandatory>]
[--description <description>]
[--rollout <rolloutPercentage>]
[--disabled <isDisabled>]
[--targetBinaryVersion <targetBinaryVersion>]
```

label: 发布版本 (如: v3)

### promote

促进更新: 将 Staging 环境推到 Production 环境上

```shell
code-push promote <appName> <sourceDeploymentName> <destDeploymentName>
[--description <description>]
[--disabled <disabled>]
[--mandatory]
[--rollout <rolloutPercentage>]
[--targetBinaryVersion <targetBinaryVersion>]
```

### rollback

回滚更新

```shell
code-push rollback <appName> <deploymentName>
[--targetRelease <targetBinaryVersion>]
```

targetRelease参数指定需要回滚的版本，默认为上个版本

### deployment

查看部署状况

`code-push deployment ls <AppName>`: 该 App 列出所有环境的更新包状况
`code-push deployment history <AppName> <deploymentName>`: 列出 App 在某个环境下的更新记录

Install Metrics:
Active: 活跃用户比例(当前活跃用户数 of 全部用户数)
Total: 使用当前版本的用户总数

```shell
Active: 50% (1 of 2)
Total: 1
```

### debug

`code-push debug <platform>`: 监听某平台下的更新记录
