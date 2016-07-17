Spring技术内幕：深入解析spring架构与设计原理
==
第1章：准备源代码环境
--
SVN是Subversion的简称，是一个开放源代码的版本控制系统，相较于RCS、CVS，它采用了分支管理系统，它的设计目标就是取代CVS。互联网上很多版本
控制服务已从CVS迁移到Subversion。说得简单一点SVN就是用于多个人共同开发同一个项目，共用资源的目的。
第2章：Spring framework的核心;IoC容器的实现
--
http://www.cnblogs.com/fuchongjundream/p/3873073.html 对于IoC模式诠释的很好，可以看看<br>
容器可以管理对象的生命周期、对象与对象之间的依赖关系，您可以使用一个配置文件（通常是XML），在上面定义好对象的名称、如何产生（Prototype 方式或Singleton 方式）、哪个对象产生之后必须设定成为某个对象的属性等，在启动容器之后，所有的对象都可以直接取用，不用编写任何一行程序代码来产生对象，或是建立对象与对象之间的依赖关系<br>
IoC（Inversion of Control）控制反转，是一个重要的面向对象编程的法则来削减计算机程序的耦合问题，也是轻量级的Spring框架的核心，通俗地讲，IoC容器是用来管理对象之间的依赖关系的。<br>
###2.2 Ioc容器系列的实现：BeanFactory和AppplicationContext
BeanFactory定义了IoC容器的基本功能规范。如果myJndiObject是一个FactoryBean,那么使用&myJndiObject得到的是FactoryBean而不是myJndiObject这个FactoryBean产生出来的对象。
