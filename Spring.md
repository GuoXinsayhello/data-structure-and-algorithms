Spring 3.x 企业应用开发实战
==
第1章：Spring概述
--
SVN是Subversion的简称，是一个开放源代码的版本控制系统，相较于RCS、CVS，它采用了分支管理系统，它的设计目标就是取代CVS。互联网上很多版本
控制服务已从CVS迁移到Subversion。说得简单一点SVN就是用于多个人共同开发同一个项目，共用资源的目的。<br>
http://www.cnblogs.com/fuchongjundream/p/3873073.html 对于IoC模式诠释的很好，可以看看<br>
容器可以管理对象的生命周期、对象与对象之间的依赖关系，您可以使用一个配置文件（通常是XML），在上面定义好对象的名称、如何产生（Prototype 方式或Singleton 方式）、哪个对象产生之后必须设定成为某个对象的属性等，在启动容器之后，所有的对象都可以直接取用，不用编写任何一行程序代码来产生对象，或是建立对象与对象之间的依赖关系<br>
IoC（Inversion of Control）控制反转，是一个重要的面向对象编程的法则来削减计算机程序的耦合问题，也是轻量级的Spring框架的核心，通俗地讲，IoC容器是用来管理对象之间的依赖关系的。<br>
`BeanFactory`定义了IoC容器的基本功能规范。如果myJndiObject是一个FactoryBean,那么使用&myJndiObject得到的是FactoryBean而不是myJndiObject这个FactoryBean产生出来的对象。<br>
Spring是以IoC（Inverse of Control）和AOP（Aspect Oriented Programming:面向切面编程）为内核。`DAO(Data Access Object)`是一个数据访问接口，数据访问：顾名思义就是与数据库打交道。夹在业务逻辑与数据库资源中间。<br>
`xmlns` ——是XML NameSpace的缩写，因为XML文件的标签名称都是自定义的，自己写的和其他人定义的标签很有可能会重复命名，而功能却不一样，所以需要加上一个namespace来区分这个xml文件和其他的xml文件，类似于java中的package。<br>
`xmlns:xsi` ——是指xml文件遵守xml规范，xsi全名：xml schema instance
第2章：快速入门
--
spring中的DAO是指数据访问对象（data access object），DBCP（DataBase Connection Pool）数据库连接池，是java数据库连接池的一种，由Apache开发，通过数据库连接池，可以让程序自动管理数据库连接的释放和断开。<br>
持久层负责数据的访问和操作。所谓的三层开发就是将系统的整个业务应用划分为表示层，业务逻辑层和数据访问层。<br>
`REST架构风格`最重要的架构约束有6个：

客户-服务器（Client-Server）
通信只能由客户端单方面发起，表现为请求-响应的形式。

无状态（Stateless）
通信的会话状态（Session State）应该全部由客户端负责维护。

缓存（Cache）
响应内容可以在通信链的某处被缓存，以改善网络效率。

统一接口（Uniform Interface）
通信链的组件之间通过统一的接口相互通信，以提高交互的可见性。

分层系统（Layered System）
通过限制组件的行为（即，每个组件只能“看到”与其交互的紧邻层），将架构分解为若干等级的层。

按需代码（Code-On-Demand，可选）
支持通过下载并执行一些代码（例如Java Applet、Flash或JavaScript），对客户端的功能进行扩展。<br>
Servlet（Server Applet），全称Java Servlet，未有中文译文。是用Java编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态Web内容.<br>
POJO（Plain Ordinary Java Object）简单的Java对象，实际就是普通JavaBeans，是为了避免和EJB混淆所创造的简称<br>
第3章：IoC容器概述
--
IoC（Inverse of control)对于软件来说，就是某一个接口的具体实现类的选择控制权从调用类中移除，转交给第三方决定。DI（dependency injection）的概念也可以代替IoC，从注入方法上来看主要分为构造函数注入，属性注入，接口注入。<br>
####构造函数注入
```java
public class MoAttack
{
 private Geli geli;
 public MoAttack(Geli geli){
  this.geli=geli;
 }
 public void cityAttack(){
  geli.responseAsk("kkk");
  }
}

public class Director {  
   public void direct(){  
        //①指定角色的扮演者  
       GeLi geli = new LiuDeHua();    
  
        //②注入具体扮演者到剧本中  
       MoAttack moAttack = new MoAttack(geli);   
       moAttack.cityGateAsk();  
   }  
}  
```
####属性注入
```java
public class MoAttack {  
    private GeLi geli;  
     //①属性注入方法  
    public void setGeli(GeLi geli) {    
        this.geli = geli;  
    }  
    public void cityGateAsk() {  
        geli.responseAsk("墨者革离");  
    }  
}
public class Director {  
   public void direct(){  
       GeLi geli = new LiuDeHua();  
       MoAttack moAttack = new MoAttack();  
  
        //①调用属性Setter方法注入  
       moAttack.setGeli(geli);   
       moAttack.cityGateAsk();  
   }  
}  
```
####接口注入
```java
public interface ActorArrangable {  
   void injectGeli(GeLi geli);  
}  
public class MoAttack implements ActorArrangable {  
    private GeLi geli;  
     //①实现接口方法  
    public void injectGeli (GeLi geli) {    
        this.geli = geli;         
    }  
    public void cityGateAsk() {  
        geli.responseAsk("墨者革离");  
    }  
}
public class Director {  
   public void direct(){  
       GeLi geli = new LiuDeHua();  
       MoAttack moAttack = new MoAttack();  
       moAttack. injectGeli (geli);  
       moAttack.cityGateAsk();  
   }  
}  
```
