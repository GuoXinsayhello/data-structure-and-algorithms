精通Hibernate
==
1.简介
--
ORM 表示 Object-Relational Mapping (ORM)，是一个方便在关系数据库和类似于 Java， C# 等面向对象的编程语言中转换数据的技术。Hibernate 是传统 Java 对象和数据库服务器之间的桥梁，用来处理基于 O/R 映射机制和模式的那些对象。<br>
没有JNDI的做法存在的问题： <br>
1、数据库服务器名称MyDBServer 、用户名和口令都可能需要改变，由此引发JDBC URL需要修改； <br>
2、数据库可能改用别的产品，如改用DB2或者Oracle，引发JDBC驱动程序包和类名需要修改； <br>
3、随着实际使用终端的增加，原配置的连接池参数可能需要调整；等等一系列问题 <br>
因此就出现了JNDI，首先，在在J2EE容器中配置JNDI参数，定义一个数据源，也就是JDBC引用参数，给这个数据源设置一个名称；然后，在程序中，通过数据源名称引用数据源从而访问后台数据库。 <br>
JNDI的作用如下：<br>
1、JNDI 提出的目的是为了解藕，是为了开发更加容易维护，容易扩展，容易部署的应用。 <br>
2、JNDI 是一个sun提出的一个规范(类似于jdbc),具体的实现是各个j2ee容器提供商，sun   只是要求，j2ee容器必须有JNDI这样的功能。 <br>
3、JNDI 在j2ee系统中的角色是“交换机”，是J2EE组件在运行时间接地查找其他组件、资源或服务的通用机制。<br> 
4、JNDI 是通过资源的名字来查找的，资源的名字在整个j2ee应用中(j2ee容器中)是唯一的。<br>

映射文件中的meta标签不会直接影响映射，相反，这个标签为使用其他不同工具提供额外的信息。generator标签用于设置HIbernate如何为新的实例创建id值，可以选择不同的ID生成策略。<br>
可以通过映射文件反向生成java实体类，而映射文件可以通过一些工具自动生成，

2.会话
--
Session 用于获取与数据库的物理连接。<br>
Query 对象：Query 对象使用 SQL 或者 Hibernate 查询语言（HQL）字符串在数据库中来检索数据并创造对象。一个查询的实例被用于连结查询参数，限制由查询返回的结果数量，并最终执行查询。<br>
Criteria 对象:Criteria 对象被用于创造和执行面向规则查询的对象来检索对象。<br>
Hibernate 的完整概念是提取 Java 类属性中的值，并且将它们保存到数据库表单中

3.持久化类
--
在 Hibernate 中，其对象或实例将会被存储在数据库表单中的 Java 类被称为持久化类。<br>
以下所列就是持久化类的主要规则，然而，在这些规则中，没有一条是硬性要求。<br>
所有将被持久化的 Java 类都需要一个默认的构造函数。<br>
为了使对象能够在 Hibernate 和数据库中容易识别，所有类都需要包含一个 ID。此属性映射到数据库表的主键列。<br>
所有将被持久化的属性都应该声明为 private，并具有由 JavaBean 风格定义的 getXXX 和 setXXX 方法。<br>
Hibernate 的一个重要特征为代理，它取决于该持久化类是处于非 final 的，还是处于一个所有方法都声明为 public 的接口。<br>
所有的类是不可扩展或按 EJB 要求实现的一些特殊的类和接口。<br>

4.映射文件
--
作者说XDoclet, Middlegen 和 AndroMDA等等这些工具可以给HIbernate用户自动生成映射文件

5.O/R映射
--
集合映射，关联映射，组件映射
6.注释
--
@Entity 注释，标志着这个类为一个实体 bean，所以它必须含有一个没有参数的构造函数并且在可保护范围是可见的。<br>
@Table 注释允许您明确表的详细信息保证实体在数据库中持续存在。<br>
@Id每一个实体 bean 都有一个主键，你在类中可以用 @Id 来进行注释。<br>
@Column 注释用于指定某一列与某一个字段或是属性映射的细节信息。
7.标准查询
--
HQL语句中的like模糊查询，用于大小写不分的情况，而ilike用于区分大小写的情况。
```java
// To get records having fistName starting with zara
cr.add(Restrictions.like("firstName", "zara%"));

// Case sensitive form of the above restriction.
cr.add(Restrictions.ilike("firstName", "zara%"));
```
Hibernate Session 接口提供了 createCriteria() 方法，可用于创建一个 Criteria 对象，使当您的应用程序执行一个标准查询时返回一个持久化对象的类的实例。比如<br>
```java
Criteria cr = session.createCriteria(Employee.class);  
List results = cr.list();  
```
8.缓存策略
--
并发策略<br>
一个并发策略是一个中介，它负责保存缓存中的数据项和从缓存中检索它们。如果你将使用一个二级缓存，你必须决定，对于每一个持久类和集合，使用哪一个并发策略。<br>
Transactional:为主读数据使用这个策略，在一次更新的罕见状况下并发事务阻止过期数据是关键的。<br>
Read-write:为主读数据再一次使用这个策略，在一次更新的罕见状况下并发事务阻止过期数据是关键的。<br>
Nonstrict-read-write:这个策略不保证缓存和数据库之间的一致性。如果数据几乎不改变并且过期数据不是很重要，使用这个策略。<br>
Read-only:一个适合永不改变数据的并发策略。只为参考数据使用它。<br>
对数据库操作的日志记录可以用Interceptor实现。

9.集合与关联
--
使用HQL就能够以面向对象的方法来检索映射到数据库表的内容，可以是以前会话中保存的持久化对象，也可以是完全来自于应用程序代码以外的数据。<br>
例如要将多个艺人与多个歌曲的表建立关联，就需要用到`多对多关联`，要在两个类的映射文件中添加对方的set标签，表明要和谁关联，以及要映射的表的column名字叫什么，并且要在其中一端写上 inverse="true"

马士兵Spring课程
==
Hibernate的目的就是原来要把一个类存到数据库里面，需要SQL语句去写很多，而SQL语句不是面向对象的，因此HIbernate就在这里发挥了作用，不用去写SQL语句，直接交给HIbernate里面的SessionFactory方法去处理。
02
--
可以在eclipse中window-preferences-java-build path-user library新建自己的library把自己需要的jar包加进去。映射文件要和实体类放在一个包里面。假如在映射文件里面只是写了java的类名而没有写表名，默认是和类名一样的名字，在数据库里面表名不区分大小写。当遇到工程未知错误时，对工程名重新命名可能是一种解决方案。
05
--
如果不给提示，可以alt + / 就会给出提示，并且在windows，preference，content中有content assitance就会有关于提示的设置。
06
--
马士兵在这里自己模拟了一下hibernate的原理，就是自己用java语言拼了一个sql语句，然后得到通过反射的方法得到java中的方法名以及参数，然后将其持久化到数据库当中。这个视频感觉挺不错的，主要用到了反射机制。具体步骤就是首先根据配置内容自己拼出SQL语句，然后找到方法名，然后用反射调用方法拿到返回值，根据返回值的类型，从而决定PreparedStatement究竟是setString还是setInt等等其他的。得到方法名,比如methodname之后，用类的实例比如Method m=s.getClass().getMethod(methodname)就可以得到这个方法。
11
--
在cfg.xml中有一个hbm2ddls标签设置，可以设置为create，表示如果数据库中没有这个表就创建一个。
12
--
hibernate可以通过数据库的表生成实体类以及xml配置文件，此外也可以通过实体类生成数据库的表。<br>
先建表还是先建类呢？理论上先类后表，因为面向对象的编程，有了类可以生成各种各样的表（SQL，Oracle等等），但是实际当中是先表后类，如果让类自动生成表就难以对数据库进行优化，而先表后类可以人为优化数据库。用powerdesigner先建表。
13
--
做日志的框架有很多，有slf，log4j，apache commons log等等。在这里有slf的API，以及log4j的jar包，需要把这两个连起来，于是就需要导入slf4j-log4j的jar包把这两个连起来，这是一种适配器模式。用log4j需要写一个log4j.properties的配置文件，或者直接导入也可以，这个文件是说什么log输出，什么log不输出。
14
--
自己写一个Junit test，马士兵模仿Maven，新建了一个source folder，z里面放的的是测试代码，然后和源代码结构一样，也就是说src文件夹下有什么类对应的test就有一个测试类。然后在测试文件下中写入@Test注解。一般而言sessionfactory只用实例化一次，所以可以用单例以及static语句块只实例化一次。但是在5.2中好像不用static语句块了，而是用metadatasource来创建。
16
--
如果表名和类名不一样，需要用@Table进行显式声明，@Table(name="XXX"),当然也可以在xml文件里面用table=“XXX”进行配置。如果字段名和属性名不一样，用@Column进行显式指定。Annotation会默认把所有的属性都会持久化，如果不想让其中某个属性 持久化就在对应的get方法上加上@Transient，本意是透明的。而xml中只有显式指明才会持久化，所以不想持久化只要不写就可以。要想映射枚举类型，在注解中只需要添加@Enumerated,在xml会比较麻烦。
17
--
@Id可以放在成员变量上面，也可以放在get方法上。如果放在属性上，这样会破坏java面向对象的封装性，Java可以用映射的方法获取成员变量。所以建议放在get方法上。当然有可能是没有field，但是有get方法，比如只有单价和数目两个field，但是有一个getTotalPrice方法，所以放在get方法上依然可以在数据库中持久化。
如果Juint不报异常，但是有错误，可以用try catch,如果还是不报异常，可以写上public static void main(String[] args)方法，把Junit 程序当做Application来运行。
20
--
主要说的ID生成策略，在xml标签里面加入\<generator class="XXX"\>就可以,class常用有 uuid，sequence，identity。如果使用注解，可以加@GeneratedValue，要用javax.persistence下的generator。
@SequenceGenerator是根据sequence生成id. TableGenerators是自动产生table值来生成id号

24
--
如果要使用联合主键，需要用到\<composite-id name="XXX" class="XXX"\>,其中class为多个主键组成的联合主键类， name为该类实例化的名字，该类要实现java.io.serializable接口，而且还要重写equals方法以及hashcode方法，这是马士兵当时讲的，不知道现在的新版本还需要不需要。另外也可以用注解实现，第一种方法 ：@Embededable 放在 组件类上，也就是联合主键类，然后把主键set方法加上@Id。第二种方法 ：@EmbededId放在联合主键的get方法上。第三种方法：用@IdClass，表示联合主键类是谁，此时需要在 两个联合 主键上写上@Id 。常用二三方法
26
--
hibernate.cfg.xml不必一定叫这个名， 如果改叫其他的名字，只要在sessionFactory = new AnnotationConfiguration().configure("XXX").buildSessionFactory()即可.openSession()永远是打开新的session，需要close().getCurrentSession() 如果有就会拿到当前存在的，所以拿到的session与刚才的session是相同的，但是如果session commit之后，这个session就关闭了，不用手动close().此时如果再getCurrentSession()就会得到 一个新的session。getCurrentSession一般用于一个事务中（transaction）多个事件，比如添加一个用户，添加后写入log，此时就应该是一个session。current_session_context_class表示目前session的上下文，jta, thread, managed, or a custom class四种取值。主要有两种上下文：JTA以及thread，jta运行时需要application server的支持，分布式界定事务，thread主要从数据库界定事务

28
--
openSession与getCurrentSession不能混用。对象的三种状态：transient、persistent、detached。刚开始是transient状态，save之后是persistent状态，session close之后，变成detached状态。特征，transient：无ID。save之后，缓存中和数据库都有id。但是commit后之后数据库有id，缓存没有id。
