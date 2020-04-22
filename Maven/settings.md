# localRepository

本地仓库的地址

# interactiveMode

表示maven是否需要和用户交互以获得输入

# offline

Maven进行项目编译和部署等操作时是否允许Maven进行联网来下载所需要的信息。

# pluginGroups

表示当通过plugin的前缀来解析plugin的时候到哪里寻找，

pluginGroup元素指定的是plugin的groupId。

默认情况下，Maven会自动把org.apache.maven.plugins和org.codehaus.mojo添加到pluginGroups下。

# proxies

Maven在进行联网时需要使用到的代理

# servers

当需要连接到一个远程服务器的时候需要使用到的验证方式

# mirrors

定义一系列的远程仓库的镜像

pom中定义一个下载工件的时候所使用的远程仓库

有时候远程仓库会比较忙，所以这个时候人们就想着给它创建镜像以缓解远程仓库的压力，也就是说会把对远程仓库的请求转换到对其镜像地址的请求。每个远程仓库都会有一个id，这样我们就可以创建自己的mirror来关联到该仓库，那么以后需要从远程仓库下载工件的时候Maven就可以从我们定义好的mirror站点来下载，这可以很好的缓解我们远程仓库的压力。在我们定义的mirror中每个远程仓库都只能有一个mirror与它关联，也就是说你不能同时配置多个mirror的mirrorOf指向同一个repositoryId。

```
<mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
```

### id：

　　是用来区别mirror的，所有的mirror不能有相同的id

### mirrorOf：

　　　用来表示该mirror是关联的哪一个仓库，其值为其关联仓库的id。当要同时关联多个仓库时，这多个仓库之间可以用逗号隔开；当要关联所有的仓库时，可以使用“*”表示；当要关联除某一个仓库以外的其他所有仓库时，可以表示为“*,!repositoryId”；当要关联不是localhost或用file请求的仓库时，可以表示为“external:*”。

### url：

　　表示该镜像的url。当Maven在建立系统的时候就会使用这个url来连接到我们的远程仓库。

# profiles

用于指定一系列的profile。

**profile** - profile元素由id、activation、repositories、pluginRepositories和properties五个元素组成。当一个profile在settings.xml中是处于活动状态并且在pom.xml中定义了一个相同id的profile时，settings.xml中的profile会覆盖pom.xml中的profile。

- id - profile的id

- activation - 设置profile被激活的条件
  - activeByDefault - 其值为true的时候表示如果没有其他的profile处于激活状态的时候，该profile将自动被激活
- properties - 定义属性键值对的。当该profile是激活状态的时候，properties下面指定的属性都可以在pom.xml中使用
- repositories - 用于定义远程仓库的，当该profile是激活状态的时候，这里面定义的远程仓库将作为当前pom的远程仓库
  - id
  - name
  - url - 仓库地址
  - releases/snapshots - 工件的类型限制，通过enabled控制
    - enabled
    - updatepolicy更新策略，always、daily、interval：minutes、never
    - checksumPolicy：当Maven在部署项目到仓库的时候会连同校验文件一起提交，checksumPolicy表示当这个校验文件缺失或不正确的时候该如何处理，可选项有ignore、fail和warn。

- pluginRepositories - 在Maven中有两种类型的仓库，一种是存储工件的仓库，另一种就是存储plugin插件的仓库。pluginRepositories的定义和repositories的定义类似，它表示Maven在哪些地方可以找到所需要的插件

