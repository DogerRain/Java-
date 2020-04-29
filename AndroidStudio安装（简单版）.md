AndroidStudio有很多种版本，可以在这个网站找到（不用梯子）：http://www.android-studio.org/index.php/download/hisversion/

# 一 下载AndroidStudio

我使用的是这个版本：

![image-20200430004503948](F:\笔记\yudianxxJavaNote\image-20200430004503948.png)



该版本包括了SDK、AVD一系列东西，双击进行安装：

![image-20200428234730164](F:\笔记\yudianxxJavaNote\image-20200428234730164.png)

我本地已经安装了SDK，会监测到SDK的路径，假如你没有安装SDK或者想重新安装SDK，可以选择新的文件夹，这里我把这里提示的所有都安装

![image-20200429002925854](F:\笔记\yudianxxJavaNote\image-20200429002925854.png)

我安装到这个目录

![image-20200430005248283](F:\笔记\yudianxxJavaNote\image-20200430005248283.png)

安装完成就可以了，然后这个文件夹就是你的SDK目录了，你再另外安装的插件都会在这个文件夹，然后你需要配置一下环境变量：

# 二 配置SDK环境变量

## **1. 新建环境变量 ANDROID_HOME**

ANDROID_HOME 为SDK安装目录

## **2.**修改path 

在path变量后面追加

```xml
;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;%ANDROID_HOME%\build-tools\29.0.3
```

%ANDROID_HOME%\build-tools\29.0.3 这个环境变量要根据自己安装build-tools的版本修改

如果是win10则分行写：

![image-20200428233428727](F:\笔记\yudianxxJavaNote\image-20200428233428727.png)

## **3.**cmd测试

![image-20200428234142412](F:\笔记\yudianxxJavaNote\image-20200428234142412.png)

注意，高版本的是 

```c
adb veriosn
```

低版本是

```
adb -veriosn
```

# 三 修改配置（可以省略）

这一步可以省略，我目的是为了节省C盘空间。

Android Studio安装好后会在系统盘用户目录下产生这几个文件夹
`.android`是Android SDK生成的AVD（Android Virtual Device Manager）即模拟器存放路径
`.AndroidStudio3.6`是Android Studio的配置文件夹，主要存放一些Android Studio设置、插件、项目的缓存信息
`.gradle`是构建工具Gradle的配置文件夹，也会存储一些项目的构建缓存信息

## 1  修改AndroidStudio配置路径

![image-20200429000830778](F:\笔记\yudianxxJavaNote\image-20200429000830778.png)



打开AndroidStudio的安装目录的配置文件：

![image-20200428235525282](F:\笔记\yudianxxJavaNote\image-20200428235525282.png)

修改这两个配置:

![image-20200430005839095](F:\笔记\yudianxxJavaNote\image-20200430005839095.png)

注意是 `/` 不是` \`

## 2 修改AVD路径

avd是模拟器的镜像，你用真机调试或者第三方模拟器调试可以不用安装

新建环境变量：ANDROID_SDK_HOME

目录为空文件夹。

![image-20200430010217554](F:\笔记\yudianxxJavaNote\image-20200430010217554.png)

## 3 修改gradle配置

新建环境变量：

GRADLE_USER_HOME

也是空文件夹

![image-20200430010351497](F:\笔记\yudianxxJavaNote\image-20200430010351497.png)



以上三步如果不修改，默认在C盘。但是很占空间。

# 四 新建项目

一直next

![image-20200430011625226](F:\笔记\yudianxxJavaNote\image-20200430011625226.png)

这个不勾选：

![image-20200430011650323](F:\笔记\yudianxxJavaNote\image-20200430011650323.png)

# 五 处理gradle下载慢

因为首次打开AndroidStudio会下载gradle，因为有墙，下载会很慢，会默认下载一个gradle版本，在.gralde目录会生成一个gradle目录，有一串签名，我的是这个

![image-20200430010822460](F:\笔记\yudianxxJavaNote\image-20200430010822460.png)

然后知道是gradle 4.1版本，如果你没有下载下来，可以在AndroidStudio，打开配置看看：

![image-20200430010931578](F:\笔记\yudianxxJavaNote\image-20200430010931578.png)

知道这个版本后，你要去这里下载对应的版本，一般是all版本：https://services.gradle.org/distributions/

下载完了，关闭AndroidStudio，把上面这4个文件清除，把你下载的gradle-4.1-all.zip放到这里，不用解压。



然后重启AndroidStudio，try again 就可以了。

然后根据提示:

![img](F:\笔记\yudianxxJavaNote\890216-20171111232713294-1387011987.png)

![img](F:\笔记\yudianxxJavaNote\890216-20171111232828091-429636493.png)

以上两图来自：https://www.cnblogs.com/xiadewang/p/7820377.html

gradle构建可能很慢，你可以使用阿里云的镜像：

![image-20200430011412193](F:\笔记\yudianxxJavaNote\image-20200430011412193.png)

注释这两个

```java
//allprojects {
//    repositories {
//        google()
//        jcenter()
//    }
//}
allprojects {
    repositories {
// jcenter()
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        google()
    }
}
```



然后就可以了。

# 六 运行

![image-20200430011732701](F:\笔记\yudianxxJavaNote\image-20200430011732701.png)

我已经安装了模拟器，会自动识别，运行即可：

![image-20200430011929793](F:\笔记\yudianxxJavaNote\image-20200430011929793.png)