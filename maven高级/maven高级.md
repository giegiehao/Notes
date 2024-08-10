# maven

## 分模块设计

<img src="a7feae48366ae518267a73bf3c7b1398.png" alt="截图" style="zoom:50%;" />

<br/>

## 继承

	问题：使用分模块设计，每个模块都需要引入当前模块需要的依赖，如果每个模块都需要某些依赖，那么每个模块都要重复的引入这些依赖，就会很繁琐
	
	解决：使用maven的继承，继承是单继承，创建一个父工程，打包方式为pom（只需要引入依赖），父工程引入子工程重复使用的模块。

- 继承关系

	在springboot项目中，当前项目的父工程还需要继承springboot的父工程

<img src="980387f18bc4461f6e203c12f1c4bcd2.png" alt="截图" style="zoom:50%;" />

<img src="ba46f0a75860715e161e25eb2a9faeda.png" alt="截图" style="zoom:50%;" />

<img src="dcc74f57e7d9492a476483a2b74cb1e0.png" alt="截图" style="zoom:50%;" />

- 版本锁定
  
  在父工程中使用<dependencyManagement>对子工程的依赖进行依赖管理
  
  为了方便维护和更新依赖版本，可以在<properties>中自定义标签来记录版本号，在依赖引入上的版本号位置使用${...}来插入版本号
  
  <img src="466eb8fd32314af0d9b5c6415935c516.png" alt="截图" style="zoom:50%;" />

<img src="2008cec99c4e5ce3281c8df24eabaf0e.png" alt="截图" style="zoom:50%;" />

<br/>

## 聚合

在父工程（聚合工程）中绑定子工程，当需要子工程需要进行maven批量操作时（clean，package，install等），直接在聚合工程中进行，就可以对绑定的所有子工程进行操作。

<img src="24eb8fa314020da27b207e4ec814dfac.png" alt="截图" style="zoom:50%;" />

<br/>

<br/>

## 继承与聚合

![截图](d5b067141c9ba0d19dcc67d2636a8ac0.png)
