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
##4.11基于Java类的配置
在类的定义处@Configuration注解，说明这个类可以用于为Spring提供Bean的定义信息，类的方法处可以标注@Bean注解。<br>
在XML文件中，通过<context:component-scan>可以扫描到相应的配置类。可以在@Configuration配置类中通过@ImportResource引入XML配置文件，并且通过@Autowired引用XML配置文件中定义的Bean。<br>
所以综上有基于XML，基于注解以及基于Java类这三种方式来配置。lazy-init属性的含义是延迟初始化。
第5章：Spring容器高级主题
--
2016/8/9 星期二 看到135页
##5.1Spring容器技术内幕
BeanDefinition是配置文件<bean>元素标签在容器中内部表示形式。<br>
Spring属性编辑器负责将配置文件中的文本配置值转换为Bean属性的对应值，split 方法 将一个字符串分割为子字符串，然后将结果作为字符串数组返回。PropertyEditor是属性编辑器的接口，规定了将外部设置值转化成内部JavaBean属性值的转化接口方法。其中String getAsText()将属性对象用一个字符串表示；void setAsText(String text)用一个字符串去更新属性的内部值。<br>
采用属性编辑器的一个用处就是配置Bean的属性。比如Boss类中有一个Car类型的car属性，另外一种直接的方法是在配置文件中为属性专门配置一个<bean>,然后在boss的<bean>中通过ref引用car的Bean。
##5.3使用外部属性文件
通过PropertyPlaceholderConfigurer引入属性文件，这种方式可以减少维护的工作量，并且使部署更加简单。比如：可以在配置文件中写入p:username="${userName}"。当然这种方式也可以用在基于注解以及基于Java类配置中，比如：@Value("${driverClassName}"),@Value为Bean注入一个字面值。<br>
DES属于对称加密，也就是可以根据加密后的信息解密成原值，而MD5属于非对称加密，不能根据加密后的信息还原回原值。 
##5.4引用Bean的属性值
可以通过类似于#{beanName.beanProp}的方式方便地引用另一个Bean的值。比如SysConfig.java是一个配置信息的类，该类中有maxTabPageNum这个属性，可以在beans.xml中
```java
public class Polishing {  
    int laboratory = 1;  
  
    public int getLavatory(int lavatory) {  
        return lavatory;  
    }  
  
    // Getters and setters are omitted  
} 
public class Freight {  
    int laboratory;  
    int slurry;  
    int compensatory;  
  
    // Getters and setters are omitted  
}  
```
beans.xml如下：
```xml
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xsi:schemaLocation="    
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">          
    <bean id="polishing" class="com.john.spring.Polishing" />  
    <bean id="freight" class="com.john.spring.Freight">  
        <property name="laboratory" value="#{polishing.laboratory}" />  
        <property name="slurry" value="#{polishing.getLaboratory()}" />  
        <property name="compensatory" value="#{polishing.getLavatory(4)}" />  
    </bean>  
</beans>  
```
测试类代码如下：
```java
public class Perplex {  
    public static void main(String[] args) {  
        ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");  
        Freight bean2 = (Freight) ctx.getBean("freight");  
        System.out.println(bean2.getLaboratory());  
        System.out.println(bean2.getSlurry());  
        System.out.println(bean2.getCompensatory());  
    }  
}  
```
##5.6容器事件
`Portlets`在Web门户上管理和显示的可插拔的用户界面组件。Portlet产生可以聚合到门户页面中的标记语言代码的片段，如HTML，XML等。通常，根据桌面隐喻，一个门户页面显示为一组互相不重叠的portlet窗口，其中每一个portlet窗口显示一个portlet。因此，可以说一个（或一组）portlet就像一个在门户网站上运行的基于Web的应用程序。 Portlet应用程序的一些例子包括电子邮件，天气预报，论坛和新闻等。
Portlet标准的目的是使开发人员开发出的portlet可以插入到任何支持该标准的门户网站。
`Servlet（Server Applet）`，全称Java Servlet，未有中文译文。是用Java编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态Web内容。狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。
第6章：Spring AOP基础
--
###6.2.2 JDK动态代理
JDK动态代理允许开发者在运行期间创建接口的代理实例。
JDK的动态代理主要涉及两个类：proxy和InvocationHandler，后者是一个接口，可以通过该接口定义横切逻辑，并且通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编织在一起。而Proxy利用InvocationHandler动态创建一个符合某一接口的实例。
###6.2.3 CGLib动态代理
CGLib采用非常底层的字节码技术，可以为一个类创建子类，并且在子类中采用方法拦截的技术拦截所有父类方法的调用。
使用JDK创建代理的限制就是它只能为接口创建代理实例，而CGLib可以用于任何类。
2016/8/10看到184页<br>
Spring AOP的底层就是通过使用JDK动态代理或者CGLib动态代理技术为目标Bean织入横切逻辑，由于CGLib采用动态创建子类的方式生成代理对象，所以不能对目标类中的final方法进行代理。
##6.3创建增强类
按照增强在目标类方法的连接点位置，可以分为以下5类：前置增强（BeforeAdvice）、后置增强（AfterRunningAdvice)、环绕增强（MethodInterceptor)、异常抛出增强（ThrowsAdvice）、引介增强（IntroductionInterceptor）<br>
 `前置增强`，该增强织入在方法调用之前
CGLib创建代理时速度慢，而创建出的代理对象运行效率较高，而使用JDK代理的表现正好相反。<br>
`引介增强`：该增强方法是为目标类创建新的方法和属性，通过引介增强，可以为目标类添加一个接口的实现。<br>
ThreadLocal是将非线程安全类改造成线程安全类的法宝。
##6.4 创建切面
如果只有增强，增强会被织入到目标类的所有方法中，而切点进一步描述织入到哪些类的哪些方法上。
2016/8/11看到202页<br>
Spring提供了6种类型的切点：静态方法切点，动态方法切点，注解切点，表达式切点，流程切点，复合切点。
###6.4.3 静态普通方法名匹配切面
1.编写需要植入逻辑的代码
```java
public class Waiter {
    public void greetTo(String name){
        System.out.println("waiter greet to :"+name);
    }
    public void serveTo(String name){
        System.out.println("waiter serve to :"+name);
    }
}
public class Seller {
    public void greetTo(String name){
        System.out.println("seller greet to :"+name);
    }
}
```
2.编写需要植入的逻辑即增强
```java
import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

public class RoundGreeting implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        Object[] args = methodInvocation.getArguments();
        String name = (String) args[0];
        System.out.println("how are you :Mr"+name);

        Object obj = methodInvocation.proceed();

        System.out.println("Please enjoy yourself!");

        return obj;
    }
}
```
3.定义一个切面，可以通过类，方法名，以及方法方位等信息灵活的定义切面的连接点（即添加增强的地方）
```java
import org.springframework.aop.ClassFilter;
import org.springframework.aop.support.StaticMethodMatcherPointcutAdvisor;

import java.lang.reflect.Method;

public class GreetingAdvisor extends StaticMethodMatcherPointcutAdvisor {
    @Override
    public boolean matches(Method method, Class<?> aClass) {
        return "greetTo".equals(method.getName());
    }

    @Override
    public ClassFilter getClassFilter() {
        return new ClassFilter() {
            @Override
            public boolean matches(Class<?> aClass) {
                return Waiter.class.isAssignableFrom(aClass);
            }
        };
    }
}
```
4.在xml文件中定义各种Bean
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="waiter" class="aspect.Waiter"/>
    <bean id="seller" class="aspect.Seller"/>
    <bean id="advice" class="aspect.RoundGreeting"/>

    <bean id="advisor" class="aspect.GreetingAdvisor"
          p:advice-ref="advice"
            />
    <bean id="parent" abstract="true"
          class="org.springframework.aop.framework.ProxyFactoryBean"
          p:interceptorNames="advisor"
          p:proxyTargetClass="true"
          />
    <bean id="proxyWaiter" parent="parent"
          p:target-ref="waiter"
            />
    <bean id="proxySeller" parent="parent"
            p:target-ref="seller"
            />

</beans>
```
##6.4.4静态正则表达式方法匹配切面
```xml
<!-- 正则表达式方法名匹配切面 -->  
    <bean id="regexpAdvisor"  
        class="org.springframework.aop.support.RegexpMethodPointcutAdvisor"  
        p:advice-ref="greetingAdvice">  
        <!--   
                   用正则表达式定义目标类全限定方法名（带类名的方法名）的匹配模式串   
            pattern 如果只有一个，可以用这个属性  
            patterns 定义多个匹配模式串，这些匹配模式串之间是或的关系  
            order 切面在织入时对应的顺序  
        -->  
        <property name="patterns">  
            <list>  
                <value>.*greet.*</value>  
            </list>  
        </property>  
    </bean>  
    <bean id="waiter1" class="org.springframework.aop.framework.ProxyFactoryBean"  
        p:interceptorNames="regexpAdvisor" p:target-ref="waiterTarget"  
        p:proxyTargetClass="true" />  
```
所谓静态切面是指在生成代理对象时，就确定了增强是否需要植入到目标类的连接点上，而动态切面是指必须在运行期间内根据方法入参的值来判断增强是否需要织入到目标类连接点上。<br>
作者在这里推荐了一个关于正则表达式的小工具，RegexBuddy，可以快速形成，识别正则表达式。
###6.4.6流程切面
流程切点代表由某个方法A直接或者间接发起调用的其他方法BB，希望A方法调用的其他方法BB都织入增强，此时需要使用流程切面来完成目标。流程切面和动态切面算是一类切面，因为两者都需要在运行期判断动态的环境。但是流程切点通常要比其他切点慢5到10倍。
###6.4.7复合切点切面
ComposablePointCut可以将一个复合切点和一个切点对象进行交并集运算
```java
import java.lang.reflect.Method;
import org.springframework.aop.MethodMatcher;
import org.springframework.aop.Pointcut;
import org.springframework.aop.support.ComposablePointcut;
import org.springframework.aop.support.ControlFlowPointcut;
import org.springframework.aop.support.NameMatchMethodPointcut;

public class GreetingComposablePointcut {
    public Pointcut getIntersectionPointcut(){
        ComposablePointcut cp=new ComposablePointcut();
        Pointcut pt1=new ControlFlowPointcut(WaiterDelegate.class,"service");
        MethodMatcher pt2=new NameMatchMethodPointcut(){
            public boolean matches(Method method, Class<?> cls) {// 切点方法静态匹配
                return "check".equals(method.getName());
            }
        };
        return cp.intersection(pt1).intersection(pt2);
    }
}
```
```xml
<bean id='gcp' class='com.GreetingComposablePointcut'></bean>
<bean id='composableAdvisor' class='org.springframework.aop.support.DefaultPointcutAdvisor' 
p:pointcut='#{gcp.intersectionPointcut}' p:advice-ref='greetBeforeAdvice'></bean>
<bean id='waiter3' class='org.springframework.aop.framework.ProxyFactoryBean' 
p:interceptorNames='composableAdvisor' p:target-ref='waiterTarget'/>
```
##6.5自动创建增强
 1）基于Bean配置名规则的自动代理创建器：允许为一组特定配置名的Bean自动创建代理实例的代理创建器，实现类为BeanNameAutoProxyCreator；<br>
 2）基于Advisor匹配机制的自动代理创建器：它会对容器中所有的Advisor进行扫描，自动将这些切面应用到匹配的Bean中（即为目标Bean创建代理实例），实现类为DefaultAdvisorAutoProxyCreator；<br>
 3）基于Bean中AspjectJ注解标签的自动代理创建器：为包含AspectJ注解的Bean自动创建代理实例，它的实现类是AnnotationAwareAspectJAutoProxyCreator，该类是Spring 2.0的新增类。<br>
http://www.cnblogs.com/yangyquin/p/5475664.html 这个网站对于这节讲的比较好<br>
Spring中有两种类型的Bean，一种是普通Bean，另一种是工厂Bean，即FactoryBean，这两种Bean都被容器管理，但工厂Bean跟普通Bean不同，其返回的对象不是指定类的一个实例，其返回的是该FactoryBean的getObject方法所返回的对象。在Spring框架内部，有很多地方有FactoryBean的实现类，它们在很多应用如(Spring的AOP、ORM、事务管理)及与其它第三框架(ehCache)集成时都有体现.<br>
第7章：基于@AspectJ和Schema的AOP
--
##7.3使用AspectJ
```java
package com.baobaotao.aspectj.aspectj; 
import org.aspectj.lang.annotation.Aspect; 
import org.aspectj.lang.annotation.Before; 
@Aspect ①通过该注解将PreGreetingAspect标识为一个切面 
public class PreGreetingAspect{ 
@Before("execution(* greetTo(..))") ②定义切点和增强类型 
public void beforeGreeting(){③增强的横切逻辑 
System.out.println("How are you"); 
} 
} 
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:aop="http://www.springframework.org/schema/aop" ①
xsi:schemaLocation="http://www.springframework.org/schema/beans 
                   http://www.springframework.org/schema/beans/spring-beans-2.0.xsd 
                   http://www.springframework.org/schema/aop ② 
                   http://www.springframework.org/schema/aop/spring-aop-2.0.xsd"> 
                   
    <aop:aspectj-autoproxy /> ③基于@AspectJ切面的驱动器
    <bean id="waiter" class="com.baobaotao.NaiveWaiter" />
    <bean class="com.baobaotao.aspectj.example.PreGreetingAspect" />
</beans>
```
2016/8/14 看到231页
###7.4.4不同增强类型
@Before前置增强，@AfterReturning后置增强，相当于AfterReturningAdvice,@Around环绕增强，相当于MethodInterceptor；@AfterThrowing抛出增强；@After，final增强，不管是抛出异常或者正常退出，该增强都会得到执行；@DeclareParents引介增强，相当于IntroductionInterceptor
##7.5 切点函数详解
`@annotation`表示标注了某个注解的所有方法。<br>
`execution()`是最常用的的切点函数，该函数所指定的连接点，可以大到包，小到方法入参<br>
`@args()`函数，首先定义在方法签名中入参类型在类继承树中的位置，称之为入参类型点，而标注了注解类M（标注了@M）的类在类继承树中的位置，称之为注解点。如果在类继承树中注解点高于入参类型点，则该目标方法不可能匹配切点@args（M）；反之，如果低于，则注解点所在的类及其子孙类作为方法入参时，该方法匹配@args（M)切点。<br>
`within()`该函数定义的连接点是针对目标类而言，而不是针对运行期对象的类型而言，但是within()所指定的连接点最小范围只能是类，execution()函数的功能涵盖了within（）函数的功能。<br>
`@within(M)`匹配标注了@M的类以及子孙类<br>
`@target(M)`匹配任意标注了@M的目标类<br>
`target()`切点函数通过判断目标类是否按照类型匹配指定类决定连接点是否匹配，比如Waiter是一个接口（含有serveTo()与greetTo()两个方法），NaiveWaiter与NaughtyWaiter都实现了Waiter的接口，target(XXXX.Waiter),则这两个实现类中的所有方法都匹配切点，包括Waiter中不存在的方法。<br>
`this()`一般情况下，使用this()和target()通过定义切点，两者等效。也就是target(XXX.Waiter)等价于this(XXX.Waiter),target(XXX.NaiveWaiter)等价于this(XXX.NaiveWaiter).两者的区别体现在引介切面上，如果为NaiveWaiter引介一个Seller接口的实现，则this(XXX.Seller)匹配NaiveWaiter代理对象的所有方法，而target(XXX.Seller)不匹配通过引介切面产生的NaiveWaiter代理对象。
##7.6@AspectJ进阶
###7.6.2命名切点
切点直接声明在增强方法处，这种切点声明方式为匿名切点，匿名切点只能在声明处使用。可以通过@Pointcut注解以及切面类方法对切点进行命名。
###7.6.3增强织入的顺序
1. 增强在同一个切面类中定义：依照增强在切面类中定义的顺序依次织入。 <br>
2. 增强位于不同的切面，但果这些切面都实现了org.springframework.core.Ordered 接口，则由接口注解的顺序号决定(顺序号小的先织入）<br>
3.如果增强位于不同的切面类中，且这些切面类没有实现org.springframework.core.Ordered 接口，织入的顺序不确定。<br>
2016/8/16 看到247页<br>

###7.6.4
可以采用ProceedingJoinPoint表示连接点的对象。args()、this（）、target（）、@args（）、@within（）、@target（）和@annotaion这7个函数除了可以指定类名外，还可以指定参数名，将目标对象连接点上的方法入参绑定到增强的方法中。
##7.7基于Schema配置切面
如果项目不能使用JDK5.0，那么就不能使用基于@AspectJ注解的切面了，但是可以在xml中使用Schema配置的方法，它完全可以替代基于@AspectJ注解声明切面的方法。
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xmlns:p="http://www.springframework.org/schema/p"  
    xmlns:aop="http://www.springframework.org/schema/aop"   
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd  
http://www.springframework.org/schema/aop  
     http://www.springframework.org/schema/aop/spring-aop-3.0.xsd">  
    <aop:config proxy-target-class="true">  
        <aop:aspect ref="adviceMethod">  
            <aop:before method="preGreeting" pointcut="target(com.zheng.demo7.NaiveWaiter) and execution(* greetTo(..))"/>  
        </aop:aspect>  
    </aop:config>  
    <bean id="adviceMethod" class="com.zheng.demo7.AdviceMethod"></bean>  
    <!-- 目标bean -->  
    <bean id="waiter" class="com.zheng.demo7.NaiveWaiter"></bean>  
</beans> 
```
2016/8/17 看到259页<br>
