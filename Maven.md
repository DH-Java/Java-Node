# 1.目前掌握的技术

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\目前掌握的技术.png)

# 2.目前的技术在开发中存在的问题[why]

1.一个项目就是一个工程
如果项目非常庞大，就不适合继续使用package来划分模块。最好是每一个模块对应一个项目，利于分工协作。
借助于maven就可以将一个项目拆分成多个工程。
2.项目中需要的jar包必须手动“复制”、”粘贴” 到WEB-INF/lib 项目下
带来的问题：同样的jar包文件重复出现在不同的项目工程中，一方面浪费存储空间，另外也让工程比较臃肿。
借助Maven，可以将jar包仅仅保存在“仓库”中，有需要使用的工程“引用”这个文件，并不需要重复复制。
3.jar包需要别人替我们准备好，或到官网下载
所有知名框架或第三方工具jar包已经按照统一规范放在了Maven的中央仓库中。
4.一个jar包依赖的其他jar包需要自己手动加到项目中
Maven会自动将被依赖的jar包导入进来。

# 3.Maven是什么[what]

1.Maven 是 Apache 软件基金会组织维护的一款自动化构建工具，专注服务于 Java 平台的项目构建和依赖管理 。
构建工具的发展：Make→Ant→Maven→Gradle
2.构建：就是以我们编写的Java代码、框架配置文件、国际化等其他资源文件、jsp页面和图片等静态资源作为“原材料”，去“生产”出一个可以运行的项目的过程。
3.构建过程中的几个主要环节
①清理：删除以前的编译结果，为重新编译做好准备。
②编译：将Java源程序编译为字节码文件。
③测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。
④报告：将每一次测试后以标准的格式记录和展示测试结果。
⑤打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java工程对应jar包，Web工程对象war包。
⑥安装：在Maven环境下特指将打包的结果——Jar包或War包安装到本地仓库中。
⑦部署：将打包的结果部署到远程仓库或将war包部署到服务器上运行。
4.自动化构建（简答举例）
程序员一天的工作：

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\自动化构建.png)

能否将这些程式化的工作交给机器自动完成呢？——当然可以！这就是自动化构建。

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\自动化构建1.png)

此时 Maven 的意义就体现出来了，它可以自动的从构建过程的起点一直执行到终点：

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\maven.png)

5.Maven安装
https://blog.csdn.net/weixin_43920778/article/details/105218281

# 4.Maven的核心概念

1.约定的目录结构
2.POM
3.坐标
4.依赖
5.库
6.生命周期/插件/目标
7.继承
8.聚合

# 5.第一个Maven工程

1.创建约定的目录结构

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\创建约定的目录结构.png)

pom.xml文件为Maven工程的核心配置文件
2.为什么要遵循约定的目录结构呢？
我们在开发中如果需要让第三方工具或框架知道我们自己创建的资源在哪，那么基本上就是两种方式：
①以配置文件的方式明确告诉框架 如 < param-value>classpath:spring-context.xml < /param-value>
②遵循框架内部已经存在的约定 如log4j的配置文件名规定必须为 log4j.properties 或 log4j.xml ；Maven 使用约定的目录结构

# 6、Maven常用命令

注意：执行与构建过程相关的Maven命令，必须进入pom.xml 所在的目录。
常用命令
【1】mvn clean : 清理
【2】mvn compile : 编译主程序
【3】mvn test-compile : 编译测试程序
【4】mvn test : 执行测试
【5】mvn package : 打包
【6】mvn install ： 安装
【7】mvn site ：生成站点

# 7、关于联网问题

Maven 的核心程序中仅仅定义了抽象的生命周期，但是具体的工作必须有特定的插件来完成。而插件本身不包含在Maven核心程序中。
当我们执行的Maven命令需要用到某些插件时，Maven核心程序会首先到本地仓库中查找。
本地仓库的默认位置：[系统登陆用户的家目录] \ .m2\repository
Maven核心程序如果在本地仓库中找不到需要的插件，那么它会自动连接外网，到中央仓库下载。
如果此时无法连接外网，则构建失败。
修改默认本地仓库的位置可以让Maven核心程序到我们事先准备好的目录下查找插件
①找到Maven解压目录\conf\settings.xml
②在setting.xml 文件中找到 localRepository 标签
③将 < localRepository>/path/to/local/repo< /localRepository>从注释中取出
④将标签体内容修改为自定义的Maven仓库目录

# 8、POM

1.含义：Project Object Model 项目对象模型
DOM ：Document Object Model 文档对象模型
2.pom.xml 对于 Maven工程是核心配置文件，与构建过程相关的一切设置都在这个文件中进行配置。
重要程度相当于web.xml 对于动态web工程

# 9、坐标

数学中的坐标：
①在平面中，使用X,Y坐标可以唯一的定位平面中任何一个点。
②在空间中，使用X,Y，Z三个向量可以唯一的定位空间中的任何一个点。
Maven的坐标：
使用下面三个向量在仓库中唯一定位一个Maven工程
①groupid:公司或组织域名倒序+项目名

```java
< groupid>com.atguigu.maven< /groupid>
```

②artifactid:模块名

```java
< artifactid>Hello< /artifactid>
```

③version：版本
< version>1.0.0< /version>
Maven 工程的坐标与仓库中路径的对应关系，以spring为例

```java
< groupId>org.springframework< /groupId>
< artifactId>spring-core< /artifactId>
< version>4.0.0.RELEASE< /version>
```

org/springframework/spring-core/4.0.0.RELEASE/spring-core-4.0.0.RELEASE.jar

注意：我们自己的 Maven 工程必须执行安装操作才会进入仓库。安装的命令是：mvn install

# 10、仓库

仓库的分类
①本地仓库：当前电脑上部署的仓库目录，为当前电脑上所有Maven工程服务
②远程仓库
（1）私服：搭建在局域网环境中，为局域网范围内的所有Maven工程服务

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\私服.png)

（2）中央仓库：假设在Internet上，为全世界所有Maven工程服务
（3）中央仓库镜像：为了分担中央仓库流量，提升用户访问速度
仓库中保存的内容：Maven工程
①Maven自身所需要的插件
②第三方框架或工具的jar包
③我们自己开发的Maven工程
不管是什么样的 jar 包，在仓库中都是按照坐标生成目录结构，所以可以通过统一的方式查询或依赖。

# 11、依赖

当 A jar 包用到了 B jar 包中的某些类时，A 就对 B 产生了依赖，这是概念上的描述。Maven解析依赖信息时会到仓库中查找被依赖的jar包。
对于我们自己开发的Maven工程，要使用mvn install 命令安装后就可以进入仓库。
依赖的范围
①从项目结构角度理解compile和test的区别

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\从项目结构角度理解compile和test的区别.png)

compile范围依赖
》对主程序是否有效：有效
》对测试程序是否有效：有效
》是否参与打包：参与
》是否参与部署：参与
》典型例子：spring-core
test范围依赖
》对主程序是否有效：无效
》对测试程序是否有效：有效
》是否参与打包：不参与
》是否参与部署：不参与
》典型例子：Junit
②从开发和运行这两个阶段理解compile 和 provided 的区别

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\从开发和运行这两个阶段理解compile 和 provided 的区别.png)

》对主程序是否有效：有效
》对测试程序是否有效：有效
》是否参与打包：不参与
》是否参与部署：不参与
》典型例子：Servlet-api.jar
③有效性总结

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\有效性总结.png)

依赖的传递性

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\依赖的传递性.png)A依赖B，B依赖C，A能否使用C呢？要看B依赖C的范围是不是compile

依赖的排除

如果我们当前工程中引入了一个依赖是A，而A又依赖了B，那么Maven会自动将A依赖的B引入当前工程，但是个别情况下B有可能是一个不稳定版本，或对当前工程有不良影响。这时我们可以在引入A的时候将B排除。
①情景举例

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\依赖的排除.png)

②配置方式

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\配置方式.png)

③排除后的效果

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\排除后的效果.png)

统一管理所依赖 .jar 包的版本
对同一个框架的一组jar包最好使用相同的版本。为了方便升级架构，可以将jar包的版本信息统一提取出来
①统一声明版本号

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\统一声明版本号.png)

其中 atguigu.spring.version 部分是自定义标签。
②引用前面声明的版本号

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\引用前面声明的版本号.png)

③其他用法

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\其他用法.png)

依赖的原则，解决jar包冲突
①路径最短者优先

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\路径最短者优先.png)

②路径相同时先声明者优先

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\Maven\路径相同时先声明者优先.png)

# 12、生命周期

1.各个构建环节执行的顺序：不能打乱顺序，必须按照既定的正确顺序来执行。
2.Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的。
3.Maven核心程序为了更好的实现自动化构建，按照这一特点执行生命周期中各个阶段：不论现在要执行生命周期中的哪一阶段，都是从这个生命周期最初的位置开始
4.Maven有三套相互独立的生命周期，分别是：
①Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
②Default Lifecycle 构建的核心部分，编译、测试、打包、安装、部署等等。
③Site Lifecycle 生成项目报告，站点，发布站点。
他们相互独立。也可以直接运行 mvn clean install site 运行所有这三套生命周期。
每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行 mvn clean，这个 clean 是 Clean 生命周期的一个阶段。有 Clean 生命周期，也有 clean 阶段。
5.Clean声明周期
①pre-clean 执行一些需要在clean之前完成的工作
②clean 移除所有上一次构建生成的文件
③post-clean 执行一些需要在clean 之后立刻完成的工作
6.Default声明周期
Default 生命周期是 Maven 生命周期中最重要的一个，绝大部分工作都发生在这个生命周期中。这里，只解释一些比较重要和常用的阶段：
validate
generate-sources
process-sources
generate-resources
process-resources 复制并处理资源文件，至目标目录，准备打包。
compile 编译项目的源代码。
process-classes
generate-test-sources
process-test-sources
generate-test-resources
process-test-resources 复制并处理资源文件，至目标测试目录。
test-compile 编译测试源代码。
process-test-classes
test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
prepare-package
package 接受编译好的代码，打包成可发布的格式，如 JAR。
pre-integration-test
integration-test
post-integration-test
verify
install 将包安装至本地仓库，以让其它项目依赖。
deploy 将最终的包复制到远程的仓库，以让其它开发人员与项目共享或部署到服务器上运行。
7.Site生命周期
①pre-site 执行一些需要在生成站点文档之前完成的工作
②site 生成项目的站点文档
③post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
④site-deploy 将生成的站点文档部署到特定的服务器上
这里经常用到的是 site 阶段和 site-deploy 阶段，用以生成和发布 Maven 站点，这可是 Maven 相当强大的功能，Manager 比较喜欢，文档及统计数据自动生成，很好看。
8.插件和目标
●Maven 的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的。
●每个插件都能实现多个功能，每个功能就是一个插件目标。
●Maven 的生命周期与插件目标相互绑定，以完成某个具体的构建任务。
例如：compile 就是插件 maven-compiler-plugin 的一个目标；pre-clean 是插件 maven-clean-plugin 的一个目标