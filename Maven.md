# 同时发布到Jcenter
先发布到Jcenter，发布时会对内容进行签名，后期在Bintray控制台可以手动同步到Maven，第一次手动同步到Maven后，以后可以选择自动同步到Maven的方式。

## 一、依赖本库
在项目中要发布的`module`下的`build.gradle`文件末尾应用本库。

* 喜欢手动同步到Maven
不想发布后就自动同步到Maven，应用这个文件：
```
apply from: 'https://github.com/yanzhenjie/bintray/blob/master/maven.gradle?raw=true'
```
它会先发布到Jcenter，并对发布内容进行签名，然后我们可以在Bintray控制台手动同步到Maven。

* 喜欢自动动同步到Maven
想发布后就自动同步到Maven，应用这个文件：
```
apply from: 'https://github.com/yanzhenjie/bintray/blob/master/maven-auto.gradle?raw=true'
```
它会先发布到Jcenter，并对发布内容进行签名，然后自动同步到Maven。

## 二、配置config.gradle文件
在项目（不是module）下创建一个`config.gradle`文件（如果已有就不用创建），添加下面的内容，具体内容根据自己的项目改动:
```java
ext {
    plugins = [
            maven      : 'com.github.dcendents.android-maven',
            bintray    : 'com.jfrog.bintray'
    ]
    
    bintray = [
            version       : "1.0.0", // 项目本地发布的版本号
            group         : "com.yanzhenjie", // 项目的group名，第一次定好以后不能改

            siteUrl       : 'https://github.com/yanzhenjie/StatusView', // 项目开源地址
            gitUrl        : 'git@github.com:yanzhenjie/StatusView.git', // 项目git克隆地址

            packaging     : 'aar',
            name          : 'StatusView', // 项目名，随意
            description   : 'StatusView for android', // 项目描述，随意

            licenseName   : 'The Apache Software License, Version 2.0', // 开源协议
            licenseUrl    : 'http://www.apache.org/licenses/LICENSE-2.0.txt', // 开源协议地址

            developerId   : 'yanzhenjie', // 开发者id
            developerName : 'yanzhenjie', // 开发者姓名 
            developerEmail: 'smallajax@foxmail.com', // 开发者邮箱

            binrayLibrary : "StatusView", // 项目在bintray上看到的名称
            bintrayRepo   : "maven", // 发布到自己在bintray的哪个仓库中，一般默认maven
            bintrayUser   : 'yolanda', // bintray的用户名
            bintrayLicense: "Apache-2.0" // 在bintray上采用的开源协议
    ]
}
```

> **注意**：因为config文件上传到git/svn等版本管理服务器上，所以不能写密码等重要信息。

## 三、依赖发布插件
第一步：在项目下的`build.gradle`文件开头引入`config.gradle`文件：
```
apply from : "config.gradle"
```

第二步：在项目下的`build.gradle`文件中添加如下两个依赖：
```
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
```

以上两步之后的完整文件大概是这样：
```
apply from : "config.gradle"

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.3'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
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

## 四、配置bintray的apikey
因为`local.properties`是不会上传到git/svn等版本管理服务器上的，所以在项目的`local.properties`文件中填写密码等重要信息：
```
bintray.apikey=fa1d******************6b65a
bintray.gpg.password=s1********b0

bintray.oss.user=a9******h9
bintray.oss.password=b2**********01
```

完成后`local.properties`完整文件是：
```
sdk.dir=\Develop\\Android-SDK
ndk.dir=\Develop\\Android-SDK\\ndk-bundle

bintray.apikey=fa1d******************6b65a
bintray.gpg.password=s1********b0

// 如果需要自动同步到Maven，追加：
bintray.oss.user=a9******h9
bintray.oss.password=b2**********01
```

**注意**：这里我用`*`替代了我的帐号信息，实际上要填入从bintray获取到的真实apikey和真实的gpg密码。oss.user和oss.password是sonatype的帐号和密码，为了自动同步到Maven。

## 发布
上面的各种配置都好了之后，同步一下项目，在当前项目下打开命令行，输入下面的命令进行发布前编译：
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

如果你在项目已经加入Jcenter中，若你选择的是手动同步，那么在bintray控制台手动同步到Maven即可，需要输入sonatype的帐号和密码；若你选择的是自动同步，此时你的项目已经自动发布到Maven了。操作完成后你可以在gradle中使用Jcenter仓库和Maven仓库作为依赖源远程依赖的你的项目了。