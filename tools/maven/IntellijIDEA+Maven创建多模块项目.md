# Intellij IDEA + Maven创建多模块项目

### 环境

| 名称     | 版本      |
| -------- | --------- |
| 操作系统 | Windows10 |
| IDEA     | 2019.2    |
| Maven    | 3.6.1     |
| JDK      | 11.0.6    |

### Maven的继承和聚合

maven多模块项目通常由一个父模块和若干个子模块构成，每个模块都对应着一个pom.xml。它们之间通过继承和聚合（也称作多模块）相互关联。多模块适用于一些比较大的项目，通过合理的模块拆分，实现代码的复用，便于维护和管理。
**继承**：与Java中的继承有点类似，就是父pom.xml声明的版本和引用的jar，子模块可以不用再引用直接调用。继承可以使子pom获得parent中的各项配置，对子pom进行统一的配置和依赖管理。父pom中的大多数元素都能被子pom继承。
**聚合**：父模块包含多个子模块就是聚合，多个子模块之间可以调用，但是要注意关系，不要两个互相依赖，这样做的好处就是可以通过一条命令进行构建。
**依赖**：就相当于我们java中的导包,二者有着异曲同工之妙;你想用的东西只需要告诉maven它在哪就可以,它会自动帮你找过来给你用。

> groupId是项目组织唯一的标识符，实际对应JAVA的包的结构，artifactId是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称。groupId一般分为多个段，一般第一段为域，第二段为公司名称,第三段通常为项目名称。

### IDEA 创建多项目模块

##### 创建父模块（空Maven项目）

基于IDEA 创建新的Maven 项目，选择菜单项File->New->Project。弹出窗口左侧边栏选择maven，右边选择jdk版本，不选择create from archetype，next。填写GroupId、ArtifactId和Version，next。填写工程名称和本地存放工程的位置，点击完成。

<img src="https://i.loli.net/2020/03/10/W2TMxlqkvY1XDre.png" alt="idea-maven-01.png" style="zoom:50%;" />

目前得到一个maven工程，由于这个工程作为父工程存在，一般多模块开发中父工程只做依赖管理，不需要编写代码。可以直接删除src目录。删除后的目录结构如下：

![idea-maven-02.png](https://i.loli.net/2020/03/10/aqYDEZUQpIzjd3v.png)

##### 项目文件名称及含义

| 目录/文件 | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| .idea     | 创建项目的时候自动创建一个 .idea 的项目配置目录来保存项目的配置信息。这是默认选项。 子目录包含一系列XML文件，包括：compiler.xml、encodings.xml、modules.xml等。这些文件记录工程本身的核心信息，包括：模块组件的名称和位置、编译器设置等，可以存放到VCS。一个例外是workspace.xml，该文件存储个人设置（例如窗口位置）以及其它附属于开发环境的信息，不应该存放到VCS |
| pom.xml   | Maven 项目配置文件。POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。 |
| .iml      | 模块是工程中一个可以独立编译、运行、调试、测试的单元。模块的配置信息默认存放在其内容根目录（Content root folder）下的 .iml 文件中，该文件一般存放到VCS。 |

在pom 文件中，添加 <packaging> 打包类型<packaging>pom</packaging>，**父模块的打包方式必须是pom**。另外，maven项目之间的继承关系通过<parent>元素表示。这里使用的开发框架是spring boot，默认继承spring-boot-starter-parent。

##### 使用dependencyManagement管理依赖版本号
一般在项目最顶层的父pom中使用该元素，让所有子模块引用一个依赖而不用显式的列出版本号。maven会沿着父子层次向上走，直到找到一个拥有dependencyManagement元素的项目，然后它就会使用在这个dependencyManagement元素中指定的版本号。pom文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.jerico.spring</groupId>
  <artifactId>springboot</artifactId>
  <version>1.0-SNAPSHOT</version>

  <!-- Inherit defaults from Spring Boot -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
  </parent>

  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR3</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
</project>
```

##### 创建子模块

选择项目根目录，右键单击，选择「 New -> Module 」，弹出的窗口左侧边栏，选择maven->next；填写 ArifactId ，版本号默认继承父工程的，点击「 Next 」； 修改 Module name 增加横杠提升可读性，点击「 Finish 」。

![idea-maven-03.png](https://i.loli.net/2020/03/10/HcOxkColyFY8Tmw.png)

![idea-maven-04.png](https://i.loli.net/2020/03/10/RvMcZHrqyEOx6eV.png)

同样的方式，添加其他子模块(dao-mybatis，service，controller，common，entity)。另外controller到service模块的调用，还可以使用dubbo、hessian等方式进行远程调用 。

### 整理和解释父子模块的pom文件

##### 父工程pom

1. 删除 dependencies 标签及其中的 spring-boot-starter 和 spring-boot-starter-test 依赖，因为 Spring Boot 提供的父工程已包含，并且父 pom 原则上都是通过 dependencyManagement 标签管理依赖包。

> 注：dependencyManagement 及 dependencies 的区别自行查阅文档

2. 删除 build 标签及其中的所有内容，spring-boot-maven-plugin 插件作用是打一个可运行的包，多模块项目仅仅需要在入口类所在的模块添加打包插件，这里父模块不需要打包运行。而且该插件已被包含在 Spring Boot 提供的父工程中，这里删掉即可。
3.  modules标签包含的是子模块。
4. properties标签里面是总体参数配置。
5. dependencyManagement标签并不引入依赖，是总体的依赖声明。
6. dependencies标签引入依赖，子类共享。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- 这里作为聚合工程的父工程 -->
  <groupId>com.jerico.spring</groupId>
  <artifactId>springboot</artifactId>
  <packaging>pom</packaging>
  <version>1.0-SNAPSHOT</version>

  <!-- maven的聚合。自动生成的子模块依赖（这里声明多个子模块）-->
  <modules>
    <module>dao-jpa</module>
    <module>dao-mybatis</module>
    <module>service</module>
    <module>common</module>
    <module>controller</module>
    <module>entity</module>
  </modules>

  <!-- 属性说明 -->
  <properties>
    <java.version>1.8</java.version>
  </properties>

  <!-- Inherit defaults from Spring Boot（继承：这里继承Spring Boot提供的父工程） -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
  </parent>

  <!--公共jar包声明-->
  <dependencies>
  </dependencies>

  <!--版本统一声明，声明子模块的版本号-->
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR3</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>

</project>
```

##### entity模块

1. parent标签声明父标签；
2. artifactId声明子标签，因为`groupId`和`version`都是继承了父pom，故可以省略；
3. 因为是最底层依赖，故可以不依赖其他子模块；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <!-- 继承父工程信息 -->
  <parent>
    <artifactId>springboot</artifactId>
    <groupId>com.jerico.spring</groupId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>

  <!-- groupId和version都是继承了父pom，故可以省略 -->
  <artifactId>entity</artifactId>

</project>
```

##### service模块

1. parent标签声明父pom；

2. service模块需要使用entity模块的数据，故需要dependencies标签引入entity模块；

3. 注意循环引用问题，即service模块依赖entity模块；entity模块也依赖service模块

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <!-- maven的继承 -->
  <parent>
    <artifactId>springboot</artifactId>
    <groupId>com.jerico.spring</groupId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>

  <artifactId>service</artifactId>

  <dependencies>
    <dependency>
      <groupId>com.jerico.spring</groupId>
      <artifactId>entity</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>

</project>
```

##### dao-jpa，dao-mybatis，common模块不再一一列出，与service和entity模块相似

##### controller模块

1. web层需要依赖service层和entity层，故需将其dependency引入；
2. 在多模块项目中仅仅需要在启动类所在的模块添加打包插件，将其打成可运行的jar。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <!-- maven的继承 -->
  <parent>
    <artifactId>springboot</artifactId>
    <groupId>com.jerico.spring</groupId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>

  <artifactId>controller</artifactId>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--依赖service层-->
    <dependency>
      <groupId>com.jerico.spring</groupId>
      <artifactId>service</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
    <!--依赖entity层-->
    <dependency>
      <groupId>com.jerico.spring</groupId>
      <artifactId>entity</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
  </dependencies>
  <!--多模块打包-->
  <build>
    <defaultGoal>package</defaultGoal>
    <finalName>${project.artifactId}</finalName>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
  
</project>
```

### 创建启动类

##### 创建启动类

在controller模块中，新建（com.jerico.springboot）包和启动类(Application)。

![idea-maven-06.png](https://i.loli.net/2020/03/11/9YjCqwFTsg8WNUu.png)

##### 注意点

1. 父pom.xml打包方式，jar要更改为pom，build需要更改；
2. 不需要打包的模块pom.xml文件不要写<build>，全删掉，例如有些工程的common模块，utils模块都不需要打包；
3. 声明父工程时，填写父类工程位置<relativePath>../pom.xml</relativePath>
4. 关于application.properties配置文件，只需要在启动的模块中配置就可以了。
5. 关于打包为什么打包jar包，不打war包，打war包目的是war包可以运行在tomcat下，但是SpringBoot是内置tomcat，如果你打war包，前提是干掉内置的tomcat，然后才能打包，各种麻烦，直接打包可执行jar包，使用java -jar 命令就可以完美的运行起来很方便！
6. 真实开发中使用@Autowired 注解 来实现注入，而不是new对象这种方式，所以可能会产生注入以后报错，是因为你的启动类上没有配置扫描，使用

@ComponentScan(basePackages = "你的路径")注解来解决，如果你使用的持久层是Mybatis，那么你的mapper也需要扫描，在启动类上使用

@MapperScan("你的mapper文件地址")注解来解决，算了还是贴个图片吧
 真实开发中使用@Autowired 注解 来实现注入，而不是new对象这种方式，所以可能会产生注入以后报错，是因为你的启动类上没有配置扫描，使用

@ComponentScan(basePackages = "你的路径")注解来解决，如果你使用的持久层是Mybatis，那么你的mapper也需要扫描，在启动类上使用

@MapperScan("你的mapper文件地址")注解来解决，算了还是贴个图片吧



### 打包为可执行jar

##### 打包

1. 在每次打包前，需要clean。
2. 双击运行package，看到BUILD SUCCESS 就证明打包成功了。

> **打包的重点是每一个模块下的pom要配置好，谁需要打包，谁不需要打包，谁依赖谁，父工程是否声明了子模块，子模块是否声明了父工程。**

![idea-maven-05.png](https://i.loli.net/2020/03/11/k1264TPlxUzwrEb.png)







### 聚合和继承的关系

区别：

- 对于聚合模块来说，他知道哪些被聚合的模块，但那些被聚合的模块不知道这个模块的存在；
- 对于继承关系的父POM来说，它不知道哪些子模块继承了它，但是子模块都必须知道自己的父POM是什么。

共同点：

- 聚合POM与继承关系的父POM的packaging都是POM。
- 聚合模块与继承关系的父模块除了POM之外都没有实际的内容。

![img](https:////upload-images.jianshu.io/upload_images/16013479-d13ab7ecfedb470d.png?imageMogr2/auto-orient/strip|imageView2/2/w/558/format/webp)

> 聚合工程举一个简单的例子：
>  整个工程就好像一个公司，父工程（退休了什么都不干）只需要声明有几个儿子（子模块）就完事了。
>  子模块web声明父工程是谁，就当他是大儿子，公司归他管，pom.xml需要打包，需要build配置，需要其他子模块的帮助。
>  其他模块声明父工程是谁，之间关系都是兄弟，不需要打包，哪里需要就去哪里。

### 参考

[IntelliJ IDEA创建多服务模块的Spring Cloud微服务项目]([https://youyou-tech.com/2019/11/11/IntelliJIDEA%E5%88%9B%E5%BB%BA%E5%A4%9A%E6%9C%8D%E5%8A%A1%E6%A8%A1%E5%9D%97%E7%9A%84/](https://youyou-tech.com/2019/11/11/IntelliJIDEA创建多服务模块的/))

[Idea创建多模块maven聚合项目](https://segmentfault.com/a/1190000021385783)

[IDEA多module项目maven依赖的一些说明](https://segmentfault.com/a/1190000016778269)

[Spring Boot 项目实战（一）Maven 多模块项目搭建](https://symonlin.github.io/2019/01/15/springboot-1/)

[maven-idea构建多模块项目-maven的聚合和继承](https://www.jianshu.com/p/83f334e2c810)

[IntelliJ IDEA 构建 Maven 多模块工程](https://zhuanlan.zhihu.com/p/84175296)

[IntelliJ IDEA 2018.3 创建 Maven 多模块(Module)项目+负载均衡](http://luoma.pro/Content/Detail/548?parentId=1)