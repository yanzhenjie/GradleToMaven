# 仅发布到Jcenter
只会发布到`jcenter()`，发布时不会对发布的内容进行签名，所以在Bintray控制台也不能手动同步到`mavenCentral()`。

## 一、添加依赖
在项目中要发布的`module`下的`build.gradle`文件末尾添加：
```groovy
apply from: 'https://raw.githubusercontent.com/yanzhenjie/bintray/master/bintray.gradle'
```

## 二、配置config.gradle
在项目根目录下创建一个`config.gradle`文件，然后添加下面的内容，具体内容根据自己的项目改动:
```groovy
ext {
    plugins = [
            bintray    : 'com.jfrog.bintray'
    ]
    
    bintray = [
            version       : "1.0.0", // 指定库的版本号
            group         : "com.example", // 库的group名，确定好之后不能修改

            siteUrl       : 'https://github.com/yanzhenjie/bintray', // 项目开源地址
            gitUrl        : 'git@github.com:yanzhenjie/bintray.git', // 项目git地址

            packaging     : 'aar',
            name          : 'ProjectName', // 项目名，随意
            description   : 'Library for android', // 项目描述，随意

            licenseName   : 'The Apache Software License, Version 2.0', // 开源协议
            licenseUrl    : 'http://www.apache.org/licenses/LICENSE-2.0.txt', // 开源协议地址

            developerId   : 'developer', // 开发者id
            developerName : 'developer', // 开发者姓名
            developerEmail: 'developer@gmail.com', // 开发者邮箱

            binrayLibrary : "ProjectName", // 项目在bintray上的名称
            bintrayRepo   : "maven", // 发布到自己在bintray的哪个仓库中，一般默认maven
            bintrayUser   : 'BintrayUser', // bintray的用户名
            bintrayLicense: "Apache-2.0" // 指定bintray上的开源协议
    ]
}
```

> **注意**：因为`config.gradle`文件需要上传到`git/svn`上，所以不能写密码等重要信息。

## 三、依赖发布插件
第一步：在项目下的`build.gradle`文件开头引入`config.gradle`文件：
```groovy
apply from : "config.gradle"
```

第二步：在项目下的`build.gradle`文件中添加依赖：
```groovy
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
```

以上两步之后的完整文件大概是这样：
```groovy
apply from : "config.gradle"

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

## 四、配置bintray apikey
因为`local.properties`是不会上传到`git/svn`上的，所以在项目的`local.properties`文件中填写密码等重要信息：
```
bintray.apikey=fa1d******************6b65a
```

完成后`local.properties`完整文件是：
```
sdk.dir=\Develop\\Android-SDK
ndk.dir=\Develop\\Android-SDK\\ndk-bundle
bintray.apikey=fa1d******************6b65a
```

**注意**：这里我用`*`替代了我的`apikey`，实际上要填入从Bintray获取到的真实`apikey`。

## 发布
上面的各种配置都好了之后，同步一下项目，然后在当前项目下打开命令行，输入下面的命令进行发布前编译：
```
gradle clean install
```
如果成功执行，则没有任何问题了，就可以直接发布了：
```
gradle bintrayUpload
```
等个一两分钟执行完之后，到bintray上看看，你的项目已经被推送上去了。

后续其它步骤请参考下面的链接：  
[http://blog.csdn.net/yanzhenjie1003/article/details/51672530](http://blog.csdn.net/yanzhenjie1003/article/details/51672530)