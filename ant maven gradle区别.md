# ant

ANT是最早的构建工具，基于idea,好象是2000年有的，当时是最流行java构建工具，不过它的XML脚本编写格式让XML文件特别大。对工程构建过程中的过程控制特别好。

# Maven

Maven它是用来给Ant补坑的，Maven第一次支持了从网络上下载的功能，仍然采用xml作为配置文件格式，它的问题是不能很好的相同库文件的版本冲突。Maven专注的是依赖管理，构建神马的并不擅长。

# Gradle

Gradle是一个自动化的构建工具，Gradle属于结合以上两个的优点，它继承了Ant的灵活和Maven的生命周期管理，它最后被google作为了Android御用管理工具,它能够构建几乎所有类型的软件,它最大的区别是不用XML作为配置文件格式，采用了DSL格式，使得脚本更加简洁。Gradle引入了基于Groovy语言的DSL语法来代替XML配置，因此它的配置文件是一个Groovy文件。



四、构建性能
Gradle比Maven性能高不了，甚至还低一些。
不过是这两种构建工具都要明显慢于Ant。


五、仓库
Ant全部要自己处理。
Maven有自己的单一仓库坐标格式。
Gradle可以使用Ivy仓库和Maven仓库。

六、构建生命周期
Ant也可以访问任何一部分构建，但是需要用n多个任务来实现而不是代码，所以还是不如Gradle强大。
Maven提供有限的构建生命周期访问。 插件可以连接到生命周期的特定阶段，而且只有在核心插件执行;不过也可以自己写插件完成特定生命周期操作,不过这个可不是一两天可以搞定的.
Gradle很容易这方面NB，因为它可以轻松地访问任何生成的一部分，并允许用Groovy代码处理。



gradle自定义打印构建日志：

在build.gradle里添加这样的任务：

```groovy
task hello << { 
    println 'welcome to gradle';
}
```

然后执行命令：

```groovy
gradle -q hello
```





ant有一个很致命的缺陷，那就是没办法管理依赖。我们一个工程，要使用很多第三方工具，不同的工具，不同的版本。每次打包都要自己手动去把正确的版本拷到lib下面去，然后在配置文件depend:

```xml
    <!-- 第三方jar包的路径 -->
    <path id="lib-classpath">
        <fileset dir="${lib.dir}">
            <include name="**/*.jar"/>
        </fileset>
    </path>
```

```xml
<dependencies>
        <dependency org="org.slf4j" name="slf4j-api" rev="1.6.1"/>
        <dependency org="org.slf4j" name="slf4j-log4j12" rev="1.6.1" transitive="false"/>
</dependencies>
```

maven、gradle只需要引入版本号就可以对jar进行管理：

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.68</version>
</dependency>
```

```groovy
// https://mvnrepository.com/artifact/com.alibaba/fastjson
compile group: 'com.alibaba', name: 'fastjson', version: '1.2.68'
```

![unnamed](picture/unnamed.jpg)