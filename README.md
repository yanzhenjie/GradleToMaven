# 简化Gradle发布Java/Android项目到Jcenter/Maven
这个仓库是因为个人发布项目到Jcenter/Maven时每次需要写maven文件比较麻烦，所以放到github上来方便自己，如果其它人看到之后也喜欢这种方式，不妨也试试。

这个库只是为了简化发布，并不是发布流程，具体的发布流程，请参考下面的链接：  
[http://blog.csdn.net/yanzhenjie1003/article/details/51672530](http://blog.csdn.net/yanzhenjie1003/article/details/51672530)

本库提供的方式可以只发布到Jcenter，或者发布Jcenter和Maven，后者的原理是发布到Jcenter后同步到Maven，所以不能只发布到Maven。  

- [仅发布到Jcenter](./Bintray.md)  
- [同时发布到Jcenter和Maven](./Maven.md)  

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