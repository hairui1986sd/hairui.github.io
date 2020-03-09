# IDEA配置Google Java Code Style

### Code Style（代码样式，代码规范）

如果公司中存在某些编码规范，则在写代码时必须遵守这些规范。IntelliJ IDEA可帮助您维护所需的代码样式。

Code Style是在项目级别和IDE级别（全局）定义的。





### Google-java-format plugin和intellij-java-google-style.xml

> 该`google-java-format`插件是格式化代码的首选方式。由于它只是按需启动，因此还建议您具有代码样式设置（intellij-java-google-style.xml），以帮助您随时随地创建格式正确的代码。这些设置不能完全模仿`google-java-format`插件所强制执行的格式，但是请尽量使其接近。因此，在提交代码之前，请确保运行 **重新格式化代码**。

##### 下载Google Java code style

访问https://github.com/google/styleguide ，找到intellij-java-google-style.xml文件，将其保存到本地。

##### 导入IDEA

在IDEA中File--> Setting --> Editor --> Code Syle --> Java --> 设置图标--> import scheme --> intellij IDEA  code style XML.

![IDEA配置Google Java Code Style-01.png](https://i.loli.net/2020/03/10/fYjOZCUp1qiKRbL.png)

##### 设置Scheme

将上面导入的GooglesStyle设置为全局的规范。

![IDEA配置Google Java Code Style-02.png](https://i.loli.net/2020/03/10/bs2Hwj3ApyJ9DtL.png)

##### 安装Google-Java-format插件

在IDEA中file-->settings-->plugins，输入插件名称Google-Java-format搜索插件并安装，然后重启IDEA。

> **PS: 现象：marketplace plugins are not loaded”或Plugins搜不到插件。解决方法：Windows Defender防火墙没关，把防火墙关闭或者把Idea和浏览器的应用防火墙关闭。**

 After installation, the plugin has to be enabled per-project (it is disabled by default). To do that, open your project and go to File -> Settings -> Other Settings -> google-java-format Settings and check “Enable google-java-format”

![IDEA配置Google Java Code Style-03.png](https://i.loli.net/2020/03/10/vsl6zD8PuwX3aNB.png)

### 使用

IDEA的菜单栏-->Code -->-Reformat Code，也可以使用快捷键（Ctrl+Alt+L）。

### 参考

[intelij IDEA设置goole code style风格](https://blog.csdn.net/banana1006034246/article/details/84071935)

[Intellij IDEA 配置 Code Style](https://www.jianshu.com/p/a44329bf5935)

[在IntelliJ IDEA中配置Google Java Code Style及代码格式化快捷键](https://www.cnblogs.com/tonggc1668/p/9322415.html)

[Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)