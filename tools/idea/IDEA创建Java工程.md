---
title: IDEA创建Java工程
date: 2019-08-11 23:51:07
tags:
---

### 创建java工程

1. 打开IDEA软件，顶部工具栏File --> New --> Project ;可以选择java 或者 Maven都可以创建普通的Java工程，如下图所示：本文以maven工程为例进行演示。

   ![](D:\Developer\Doc\images\idea-java-1.png)

   下图中选择QuickStart或者不选任何东西。

   ![](D:\Developer\Doc\images\idea-java-2.png)

2. 点击next输入groupid和artifactid，然后完成。

   ![](D:\Developer\Doc\images\idea-java-3.png)

3. 生成如下目录结构的Java工程：

   ![](D:\Developer\Doc\images\idea-java-4.png)

4. 配置Configuration，选择Run-->Edit Configurations-->Application，配置Main class和JRE点击OK即可.

   ![](D:\Developer\Doc\images\idea-java-5.png)

### 将创建的工程关联到GitHub

1. 在操作系统资源管理器打开刚刚创建的工程目录，然后右键选择Git Bash，执行：git init，把这个文件夹编程Git可管理的仓库。

2. 通过git status查看当前文件状态，执行git add . 把文件提交到暂存区；

3. 把项目提交到仓库： git commit -m "初次提交"；

4. 在GitHub上创建一个远程仓库，仓库名为Java-basic，当然这里可以和本地工程的名字不相同，此时选择不创建Readme文件；

5. 关联本地仓库和远程仓库。在git bash中执行git remote add origin https://github.com/jerico-tech/java-basic.git

   ![](D:\Developer\Doc\images\idea-java-6.png)

6. 关联好之后，就可以把本地库的所有内容推送到远程仓库了：git push -u origin master