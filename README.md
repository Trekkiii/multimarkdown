IntelliJ IDEA Multi-MarkDown插件安装破J全过程

### IntelliJ IDEA-markdown 相关介绍
multimarkdown 是支持 intelliJ IDEA工具的一款 markdown 收费插件。简单理解就是用来写文章排版的，一般 github 上面的项目默认都有一个 readme.md 文件（以".md"结尾来描述项目的说明文档）。本文就是使用简书的 markdown 语法写的。

### 注意：
目前最新版的multimarkdown1.4.10 需要jdk1.8的支持

### 安装 multi-markdown
![](http://upload-images.jianshu.io/upload_images/1620978-0e945f095a960384.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/1620978-720fab8178c44b45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**注:markdown Navigator 也即 multi-markdown**

安装完成之后需要重启 idea

重启 idea 显示如下界面，如往常一样该插件免费试用 30 天，在文章后面我们将解决这个问题。
![](http://upload-images.jianshu.io/upload_images/1620978-4b0e6ab01984c401.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 破J解
#### 总体步骤
我们需要修改 idea-multimarkdown.jar 中的 LisenceAgent.java 文件，并将该文件编译好生成 LisenceAgent.class 文件,然后将【目标文件 = LisenceAgent.class 】替换掉。
![](http://upload-images.jianshu.io/upload_images/1620978-698d78cf76d419fc.png?imageMogr2/auto-orient/strip%7CimageView2/2)

#### 根据文件名找到LicenseAgent.java
linuxjar包位置:/home/liuchunlong/.IdeaIC2016.3/config/plugins/idea-multimarkdown/lib

com.vladsch.idea.multimarkdown.license.LicenseAgent.java 找到该文件
![](http://upload-images.jianshu.io/upload_images/1620978-369002b2071ebce6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 新建项目(等会编译那个LicenseAgent.java)
在 idea 中新建一个项目将 LicenseAgent.java 扔进去
那么建一个什么样的项目呢？为什么这么问？请往下面看，重点来了！
重点：这个项目的目录必须是这样的com.vladsch.idea.multimarkdown.license
为什么呢？因为不这样建项目这个类的 package 属性就变了，在multiMarkdown插件初始化的时候会找不到名字叫：
【com.vladsch.idea.multimarkdown.license.LicenseAgent】的这个类。
再说一遍，必须这么建！如下图：
![](http://upload-images.jianshu.io/upload_images/1620978-a37d89bcce1c3b43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后把 LicenseAgent.java 扔到 ：
com.vladsch.idea.multimarkdown.license 包中。
发现 N 多报错，下面来解决
#### 解决项目的多处报错
设置项目依赖 jar 包，idea-markdown中的 lib 文件夹 和 idea 的 lib 文件夹

idea-markdown中的 lib 文件夹:
![](http://upload-images.jianshu.io/upload_images/1620978-f8dd4ca5375fe20d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

intellij idea 的 lib 文件夹
![](http://upload-images.jianshu.io/upload_images/1620978-0cdfb60b08570e32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

完整的jar 包依赖如下：
![](http://upload-images.jianshu.io/upload_images/1620978-e1fa908eaa1d9227.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后 apply 一下 Dependencies。

#### 修改LisenceAgent.java
```
修改 com.vladsch.idea.multimarkdown.license.LicenseAgent.java 文件的内容如下：

    getLicenseExpires() 整个方法体干掉不要了(删除方法体)，只留返回值改为 return "Never Expires";
    getLicenseCode() 最后一行返回值 return false 改为 return true，对你没有看错，只改最后一行代码;
    isValidLicense() 删除方法体，只留返回值，返回值改为 return true;
    isValidActivation() 删除方法体，只留返回值，返回值改为 return true;
    getLicenseType() 删除方法体，只留返回值，返回值改为 return "License" 或 return "license";
    getLicenseExpiringIn() 删除方法体，只留返回值，返回值改为 return 36000;(单位是天)
    isActivationExpired() 删除方法体，只留返回值，返回值改为 return false;

修改完成后编译一下,如果报错了查看以上步骤是不是都操作正确。
```

#### 替换LicenseAgent.class

 找到刚才编译完成的 LicenseAgent.class

最后将 idea-multimarkdown.jar 中的 LisenceAgent.class 文件替换掉。
重启intelliJ IDEA。

**编译好的项目见GITHUB:https://github.com/kbAnt/multimarkdown**
