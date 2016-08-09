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
spring容器帮助完成类的初始化与装配工作，让开发者从这些底层实现类的实例化，依赖关系装配等工作中脱离，spring容器通过配置文件或者注解描述类和类之间的依赖。
####Java反射机制实例
```java
package com.test;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.lang.reflect.Field;

public class Interval {
	public static Solution  initByDefaultConst() throws Throwable  
    {  
        //①通过类装载器获取Car类对象  
        ClassLoader loader = Thread.currentThread().getContextClassLoader();   
        Class clazz = loader.loadClass("com.test.Solution");   
          //②获取类的默认构造器对象并通过它实例化Car  
        Constructor cons = clazz.getDeclaredConstructor((Class[])null);   
        Solution car = (Solution)cons.newInstance();  
          //③通过反射方法设置属性  
        Method setBrand = clazz.getMethod("setBrand",String.class);          
        setBrand.invoke(car,"红旗CA72");        
        Method setColor = clazz.getMethod("setColor",String.class);  
        setColor.invoke(car,"黑色");        
        Method setMaxSpeed = clazz.getMethod("setMaxSpeed",int.class);  
        setMaxSpeed.invoke(car,200);          
        return car;  
    }  
    public static void main(String[] args) throws Throwable {  
        Solution car = initByDefaultConst();  
        car.introduce();  
    }  
	 
}

public class Solution {
   
	 private String brand;  
	    private String color;  
	    private int maxSpeed;  
	      
	     //①默认构造函数  
	    public Solution(){}  
	       
	     //②带参构造函数  
	    public Solution(String brand,String color,int maxSpeed){   
	        this.brand = brand;  
	        this.color = color;  
	        this.maxSpeed = maxSpeed;  
	    }  
	    public void setBrand(String brand)
	    {
	    	this.brand=brand;
	    }
	    public void setColor(String color)
	    {
	    	this.color=color;
	    }
	    public void setMaxSpeed(int speed)
	    {
	    	this.maxSpeed=speed;
	    }
	  
	     //③未带参的方法  
	    public void introduce() {   
	       System.out.println("brand:"+brand+";color:"+color+";maxSpeed:" +maxSpeed);  
	    } 
}

```
类装载器就是寻找类的节码文件并构造出类类在JVM内部表示对象的组件。JVM在运行时会产生三个ClassLoader：根装载器，ExtClassLoader和AppClassLoader，这三个互为父子层级。该书在50页介绍了一个小工具来查看JVM从哪个类包加载指定类。<br>
通过类实例变量无法在外部访问私有变量，调用私有方法，但是通过反射机制可以绕过限制。<br>
Spring框架使用resource装载各种资源。
2016/7/23看到55页<br>
Spring不仅可以通过资源地址前缀识别不同的资源类型，还支持Ant风格带通配符的资源地址。Ant风格资源地址支持三种通配符：？：匹配文件名中的一个字符，*：匹配文件名中任意个字符；**：匹配多层路径<br>
####Beanfactory和Applicationcontext
也将ApplicationContext称为Spring容器，一般称呼BeanFactory为IoC容器。BeanFactory是一个类工厂，可以创建管理各种类的对象。BeanFactory在初始化容器时并未实例化bean,直到第一次访问某个Bean时才实例目标Bean；而ApplicationContext则在初始化应用上下文时就实例化所有单实例的bean.<br>
父子容器中，子容器可以访问父容器中的bean,但是父容器不能访问子容器的Bean。<br>
2016/8/6 看到76页<br>
WEB-INF是Java的WEB应用的安全目录。
第4章：在IoC容器中装配Bean
--
##4.1Spring配置概述
###4.1.2基于xml的配置
spring2.0以后使用schema的格式对xml进行配置，xmlns:xsi ——是指xml文件遵守xml规范，xsi全名：xml schema instance。命名空间的别名和全限定名可以任意命名。
##4.3依赖注入
依赖注入有属性注入和构造函数注入以及工厂方法注入三种。
###4.3.1属性注入
属性注入就是通过setXxx()方法注入Bean的属性值或者依赖对象，该输入方式要求Bean提供一个默认的构造函数，并且为需要注入的属性提供对应的Setter方法，如果类中已经有了一个带参数的构造函数，则需要同时提供一个默认的构造函数。JavaBean属性的命名规范是：xxx的属性对应setXxx()方法，并且一个特殊规范指出变量的前两个字母要么全都是大写，要么全都是小写。
###4.3.2构造函数注入
构造函数注入保证一些必要的属性在Bean实例化的时候就得到设置，保证Bean实例在实例化后就可以使用并且通过构造函数注入与属性注入的xml显示内容会有所不同，前者会有constructor-arg，后者是property 标签。<br>
该注入方式有按`类型匹配入参`以及`按索引匹配入参`两种，前者xml中通过type属性区分，后者通过index区分，后者可以在构造函数参数类型相同的情况下进行区分。<br>
2016/8/6 看到91页<br>
如果Bean构造函数入参的类型可以辨别（非基础类型且入参类型各异），就可以不提供类型和索引的信息，通过`Java反射机制`获取构造函数入参类型。
构造参数注入有可能出现循环依赖问题，可以通过将构造函数注入的方式调整为属性注入的方式就可以解决。
###4.3.3工厂方法注入
对于非静态工厂方法，需要定义一个工厂类的Bean，而对于静态工厂方法，就无须在配置文件中定义工厂类的Bean。工厂方法注入的方式不推荐。
##4.4注入参数详解
在spring配置文件中，用户可以将String，int等字面值注入到Bean中，还可以将集合、Map等类型的数据注入，此外还可以注入配置文件中其他定义的Bean如果字面值注入含有xml文件的特殊符号(&<>"'这五个)，就需要对其进行特殊处理，一种方法是采用<![CDATA[XXXXX]]>让xml解析器对中括号中的字符当做普通文本对待，另外一种方法是采用转义字符，并且在赋值时spring不会忽略空格。Bean通过<ref>标签引用其他的Bean。
第一种形式也是最常见的形式是通过使用<ref/>标记指定bean属性的目标bean，通过该标签可以引用同一容器或父容器内的任何bean（无论是否在同一XML文件中）。XML 'bean'元素的值既可以是指定bean的id值也可以是其name值。
```xml
<ref bean="someBean"/>
```
第二种形式是使用ref的local属性指定目标bean，它可以利用XML解析器来验证所引用的bean是否存在同一文件中。local属性值必须是目标bean的id属性值。如果在同一配置文件中没有找到引用的bean，XML解析器将抛出一个例外。如果目标bean是在同一文件内，使用local方式就是最好的选择（为了尽早地发现错误）。
```xml
<ref local="someBean"/>
```
第三种方式是通过使用ref的parent属性来引用当前容器的父容器中的bean。parent属性值既可以是目标bean的id值，也可以是name属性值。而且目标bean必须在当前容器的父容器中。使用parent属性的主要用途是为了用某个与父容器中的bean同名的代理来包装父容器中的一个bean(例如，子上下文中的一个bean定义覆盖了他的父bean)。<br>
通过util命名空间配置集合类型的Bean，Bean可以有list或者set，map类型等.采用p命名空间会进一步简化XML文件的配置，采用bean的元素属性配置bean的属性，对于字面值属性，其格式为p:属性名="xxx",对于引用对象的属性，其格式为p:属性名-ref="xxx"
##4.5方法注入
###4.5.1lookup方法注入
当Bean依赖另一个生命周期不同的bean，尤其是当singleton依赖一个non-singleton时，常会遇到不少问题，Lookup Method Injection正是对付这些问题而出现的，在上述情况中，setter和构造注入都会导致singleton去维护一个non-singleton bean的单个实例，某些情况下，我们希望让singleton bean每次要求获得bean时候都返回一个non-singleton bean的新实例 <br>
应用场合：一个singleton的Bean需要引用一个prototype的Bean; 一个无状态的Bean需要引用一个有状态的Bean; ... ; 等等情景下. 
###4.5.2方法替换
用户可以使用某个Bean（比如Bean A）的方法去替换另外一个Bean（比如Bean B）的方法,此时Bean A必须实现MethodReplacer接口
##4.6bean之间的关系
###4.6.1继承
继承就是应用了OOP的思想，把具有相同的属性抽象为一个父bean，然后子Bean继承。
###4.6.2依赖
Spring允许用户通过depends-on属性指定Bean前置依赖的Bean，前置依赖的Bean会在本Bean实例化之前创建好。
###4.6.3引用
 <bean id="exampleBean2" class="com.company.wws.test.ExampleBean2"/> 
<bean id="exampleBean1" class="com.company.wws.test.ExampleBean1">  
        <property name="exampleId"><idref bean="exampleBean2"/></property>  
    </bean>  
##4.8 Bean作用域
Bean的作用域类型有singleton，prototype，request，session，globalsession这五种，后三种仅限于WebAppliocationContext<br>
 Spring之ContextLoaderListener的作用,在启动Web容器时，自动装配Spring applicationContext.xml的配置信息。其中request作用域的Bean对应一个HTTP请求和生命周期，如果作用域是session的话，Session中所有HTTP请求共享一个Bean，如果要将Web相关作用域的Bean注入到singleton或者prototye的Bean中，需要Spring AOP中的代理，也就是在Web作用域中的Bean设置
 ```xml
 <aop:scoped-proxy/>
 ```
 2016/8/9看到118页
##4.9 FactoryBean
可以编写一个Java类实现FactoryBean接口，然后就可以在Bean.xml文件中以逗号分隔的方式简单地配置自己的Car Bean了。
##4.10 基于注解的配置
@Component<br>
@Repository 用于对DAO实现类进行标注<br>
@Service 用于对Service实现类进行标注<br>
@Controller 用于对Controller实现类进行标注<br>
后三个功能比较详细，@Component功能比较宽泛
###4.10.3 自动装配Bean
使用@Autowired进行自动注入，该注解默认按照类型匹配的方式；使用@Qualifier指定注入Bean的名称，用于当有一个以上匹配的Bean的时候，@Qualifier("userDao");@Scope("xx")可以对作用范围进行指定;@PostConstruct与@PreDestroy可以指定初始化以及容器销毁前执行的方法；
#4.11基于Java类的配置
在类的定义处@Configuration注解，说明这个类可以用于为Spring提供Bean的定义信息，类的方法处可以标注@Bean注解。<br>
在XML文件中，通过<context:component-scan>可以扫描到相应的配置类。可以在@Configuration配置类中通过@ImportResource引入XML配置文件，并且通过@Autowired引用XML配置文件中定义的Bean。<br>
所以综上有基于XML，基于注解以及基于Java类这三种方式来配置。lazy-init属性的含义是延迟初始化。
第5章：Spring容器高级主题
--
2016/8/9 星期二 看到135页
