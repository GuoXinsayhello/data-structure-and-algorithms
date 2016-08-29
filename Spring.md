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
##7.8混合切面类型
有四种定义切面的方式：<br>
1)基于@AspectJ注解的方式<br>
2)基于<aop:aspect>的方式<br>
3)基于<aop:advisor>的方式<br>
4)基于Advisor类的方式<br>
##7.9JVM Class文件字节码转换基础知识
除了在运行期通过JDK代理或者CGLib代理的方式实现织入切面，还可以在类加载期通过字节码编辑的技术，将切面织入到目标类，这种织入方式称为LTW（Load Time Weaving。）但是Spring的LTW仅支持AspectJ定义的切面。

第8章：Spring对DAO的支持
--
轻型目录存取协定（英文：Lightweight Directory Access Protocol，缩写：LDAP，英语发音：/ˈɛldæp/）
是一个开放的，中立的，工业标准的应用协议，通过IP协议提供访问控制和维护分布式信息的目录信息。<br>
 NoSQL，泛指非关系型的数据库.<br>
 `悲观锁(Pessimistic Lock)`, 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。<br>
 `乐观锁(Optimistic Lock)`, 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库如果提供类似于write_condition机制的其实都是提供的乐观锁。<br>
 `JNDI(Java Naming and Directory Interface,Java命名和目录接口)`是SUN公司提供的一种标准的Java命名系统接口，JNDI提供统一的客户端API，通过不同的访问提供者接口JNDI服务供应接口(SPI)的实现，由管理者将JNDI API映射为特定的命名服务和目录系统，使得Java应用程序可以和这些命名服务和目录服务之间进行交互<br>
` 对象关系映射`（英语：Object Relation Mapping，简称ORM，或O/RM，或O/R mapping），是一种程序技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。<br>
假设你的数据库是mysql，如果数据源配置不当，将可能发生经典的`“8小时问题”`。原因是mysql在默认情况下，如果发现一个连接的空闲时间超过8小时，将会在数据库端自动关闭这个连接。而数据源并不知道这个连接已经关闭了，当它将这个无用的连接返回给某个dao时，dao就会报无法获取connection异常。
##8.4 数据源
其一是DBCP数据源，其二是C3P0数据源。dbcp、c3p0  是两个数据库连接池，这两个连接池都是Hibernate建议使用的连接池

DBCP是一个依赖Jakarta commons-pool对象池机制的数据库连接池，Tomcat的数据源使用的就是DBCP。

C3P0是一个开放源代码的JDBC连接池，它在lib目录中与Hibernate一起发布,包括了实现jdbc3和jdbc2扩展规范说明的Connection 和Statement 池的DataSources 对象。

第9章：Spring的事务管理
--
`rollback`回滚的意思，就是数据库里做修改后 （ update  ,insert  , delete）未commit 之前,使用rollback,可以恢复数据到修改之前.
###9.1.1 数据并发的问题
脏读（dirty read）<br>
就是A事务读取B事务尚未提交的更改数据，并在此基础上进行操作。如果恰巧B事务回滚，那么A事务读到的数据是不被承认的。<br>
不可重复读（unrepeatable read）<br>
是指A事务读取了B事务已经提交的更改数据。<br>
幻象读（phantom read）<br>
A事务读取B事务提交的新增数据，这时A事务出现幻象读的问题。解决不可重复读的问题，只需要对操作的数据添加行级锁，阻止操作中的数据发生变化，而防止读取到新增数据，往往需要添加表级锁，将整个表锁定，防止新增数据<br>
第一类丢失更新<br>
A事务撤销时，把已经提交的B事务的更新数据覆盖了。<br>
第二类丢失更新<br>
A事务覆盖B事务已经提交的数据，造成B事务所做操作丢失。<br>
`文件锁定`有两种方式：共享的和独占的。多个共享锁可同时对同一文件区域发生作用；独占锁则不同，它要求其他区域不能有其他锁定再起作用。
共享锁和独占锁的经典应用，是控制最初用于读取的共享文件的更新。某个进程要读取文件，会先取得该文件或该文件部分区域的共享锁。第二个希望读取相同文件区域的进程也会请求共享锁。两个进程可以并行读取，互不影响。但是，假如有第三个进程要更新该文件，它会请求独占锁。该进程会处于阻滞状态，直到既有锁定（共享的、独占的）全部解除。一旦给予独占锁，其他共享锁的读取进程会处于阻滞状态，直到独占锁解除。这样，更新进程可以更改文件，而其他读取进程不会因为文件的更改得到前后不一致的结果<br>
###9.1.4事务隔离级别
有四个等级的事务隔离级别：READ UNCOMMITED/READ COMMITED/REPEATABLE READ/SERIALIZABLE这四种
##9.2ThreadLocal基础知识
ThreadLoacal是线程的一个本地化对象，当工作于多线程中的对象使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程分配一个独立的变量副本，每个线程可以改变自己的副本。<br>
##9.3事务管理
在Spring事务管理SPI（Service Provider Interface）高层抽象层主要包括三个接口，分别是PlatformTransactionManager，TransactionDefinition，TransactionStatus<br>
持久化（Persistence），即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在的数据库中，或者存储在磁盘文件中、XML数据文件中等<br>
2016/8/19 看到303页<br>
JPA全称Java Persistence API.JPA通过JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。<br>
JTA，即Java Transaction API，JTA允许应用程序执行分布式事务处理——在两个或多个网络计算机资源上访问并且更新数据。JDBC驱动程序的JTA支持极大地增强了数据访问能力。
###9.3.4事务同步管理器
 Spring的`事务同步管理器`SynchronizationManager使用ThreadLocal为不同事务线程提供了独立的资源副本，同时维护事务配置的属性和运行状态信息。事务同步管理器是Spring事务管理的基石部分，不管用户使用编程式事务管理，还是声明式事务管理，都离不开事务同步管理器.可以通过管理器的静态方法获取当前线程绑定的会话（session）或连接（connection）
 ##9.4编程式事务管理
 ###基于底层 API 的编程式事务管理
 根据PlatformTransactionManager、TransactionDefinition 和 TransactionStatus 三个核心接口，我们完全可以通过编程的方式来进行事务管理。示例代码如清单4所示：<br>
 ```java
 public class BankServiceImpl implements BankService {
private BankDao bankDao;
private TransactionDefinition txDefinition;
private PlatformTransactionManager txManager;
......
public boolean transfer(Long fromId， Long toId， double amount) {
TransactionStatus txStatus = txManager.getTransaction(txDefinition);
boolean result = false;
try {
result = bankDao.transfer(fromId， toId， amount);
txManager.commit(txStatus);
} catch (Exception e) {
result = false;
txManager.rollback(txStatus);
System.out.println("Transfer Error!");
}
return result;
}
}
```
```xml
<bean id="bankService" class="footmark.spring.core.tx.programmatic.origin.BankServiceImpl">
<property name="bankDao" ref="bankDao"/>
<property name="txManager" ref="transactionManager"/>
<property name="txDefinition">
<bean class="org.springframework.transaction.support.DefaultTransactionDefinition">
<property name="propagationBehaviorName" value="PROPAGATION_REQUIRED"/>
</bean>
</property>
</bean>
```
###基于 TransactionTemplate 的编程式事务管理
`模板回调模式`<br>
```java
public class BankServiceImpl implements BankService {
private BankDao bankDao;
private TransactionTemplate transactionTemplate;
......
public boolean transfer(final Long fromId， final Long toId， final double amount) {
return (Boolean) transactionTemplate.execute(new TransactionCallback(){
public Object doInTransaction(TransactionStatus status) {
Object result;
try {
result = bankDao.transfer(fromId， toId， amount);
} catch (Exception e) {
status.setRollbackOnly();
result = false;
System.out.println("Transfer Error!");
}
return result;
}
});
}
}
```
```xml
<bean id="bankService"
class="footmark.spring.core.tx.programmatic.template.BankServiceImpl">
<property name="bankDao" ref="bankDao"/>
<property name="transactionTemplate" ref="transactionTemplate"/>
</bean>
```

 ##9.5使用XMl配置声明式事务
 Spring 的声明式事务管理在底层是建立在 AOP 的基础之上的。其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务。
声明式事务最大的优点就是不需要通过编程的方式管理事务，这样就不需要在业务逻辑代码中掺杂事务管理的代码，只需在配置文件中做相关的事务规则声明（或通过等价的基于标注的方式），便可以将事务规则应用到业务逻辑中.建议采用声明式事务。和编程式事务相比，声明式事务唯一不足地方是，后者的最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。<br>
 在Spring早期版本，用户必须通过TransactionProxyFactoryBean代理类对需要事务管理的业务类进行代理。然而后来可以通过tx/aop命名空间进行配置。
 ```xml
 <!-- 配置事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager"
		p:dataSource-ref="dataSource">
	</bean>

	<!-- 通过AOP配置提供事务增强，让service包下所有Bean的所有方法拥有事务 -->
	<aop:config proxy-target-class="true">
		<aop:pointcut id="serviceMethod"
			expression=" execution(* net.yingyi.games.service..*(..))" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="serviceMethod" />
	</aop:config>
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="*" />
		</tx:attributes>
	</tx:advice>

	<!-- 定义JdbcTemplate的Bean -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"
		p:dataSource-ref="dataSource">
	</bean>
	
	<!-- 需要实施事务增强的目标业务Bean -->
	<bean id="userScore"
		class="net.hingyi.springDemo.transaction.service.UserScoreServiceImpl"
		p:userScoreRepository-ref="userScoreRepository_jdbc" />
	
	<bean id="userScoreRepository_jdbc"
		class="net.hingyi.springDemo.transaction.repository.UserScoreRepositoryImpl"
         p:jdbcTemplate-ref="jdbcTemplate" />
```

##9.6使用注解配置声明式事务
 除了基于XMl的事务配置之外，还可以通过注解进行事务配置，此时需要在业务类加上@Transactional的注解符号，此外还需要对xml配置文件加上
 ```xml
 <tx:annotation-driven transaction-manager="txManager" />
 ```
 虽然@Transactional可以用在接口上，但是Spring并不建议这么做，因为注解不能被继承，所以业务接口中标注的@Transactional注解不会被实现的业务类继承，所以最好在具体业务类上使用@Transactional注解。
 2016/8/20 看到321页
 
第10章:Spring 的事务管理难点剖析
--
在相同线程中进行相互嵌套调用的事务方法工作于相同的事务中，如果这些相互嵌套调用的方法工作在不同的线程中，则不同线程下的事务方法工作在独立的事务中。
##10.5混合框架
如果要同时使用Hibernate和Spring JDBC读写数据，要考虑到Hibernate缓存机制引发的问题，及时调用Hibernate的flush(）方法。<br>
2016/8/21 看到345页
##10.6 特殊方法成为漏网之鱼
由于使用final、static、private修饰符的方法都不能被子类覆盖，相应的这些方法无法实施基于CGLib动态代理的AOP增强；而基于接口的动态代理策略，除public外的其他所有的方法不能被事务增强，此外public static也不能被增强。
##10.7数据连接泄露
如果从数据源直接获取连接（比如jdbcTemplate.getDataSource().getConnection()) ，并且在使用完成后不主动归还给数据源(调用Connection#close())，则会造成数据连接泄露的问题。Spring提供了一个可以从当前事务上下文中获取绑定的数据连接的工具类，就是DataSourceUtils，该工具类在存在事务上下文的方法中貌似不会造成数据连接泄露，但是在没有事务上下文的方法中使用仍然会造成数据连接泄露，此时就需要显式调用DataSourceUtils.releaseConnection()方法来释放获取的连接。作者特别强调要在finally（也就是try，catch，finally那个地方）里面释放连接，因为如果在try里面最后释放连接，如果之前的语句发生异常，那么释放连接就不能被正常执行。尽量使用JdbcTemplate、HibernateTemplate这些模板进行数据访问操作，避免直接获取数据连接的操作。
第11章：使用Spring JDBC访问数据库
--
关于ENGINE=InnoDB/MyISAM  MyISAM类型不支持事务处理等高级处理，而InnoDB类型支持。 MyISAM类型的表强调的是性能，其执行数度比InnoDB类型更快，但是不提供事务支持，而InnoDB提供事务支持已经外部键等高级数据库功能。<br>
下面这段代码已经能够实现向数据库中插入一条数据，不过貌似没有用JDBCTemplate
```JAVA
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class Mysql {

    /**
     * 入口函数
     * @param arg
     */
    public static void main(String arg[]) {
        try {
            Connection con = null; //定义一个MYSQL链接对象
            Class.forName("com.mysql.jdbc.Driver").newInstance(); //MYSQL驱动
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/sampledb", "root", ""); //链接本地MYSQL

            Statement stmt; //创建声明
            stmt = con.createStatement();

            //新增一条数据
            stmt.executeUpdate("INSERT INTO customer (CUST_ID, NAME,AGE) VALUES ('2', 'KOBE','38')");
            ResultSet res = stmt.executeQuery("select LAST_INSERT_ID()");
            int ret_id;
            if (res.next()) {
                ret_id = res.getInt(1);
                System.out.print(ret_id);
            }

        } catch (Exception e) {
            System.out.print("MYSQL ERROR:" + e.getMessage());
        }

    }
}
```
使用JDBC连接数据库的步骤如下：<br>
定义连接参数（包括动态加载驱动类com.mysql.jdbc.Driver）<br>
建立数据库连接<br>
指定Sql语句并参数化<br>
执行Sql语句<br>
获取查询结果并处理<br>
处理异常<br>
事务管理<br>
释放各类资源——Statement，ResultSet，Connection<br>
下面这个例子对于JDBDTemplate解释的比较详细<br>
http://wiki.jikexueyuan.com/project/spring/jdbc-framework-overview/spring-jdbc-example.html<br>
##11.2 基本的数据操作
VARCHAR是一种比CHAR更加灵活的数据类型，同样用于表示字符数据，但是VARCHAR可以保存可变长度的字符串。 
```java
public void print(String... args) {
        for (int i = 0; i < args.length; i++) {
            out.println(args[i]);
        }
    }
```
上述代码的意思是参数的个数不定。<br>
复合主键中所有字段联合起来才能唯一标示一条记录。<br>
###11.2.1 更改、查询数据
JDBCTemplate提供了若干个update（）方法来对数据进行更改和删除。Spring为KeyHolder接口指代了一个通用的实现类GeneratedKeyHolder，该类返回新增记录时的自增长主键值。如果需要一次性插入或更新多条数据，可以使用batchUpdate方法。如果需要查询数据可以用JDBCTemplate的void query(String sql,Object[] args,RowCallbackHandler rch)方法，使用RowCallbackHandler处理结果集，还有功能类似的RowMapper<T>来处理结果集。
###11.2.5查询单值数据
下面是获取int单值的例子<br>
```java
public int getForumNum()
{
 String sql="SELECT COUNT(*) FROM t_forum";
 return getJdbcTemplate().queryForInt(sql);
}
```
delimiter可以用在mysql里面使程序以其他设定的符号结束后执行，而不是以默认的分号结束后执行。
###11.2.6调用存储过程
https://blog.tankywoo.com/2015/04/01/mysql-stored-procedure.html 这个网址对于mysql的存储过程介绍的比较详细，个人理解mysql存储过程就像是函数，有人是这样理解的：<br>
我们常用的操作数据库语言SQL语句在执行的时候需要要先编译，然后执行，而存储过程（Stored Procedure）是一组为了完成特定功能的SQL语句集，经编译后存储在数据库中，
用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它。
##11.3BLOB/CLOB类型数据的操作
LOB代表大对象数据，包括BLOB和CLOB两种类型，前者用于存储大块的二进制数据，如图片数据，视频数据等；而后者用于存储长文本数据，如论坛的帖子内容，产品描述等。<br>
Spring定义了以统一的方式操作各种数据库Lob类型数据的LobCreator接口，此外LobHandler接口为操作大二进制字段和大文本字段提供了统一访问接口。
##11.5其他类型的JDBCTemplate
Spring除了JDBCTemplate还提供了NamedParameterJDBCTemplate以及SimpleJDBCTemplate，前者提供命名参数绑定的功能，后者封装了JDBCTemplate，将常用的API开放出来。
##11.6以OO的方式访问数据库
用JDBCTemplate方式也能和以OO方式完成相似功能，只不过OO方式更加面向对象，是一种可以选择的方案。但是OO方式没什么优势，作者说是鸡肋。
第12章：整合其他ORM框架
--
##12.2在spring中使用Hibernate
`零过渡障碍的配置方式`<br>
首先编写对象关系的映射文件xxx.hbm.xml，然后通过Hibernate的配置文件hibernate.cfg.xml将所有xxx.hbm.xml映射文件组装起来，然后在spring配置文件中指定Hibernate配置文件。
`更具Spring风格的配置`<br>
也可以在Spring容器中定义数据源、指定映射文件、配置Hibernate控制属性等信息，完全抛开hibernate.cfg.xml配置文件<br>
http://blog.csdn.net/chenssy/article/details/7728367 这篇博客对于Hibernate的HQL查询做了比较清晰的阐释，HQL是HIbernate的专有查询，相对于SQL语句的查询，HQL是完全面向对象的查询。<br>
SessionFactory可以用来创建session，一般一个应用只有一个SessionFactory，并且它是线程安全的，如果应用中有多个数据库，则需要对于每个数据库都要设置一个SessionFactory对象。<br>
`映射文件`指示 Hibernate 如何将已经定义的类或类组与数据库中的表对应起来。
尽管有些 Hibernate 用户选择手写 XML 文件，但是有很多工具可以用来给先进的 Hibernate 用户生成映射文件。这样的工具包括 XDoclet, Middlegen 和 AndroMDA。http://wiki.jikexueyuan.com/project/hibernate/mapping-files.html 这个网站对于Hibernate的映射文件讲的很详细。
第13章：任务调度和异步执行器
--
##13.2 Quartz快速入门
http://www.blogjava.net/jzone/articles/322015.html 介绍了Quartz<br>
Job：是一个接口，表示要执行什么任务，有一个execute的方法<br>
JobDetail:具体是要执行什么任务，那么Jobs与JobDetails有什么关系呢？简而言之，Job是对任务的一个具体实现；谁执行了这个任务呢？这就需要JobDetail来实现，所以JobDetail就是Job的一个具体实例；如9点上语文课是一个具体任务，而刘老师在9点上语文课就是这个任务的一个具体实例。 <br>
Trigger:以什么样的方法去调度（时间，重复次数等）,主要有SimpleTrigger与CronTrigger之分，后者可以自定义表达式定义出复杂的调度方案<br>
Calendar:特定的的时间集合<br>
Scheduler:一个总的容器，Trigger和JobDetail可以注册到该容器当中。<br>
ThreadPool:线程池。<br>
Job中无法定义属性来传递数据，那么如果我们需要传递数据该怎么做？就是使用JobDataMap，它是JobDetail的一个属性。JobDataMap是Map接口的一个实现，并且它有一些便利的方法来储存和检索基本类型数据。 
修改之前的测试类如下：
```java
JobDetail jobDetail = JobBuilder.newJob(HelloJob.class)
                .withIdentity("helloJob", "group1")
                .usingJobData("name", "孙悟空")
                .build();
```
```java
@Override
    public void execute(JobExecutionContext context) throws JobExecutionException {
        JobDataMap jobDataMap = context.getJobDetail().getJobDataMap();
        String name = jobDataMap.getString("name");
        System.out.println("hello "+name+", "+ DateFormatUtils.format(new Date(), "yyyy-MM-dd HH:mm:ss"));
    }
```
###13.2.3 使用CronTrigger
Cron表达式
2016/8/26 看到446页
##13.4 Spring中使用JDK Timer
TimerTask代表一个需要多次执行的任务，它实现了Runnable接口，可以在run()方法中定义任务逻辑，而Timer负责制定调度规则并调度TimerTask。个人理解，Timer背景线程中有很多TimerTask,
##13.5 Spring对JDK5.0 Executor的支持
EXecutor接口的主要目的是要将“任务提交”与“任务执行”两者分离解耦，也就是说提交和执行在不同的线程中执行，当然也可以在一个线程中执行。<br>
线程池可以让多个任务重用同一个线程，线程创建的开销相当于被分摊到了多个任务上。
##13.6 实际应用中的任务调度
产生任务有两种方法：在业务流程中产生，由扫描任务周期性产生。前者的意思就是说把所有任务都准确安排好，对于时间精度把握比较高，适用于任务执行与任务安排的时间间隔不大的情况下，后者就是以固定的频率去扫描看有什么任务。<br>
静态变量是ClassLoader级别的，如果Web应用程序停止，这些静态变量也会从JVM中清除。但是线程则是JVM级别的，如果你在Web 应用中启动一个线程，这个线程的生命周期并不会和Web应用程序保持同步。也就是说，即使你停止了Web应用，这个线程依旧是活跃的。

第14章：使用OXM进行对象XML映射
--
##14.2 XML处理利器：XStream
XStream是Java对象和XML之间一个双向转换器。instanceof 针对实例 。isAssignableFrom针对class对象。isAssignableFrom   是用来判断一个类Class1和另一个类Class2是否相同或是另一个类的超类或接口。通常用法如下：<br>
```java
Class1.isAssignableFrom(Class2)   
```
不仅可以采用编码的方式对XML进行转换，而且可以采用基于注解的方式进行转换。<br>
编码方式：<br>
```java
  public static void objectToXml()throws Exception{
    	User user = getUser();
        FileOutputStream fs = new FileOutputStream("D:\\masterSpring\\chapter14\\out\\XStreamAliasSample.xml");
        xstream.toXML(user, fs);
    }
```
注解方式：<br>
```java
public class User {
	@XStreamAsAttribute
	@XStreamAlias("id")
	private int userId;
	
	@XStreamAlias("username")
	private String userName;
	
	@XStreamAlias("password")
	private String password;
	
	@XStreamAlias("credits")
	private int credits;
	
	@XStreamOmitField
	@XStreamAlias("lastIp")
	private String lastIp;

	@XStreamConverter(DateConverter.class)
	private Date lastVisit;

	@XStreamImplicit
	private List logs;
	
	...
	}
```
