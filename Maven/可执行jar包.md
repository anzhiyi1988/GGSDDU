# maven三种打包(可执行)插件

## scope介绍

compile - 依赖的默认范围就是编译范围，classpath中会记录，并被打包。

provided - 依赖在部署时，中间件或其他容器会有提供，所以classpath中会记录，不会打包。

runtime - 依赖在运行和测试时需要，编译不需要。

test - 依赖在编译和运行是都不需要，只在测试编译和测试运行时可用。

system - 与provided类似，需要一个本地的jar，是系统类库，该配置**不推荐**使用呢



## maven build介绍

### base build

```
<build>  
    <defaultGoal>install</defaultGoal>  
    <directory>${basedir}/target</directory>  
    <finalName>${artifactId}-${version}</finalName>  
    <filters>  
        <filter>filters/filter1.properties</filter>  
    </filters>  
    ...  
</build> 
```

- defaultGoal ： 执行 *install* 任务时，如果没有指定目标，将使用的默认值
- directory ： build目标文件的存放目录，默认在${basedir}/target目录；
- finalName ： build目标文件的文件名，默认情况下为${artifactId}-${version}；

### 指定resource build

```
<build>  
    ...  
    <resources>  
         <resource>  
            <targetPath>META-INF/plexus</targetPath>  
            <filtering>false</filtering>  
            <directory>${basedir}/src/main/plexus</directory>  
            <includes>  
                <include>configuration.xml</include>  
            </includes>  
            <excludes>  
                <exclude>**/*.properties</exclude>  
            </excludes>  
         </resource>  
    </resources>  
    <testResources>  
        ...  
    </testResources>  
    ...  
</build>  
```

- resources ： 一个resource元素的列表，每一个都描述与项目关联的文件是什么和在哪里；
- targetPath ： 指定build后的resource存放的文件夹。该路径默认是basedir。通常被打包在JAR中的resources的目标路径为META-INF；
- filtering ： true/false，表示为这个resource，filter是否激活
- directory ： 定义resource所在的文件夹，默认为${basedir}/src/main/resources；
- includes ： 指定作为resource的文件的匹配模式，用*作为通配符；
- excludes ： 指定哪些文件被忽略，如果一个文件同时符合includes和excludes，则excludes生效；
- testResources ： 定义和resource类似，但只在test时使用，默认的test resource文件夹路径是${basedir}/src/test/resources，test resource不被部署。



### 指定plugin build

```
<build>  
    ...  
    <plugins>  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-jar-plugin</artifactId>  
            <version>2.0</version>  
            <extensions>false</extensions>  
            <inherited>true</inherited>  
            <configuration>  
                <classifier>test</classifier>  
            </configuration>  
            <dependencies>...</dependencies>  
            <executions>...</executions>  
        </plugin>  
    </plugins>  
</build>  
```

- extensions ：true/false，是否加载plugin的extensions，默认为false；
- inherited ：true/false，这个plugin是否应用到该POM的孩子POM，默认true；
- configuration ：配置该plugin期望得到的properies，如上面的例子，我们为maven-jar-plugin的Mojo设置了classifier属性；如果你的POM有一个parent，它可以从parent的build/plugins或者pluginManagement集成plugin配置。



## maven-jar-plugin介绍









# 参考：

https://blog.csdn.net/u011499747/article/details/83045928?depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-5&utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-5



https://yq.aliyun.com/articles/308777





https://www.jianshu.com/p/7a0e20b30401?spm=a2c4e.10696291.0.0.772119a4MJtc01



有三种打可执行jar包的方式



第一种方式

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-jar-plugin</artifactId>

<version>2.4</version>

<configuration>

<archive>

<manifest>

<addClasspath>true</addClasspath>

<classpathPrefix>lib/</classpathPrefix>

<mainClass>com.sysware.HelloWorld</mainClass>

</manifest>

</archive>

</configuration>

</plugin>  

 

mvn clean package

 

第二种方式

<plugin>  

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-assembly-plugin</artifactId>

<version>2.3</version>

<configuration>

<appendAssemblyId>false</appendAssemblyId>

<descriptorRefs>

<descriptorRef>jar-with-dependencies</descriptorRef>

</descriptorRefs>

<archive>

<manifest>

<mainClass>com.juvenxu.mvnbook.helloworld.HelloWorld</mainClass>

</manifest>

</archive>

</configuration>

</plugin>  

 

mvn assembly:assembly