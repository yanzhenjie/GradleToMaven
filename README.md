# Gradle发布Android项目到Jcenter
这个仓库是因为个人发布项目到Jcenter时每次需要写maven文件比较麻烦，所以放到github上来方便自己，如果其它人看到之后也喜欢这种方式，不妨也试试。

这个库只是为了简化发布，并不是发布流程，具体的发布流程，请参考下面的链接：  
[http://blog.csdn.net/yanzhenjie1003/article/details/51672530](http://blog.csdn.net/yanzhenjie1003/article/details/51672530)

## 一、依赖本库
在项目中要发布的`module`下的`build.gradle`文件末尾应用本库：
```
apply from: 'https://github.com/yanzhenjie/bintray/blob/master/bintray.gradle?raw=true'
```

## 二、配置config.gradle文件
在项目（不是module）下添加一个创建一个`config.gradle`文件（如果已有就不用创建），添加下面的内容，具体内容根据自己的项目改动:
```java
ext {
    plugins = [
            maven      : 'com.github.dcendents.android-maven',
            bintray    : 'com.jfrog.bintray'
    ]
    
    maven = [
            // library
            version       : "1.0.0", // 发布的版本号

            siteUrl       : 'https://github.com/yanzhenjie/StatusView', // 项目开源地址
            gitUrl        : 'git@github.com:yanzhenjie/StatusView.git', // 项目git克隆地址

            group         : "com.yanzhenjie", // 发布项目依赖时的group名

            // project
            packaging     : 'aar',
            name          : 'StatusView', // 项目名，随意
            description   : 'StatusView for android', // 项目描述，随意

            // project.license
            licenseName   : 'The Apache Software License, Version 2.0', // 开源协议
            licenseUrl    : 'http://www.apache.org/licenses/LICENSE-2.0.txt', // 开源协议地址

            // project.developers
            developerId   : 'yanzhenjie', // 开发者id
            developerName : 'yanzhenjie', // 开发者姓名 
            developerEmail: 'smallajax@foxmail.com', // 开发者邮箱

            // bintray
            binrayLibrary : "StatusView", // 项目在bintray上看到的名称
            bintrayRepo   : "maven", // 发布到自己在bintray的哪个仓库中，一般默认maven
            bintrayUser   : 'yolanda', // bintray的用户名
            bintrayLicense: "Apache-2.0" // 在bintray上采用的开源协议
    ]
}
```

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
在项目的`local.properties`文件中追加：
```
bintray.apikey=fa1d******************6b65a
```

完成后`local.properties`完整文件是：
```
ndk.dir=\Develop\\Android-SDK\\ndk-bundle
sdk.dir=\Develop\\Android-SDK
bintray.apikey=fa1d******************6b65a
```

**注意**：这里我用`*`替代了我的key和密码，实际上要填入从bintray获取到的真实apikey和你真实的apikey。

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

# License
```text
Copyright 2017 Yan Zhenjie

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```