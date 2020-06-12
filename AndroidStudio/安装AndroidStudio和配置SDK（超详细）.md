# 一 下载解压sdk

SDK 下载链接：http://tools.android-studio.org/index.php/sdk

![image-20200428234343877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428234343877.png)

目录如下，双击 SDK Manager.exe ：

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/e4821efbb2204ef39d693c2252b03a3e/clipboard.png)

## 1. 下载所需要的插件：

如果不选这三个可以自己去下载，比较麻烦。

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/d394e0a91e114e44b472b6e3785e25b0/clipboard.png)

选择Android版本：

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/0bfd96451fa94ae095048ade46857c8e/clipboard.png)

usb驱动，可以使用真机：

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/d7a84f36703048f9888f1daa6c1d4812/clipboard.png)

## 2. install安装

勾选协议：

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/72b487e086844b57b10e0ca9c9331f61/clipboard.png)

## **3. 等待安装**

确保网络良好

![img](F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/4166c45e12b14ca09be353d4e291a293/clipboard.png)

安装完成

![img](file:///F:/%E6%9C%89%E9%81%93%E4%BA%91%E6%96%87%E4%BB%B6/huangyongwen0306@163.com%281%29/469abd1f92914760a4ae4e911a0ffa17/clipboard.png)

目录多了很多工具：

![image-20200428232748668](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428232748668.png)

如果开发需要用到其他插件，同样你可以再打开 SDK Manager.exe 安装你需要的插件



如果你被墙了，出现下图，无法下载。

![image-20200429102943399](picture/image-20200429102943399.png)

那么只能去这里单独下载了：https://www.androiddevtools.cn/

按照提示下载即可

![image-20200429103412144](picture/image-20200429103412144.png)

其实使用 SDK Manager.exe 就是一键帮你安装。

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

![image-20200428233428727](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428233428727.png)

## **3.**cmd测试

![image-20200428234142412](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428234142412.png)

注意，高版本的是 

```c
adb veriosn
```

低版本是

```
adb -veriosn
```



# 三 安装Android Studio

Android studio下载链接：http://www.android-studio.org/

Android studio分安装版（.exe文件）和解压版（解压版解压直接就可以使用）

我是直接从官网下载的：https://developer.android.google.cn/studio/

安装：

![image-20200428234730164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428234730164.png)

第二个勾勾是选择是否安装虚拟机，虚拟机是用来调试的，如果你有模拟器或者用真机调试，可以不安装。

安装完就可以下一步了，

![image-20200428234958213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428234958213.png)

安装完成。





**修改配置路径**

Android Studio安装好后会在系统盘用户目录下产生这几个文件夹
`.android`是Android SDK生成的AVD（Android Virtual Device Manager）即模拟器存放路径
`.AndroidStudio3.6`是Android Studio的配置文件夹，主要存放一些Android Studio设置、插件、项目的缓存信息
`.gradle`是构建工具Gradle的配置文件夹，也会存储一些项目的构建缓存信息





![image-20200429000830778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429000830778.png)



**你可以跳过下面这步，我的目的是为了方便管理配置文件，不占用C盘，配置文件默认是放在了C盘，**

打开AndroidStudio的安装目录的配置文件：

![image-20200428235525282](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428235525282.png)

修改这两个配置:

![image-20200429001825484](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429001825484.png)

注意是 `/` 不是` \`





打开AndroidStudio 

![image-20200428235344932](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200428235344932.png)

选择自定义安装，可以指定jdk，sdk，gradle安装目录：

![image-20200429002724977](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429002724977.png)

选择jdk：

![image-20200429002752429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429002752429.png)

选择代码风格：

![image-20200429002548579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429002548579.png)

自动检测到了SDK，否则需要自己指定或者在这里安装：

Android SDK 自带模拟器一直以慢、卡顿著称，而英特尔的 HAXM 技术（Hardware Accelerated Execution Manager）使用基于 Intel(R) Virtualization Technology (VT) 的硬件加速，实现 Android 模拟器加速。具体的硬件要求和安装方式大家可以去官网查看。一般在下载安装 SDK 的时候，会默认帮你安装开启 HAXM



![image-20200429002925854](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429002925854.png)



![image-20200429003156988](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200429003156988.png)

这一步HAXM可能安装失败，不过不影响，可以参考下面的博客重新安装。



参考：

gradle安装：https://blog.csdn.net/yao_94/article/details/81211528

修改默认配置：https://blog.csdn.net/weixin_43465312/article/details/90081227

HAXM重装：https://www.jianshu.com/p/7be97678aa8b

