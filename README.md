# BotPlugin
一个运行于[Mirai Console](https://github.com/mamoe/mirai-console)的插件    
A plugin for [mirai console](https://github.com/mamoe/mirai-console) with image sender,pixiv querying and more.
## 功能
* 发送本地圖庫的圖片(请手动下载赫图,另提供指令sendImage一次发送多张)(发送带有'图'和'来'的消息或者色图来图片(不知道这图我也没办法,你可以用图片ID发出去))
* 发送在线的Pixiv的圖片(允许设置代理,说得好像不设置代理谁进得去呢?)
* 被At时复读,并且把At的对象换成对方(此外还支持颠倒消息顺序,随机旋转消息图片)
* 使用系统自带的命令行执行Ping(可能不安全)
* 查询网址的IP地址
* 在群聊中更新某些配置(带有用户控制(废话))
* 设置群聊黑名单(注意settings和darklist指令全局可用,黑名单群settings指令只回復manager)
* 禁言,取消禁言,加群,退群提示(注意:有臭味
* 没了
## 特色
* 单线程撤回发出的本地图片(防止请求过于频繁被服务器拒绝)
* CommandDispatcher(?蜜汁操作,指令派发)
**在本仓库的Releases中可以直接下载编译好的jar文件!如果不是需要研究,你可以直接下载**
## 简单使用
- 下载Release里发布的jar.
- [启动Mirai-Consle](https://github.com/mamoe/mirai-console/blob/master/docs/Run.md)
- 复制jar到Console工作目录下的plugins里
- 运行Console
## 准备工作
### 安装 IDE 插件 
-  Mirai Console 提供 IntelliJ 插件来提升开发体验。
- [Kotlin Jvm Blocking Bridge](https://github.com/mamoe/kotlin-jvm-blocking-bridge) ([JetBrains 插件仓库](https://plugins.jetbrains.com/plugin/14816-kotlin-jvm-blocking-bridge), [一键安装](https://plugins.jetbrains.com/embeddable/install/14816))：帮助 Java 用户调用 Kotlin suspend 函数
- [Mirai Console IntelliJ](../tools/intellij-plugin/) ([JetBrains 插件仓库](https://plugins.jetbrains.com/plugin/15094-mirai-console), [一键安装](https://plugins.jetbrains.com/embeddable/install/15094))：提供错误检查等功能
### JVM环境:
* 桌面 JVM：最低 Java 8，但推荐 Java 11  
### 当前项目Mirai环境:
* Mirai Core 1.3.3
* Mirai Console 1.0.0   
## 开始使用
先带着插件运行一次Mirai Console,然后停止   
在`mirai文件夹/config/RosemoeBotPlugin/PluginConfig.yml`中把   
```yml
managers: []
```
修改为
```yml
managers:
  - 你的QQ
```
当然也可以添加多个Manager,比如   
```yml
managers:
  - 114514
  - 1919810
  - 12345678
```   
然后重新运行Mirai Console   
然后用Manager账号在群聊发送
```Kotlin
/settings set ImagePathList 图片或者图片文件夹路径(可用';'分割)
```
设定你的图片路径表,即可开始使用   
如果不想被其他人知道你的图片路径,也可以参照README.md最下方的PluginConfig.yml示例手动写路径表
## 群聊指令表
非常建议您先读完下面的指令表再使用
### 设置部分
只有Plugin Manager(不是Bot Manager)才能使用这个指令!
```Kotlin
/settings set RecallDelay <时间>
/settings set RecallInterval <时间>
/settings set ImagePathList <路径,可用';'分割多个>
/settings set Prefix <指令前缀,默认'/'>
/settings reload
/settings reloadBase
/settings get RecallDelay
/settings get RecallInterval
/settings get ImagePathList
/settings get Prefix
/settings enable <功能>
/settings disable <功能>
```
reloadBase只刷新配置不重新建立图片索引,算是轻重载    
功能名称表:
```Kotlin
val allowedModuleName = listOf(
    "ImageSender",
    "Ping",
    "IpList",
    "Pixiv",
    "BatchImg",
    "AtReply",
    "Welcome",
    "MuteTip",
    "ReverseAtReply",
    "ReverseAtReplyImage"
)
```
### Pixiv
```Kotlin
查看画作信息和预览图
/pixiv illust <画作ID>
查看指定画作的指定P的大图
/pixiv illust <画作ID> <图片索引>
```
R18画作将不会发送图片
### 本地图片
```Kotlin
/sendImage <图片数目>
```
### Ping
```Kotlin
/ping 后面和操作系统一样
```
### IP获取
```Kotlin
/ipList <网址>
/ipList4 <网址>
/ipList6 <网址>
```
### 黑名单
```Kotlin
/darklist add <群号或this>
/darklist remove <群号或this>
/darklist list <页码,可选,每页15个>
```
## Console指令表
咕了,还没写呢
## 设置代理
* `proxyEnabled` 配置是否启用代理
* `proxyType` 配置代理类型,必须是socks或者http其中一种,填写其它默认socks
* `proxyAddress` 配置代理地址,如`127.0.0.1`
* `proxyPort` 配置代理端口,如`1080`   
这些代理设置仅用于Pixiv网路连接,不作它用   
实现上使用了Java的Proxy类
## 示例PluginConfig.yml
```yml
managers: 
  - 1145141919
  - 8101145141
states: 
  ImageSender: true
commandPrefix: $
imagePathList: 
  - 'D:\`Repo\`\Pixiv\grade_0'
  - 'D:\`Repo\`\Pixiv\grade_1'
  - 'D:\`Repo\`\Yande'
  - 'D:\`Repo\`\Danbooru'
  - 'D:\`Repo\`\Konachan'
  - 'D:\`Repo\`\QQ Images'
darkListGroups: 
  - 122344646
  - 464544644
imageRecallDelay: 30000
recallMinPeriod: 180
maxImageRequestCount: 16
proxyEnabled: true
proxyType: socks
proxyAddress: 127.0.0.1
proxyPort: 1080
allowR18ImageInPixiv: false
```
