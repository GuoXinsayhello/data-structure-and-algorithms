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
可以在eclipse中window-preferences-java-build path-user library新建自己的library把自己需要的jar包加进去。映射文件要和实体类放在一个包里面。假如在映射文件里面只是写了java的类名而没有写表名，默认是和类名一样的名字，在数据库里面表名不区分大小写。
