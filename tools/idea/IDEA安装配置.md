# IDEA安装配置

> Windows 10
>
> IDEA 2019.2 64位

### 下载安装

访问idea官网（http://www.jetbrains.com/）下载并安装；激活方法可参考：http://www.jetbrains.com/

### 配置

##### 全局（默认）配置 VS当前项目配置

![1565409797459](C:\Users\hairui\AppData\Roaming\Typora\typora-user-images\1565409821487.png)

1. 全局配置

   顶部工具栏 File --> Other Settings --> Setting for new project；

   主要设置一些每个项目共同的配置，比如：字体，jdk，maven，git等；

2. 当前项目配置

   顶部工具栏 File --> Settings；

   主要配置每个项目特有的配置，比如：有的项目用的jdk版本不同等。

> **PS：如果相同的选项，全局配置和当前项目配置的值不同，则使用当前项目的配置（即当前项目配置覆盖全局配置）。比如：全局配置的jdk为1.8，但是当前项目需要使用1.6版本，此时，当前项目配置为1.6版本jdk即可。**

##### 基础配置（全局）

1. ##### 设置项目文件编码格式

   顶部工具栏File -->Settings --> File Encodings 

   ![](D:\Developer\Doc\images\idea-20.png)

2. ##### 设置界面风格和外部UI的字体字号

   顶部工具栏File -->Settings --> Appearance&Behavior --> Appearance

   ![](D:\Developer\Doc\images\idea-21.png)

3. ##### 打开IDEA时默认不打开最近的项目

   顶部工具栏File -->Settings --> Appearance&Behavior --> System Settings，把红框中的勾取消即可。

   ![](D:\Developer\Doc\images\idea-22.png)

4. ##### 显示常用工具栏

   ![](D:\Developer\Doc\images\idea-17.png)

5. ##### 调整字体大小和字体类型	

   顶部工具栏 File --> Settings --> Editor --> Font，分别设置编辑器的字体、字号和行间距。

   ![](D:\Developer\Doc\images\idea-14.png)

6. ##### 新建类文件的类注释模板

   顶部工具栏 File --> Settings --> Editor --> File and Code Templates，我们以class文件为例（当然可以设置interface,Enum等），下图中有标出 #parse("File Header.java")一行，这句代码是引入了File Header.java文件，作为我们创建的Class Interface ,Enum 等文件的注释，这个类在 Files 右侧，有一个 Includes 选项，在这里可以定义各种的模板，在需要的地方去引入这个模板，这里已经在类文件中引入了File Header.java 模板，修改这个模板为我们想要的样子。

   ![](D:\Developer\Doc\images\idea-24.png)

7. ##### 快捷键设置与修改

   顶部工具栏 File -> Settings -> Keymap – > 选择Eclipse .如果刚从Eclipse切过来的同学，可以把快捷键设置为eclipse快捷键。

   ![](D:\Developer\Doc\images\idea-18.png)

#####  JDK配置（全局）

顶部工具栏 File ->Other Settins -> Structure for new project-> SDKs -> JDK

![](C:\Users\hairui\AppData\Roaming\Typora\typora-user-images\1565410917559.png)

#####  MAVEN配置（全局）

顶部工具栏 File ->Other Settins -> settings for new project-> Build，Execution，Deployment -> Build --> Maven

![](D:\Developer\Doc\images\idea-7.png)



![](D:\Developer\Doc\images\idea-15.png)

如上图所示打开Maven的面板，可以快速进行maven相关的操作。

##### 版本控制Git/SVN（全局）

顶部工具栏 File ->Other Settings -> Settings for New Projects -> Version Control -> Git

![](D:\Developer\Doc\images\idea-8.png)

![](D:\Developer\Doc\images\ideae-9.png)

IDEA默认对Git和SVN支持，直接按照上图设置可执行程序的路径即可，点击右边“Test”提示成功。如果在本地找不到svn.exe，则重装SVN，重装时配置项重新选择command line client tools 即可。

##### 自动导包和智能移除（全局）

顶部工具栏 File ->Other Settings -> Settings for New Projects -> Other Settings --> Auto Import。

> **因为optimize imports on the fly这个设置是只针对当前项目的，所以如果需要，请在每创建一个项目的时候都来设置下**

![](D:\Developer\Doc\images\idea-10.png)

##### Tomcat Server(当前项目配置)

 顶部工具栏 File -> Settings ->  Build，Execution，Deployment --> Deployment -> Application Servers -> Tomcat Server，然后选择Tomcat的本地安装目录；

由于不是所有项目都需要Tomcat，因此针对项目进行配置，而不是全局配置。

![](D:\Developer\Doc\images\idea-11.png)

##### 自动编译

顶部工具栏 File--> Other Settings -> Settings for New Projects -> Build，Execution，Deployment -> Compiler,勾选下图红框中的选项。

IDEA默认是不开启自动编译的，自动编译在服务器未运行时才可以使用。

![](D:\Developer\Doc\images\idea-16.png)

##### 代码提示（取消大小写敏感）

 顶部工具栏 File -> Settings -> Editor --> General --> Code Completion，然后去掉Match case前的勾。在写代码时，自动提示会更加的全面和丰富。

![](D:\Developer\Doc\images\idea-13.png)

##### 小技巧

1. 隐藏开发工具的配置目录，如*.idea;*.iml

   ![](D:\Developer\Doc\images\idea-19.png)

2. 收起注释，让源码阅读更为清爽

   File -> Settings -> Editor -> General -> Code Folding -> Documentation comments 勾选。

   如何想快速一键打开全部注释，则单击鼠标右键，选择Folding -> Expand Doc comments 。

##### 常用快捷键

| Eclipse       | IDEA                | 英文描述                      | 中文描述                            |
| ------------- | ------------------- | ----------------------------- | ----------------------------------- |
| ctrl+shift+r  | ctrl+shift+n        | Navigate->File                | 找工作空间的文件                    |
| ctrl+shift+t  | ctrl+n              | Navigate->Class               | 找类定义                            |
| ctrl+shift+g  | alt+f7              | Edit->Find->Find Usages       | 查找方法在哪里调用.变量在哪里被使用 |
| ctrl+t        | ctrl+t              | Other->Hierarchy Class        | 看类继承结构                        |
| ctrl+o        | ctrl+f12            | Navigate->File Structure      | 搜索一个类里面的方法                |
| shift+alt+z   | ctrl+alt+t          | Code->Surround With           | 生成常见的代码块                    |
| shift+alt+l   | ctrl+alt+v          | Refactor->Extract->Variable   | 抽取变量                            |
| shift+alt+m   | ctrl+alt+m          | Refactor->Extract->Method     | 抽取方法                            |
| alt+左箭头    | ctrl+alt+左箭头     | Navigate->Back                | 回退上一个操作位置                  |
| alt+右箭头    | ctrl+alt+右键头     | Navigate->Forward             | 前进上一个操作位置                  |
| ctrl+home     | ctrl+home           | Move Caret to Text Start      | 回到类最前面                        |
| ctrl+end      | ctrl+end            | Move Caret to Text End        | 回到类最后面                        |
| ctrl+e        | ctrl+e              | View->Recent Files            | 最近打开的文件                      |
| alt+/         | ctrl+space          | Code->Completion->Basic       | 提示变量生成                        |
| ctrl+1        | alt+enter           | Other->Show Intention Actions | 提示可能的操作                      |
| ctrl+h        | ctrl+shift+f        | Find in Path                  | 全局搜索                            |
| alt+上/下箭头 | alt+shift+上/下箭头 | Code->Move Line Up/Down       | 移动一行代码                        |
| ctrl+/        | ctrl+/              | Other->Fix doc comment        | 方法注释                            |
| ctrl+alt+s    | alt+insert          | Generate                      | 生成getter,setter,tostring等        |

### IDEA 插件





















### 创建MAVEN工程

![1565408607218](C:\Users\hairui\AppData\Roaming\Typora\typora-user-images\1565408607218.png)

![1565409168553](C:\Users\hairui\AppData\Roaming\Typora\typora-user-images\1565409168553.png)

![1565409078555](C:\Users\hairui\AppData\Roaming\Typora\typora-user-images\1565409078555.png)