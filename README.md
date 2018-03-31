# 使用Gradle发布Java项目到jcenter/mavenCentral
这个仓库是因为个人发布项目到`jcenter/mavenCentral()`时每次需要做配置比较麻烦，所以放到Github上来方便自己，如果其它人看到之后也喜欢这种方式，不妨也试试。

本库提供的方式可以只发布到`jcenter()`，或者同时发布到`jcenter()`和`mavenCentral()`，后者的原理是发布到`jcenter()`后通过Binary同步到`mavenCentral()`，所以不能只发布到`mavenCentral()`。  

- [仅发布到`jcenter()`](./Bintray.md)  
- [同时发布到`jcenter()`和`mavenCentral()`](./Maven.md)  

## 相关链接
1. Binary：[https://bintray.com](https://bintray.com)
2. Sonatype：[https://issues.sonatype.org](https://issues.sonatype.org)
3. Release process：[http://blog.csdn.net/yanzhenjie1003/article/details/51672530](http://blog.csdn.net/yanzhenjie1003/article/details/51672530)

## License
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