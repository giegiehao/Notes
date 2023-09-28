# Maven

<br/>

## maven介绍

maven是一个软件

使用需要下载软件解压到电脑上（系统需要有java环境）

配置系统环境变量和path

<br/>

**配置文件：**

修改/conf/settings.xml配置文件

修改本地仓库：localRepository

修改镜像仓库：mirrors->mirror

修改jdk版本项目构建：profiles->profile

<br/>

idea使用本地maven配置

在项目的设置中配置maven主项目

<br/>

**GAVP属性**

GAV为依赖的三个定位属性

GroupID：组织名

ArtifactID：模块名/产品名

Version：版本号

Packaging：打包后缀

	jar包 代表普通的java工程

	war包 代表java的web工程

	pom包 代表不打包，用来做继承的父工程

<br/>

在<properties>中可以声明依赖的变量，方便未来管理各种依赖版本

使用${...}导入声明的变量

<br/>

**导入插件：**<bulid> -> <plugins>

<br/>

<scope> 引入依赖的作用域

	默认 compile	范围 main test 运行和打包(全范围)

	test	test

	runtime	运行和打包

	provided	main test

<br/>

**依赖传递：**导入依赖，会自动导入依赖的依赖 简化依赖的导入 确保依赖的版本无冲突

**依赖冲突：**发现已经存在依赖（重复依赖）会终止依赖传递 避免循环依赖和重复依赖的问题

	依赖冲突的解决原则：

		第一原则：依赖传递路径长短，路径越短（靠前）优先

		第二原则：依赖导入的先后顺序，靠前优先

<br/>

## Maven构建命令周期

构建命令周期：一组固定构建命令的有序集合，触发周期后的命令，会自动触发周期前的命令

- 清理周期：clean
- 默认（构建）周期：compile - test - package - install/deploy
- 报告周期：site

<br/>

package：打包项目，生成war/jar包，用于独立部署到外部服务器软件（war）或外部运行环境（jar），打包部署

install：打包后上传到maven本地仓库（本地部署)

deploy：打包后上传到maven私服仓库（私服部署） 供其它工程使用

<br/>

<br/>

## Maven工程继承关系

<parent> 在子工程中指定父工程 继承

继承概念：

Maven继承是指在Maven的项目中，让一个项目从另一个项目中继承配置信息的机制，继承可以让我们在多个项目中共享同一配置信息 简化项目的管理和维护工作

继承作用：

在父工程中统一管理项目中的依赖信息，进行同一版本管理

创建父工程(打包方式为pom)，只需要有pom文件，项目的模块都继承父工程成为子工程，此时在父工程有进行版本管理的依赖无需进行版本选择，或者已经在父工程中引入的依赖无需再次引入

<dependencies> 标签为导入依赖，会下载引入

<dependencyManagement> 标签为声明依赖，不会下载依赖，只做版本声明

<br/>

<br/>

## Maven工程聚合关系

<modules> -> <module> 在父工程中指定子工程 聚合

聚合作用：在父工程中统一管理子工程的构建，在项目统一打包、测试、部署等构建过程的时候无需对多个子工程操作，只需在父工程中构建即可对聚合的所有子工程进行构建 简化构建过程
