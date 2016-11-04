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

30
--
session.load()方法可以从数据库拿到session，其中第一个参数表示类型，第二个参数表示序列化的元素，马老师此时在这里用的是id号，例子中直接写为1.session.get()方法也可以。但是两者有着很大的区别，如果用load的时候，返回的是代理对象，什么时候用到对象属性才会在什么时候发出sql语句。所以如果session在commit之后，再想拿到该session就会报错，而get方法不会这样，get之后立刻发出sql语句，不会延迟。验证是否是代理对象，可以用t.getClass()

32
--
update方法可以用来更新一个detached对象，也可以更新persistent对象，但是更新transient对象会报错，如果自己给transient对象设定一个id，此时更新该transient对象就不会报错（要求数据库中有这个对象）。一个persistent对象如果修改了与原来的值不同，就会发update语句，如果相同就不会发update语句，发的时候所有字段都会更新。所有字段都会更新因此就会使效率变低，所以如果想让某些字段不更新的话，就可以用注解@Column(updatable=false),当然在xml中也可以，用
update=true|false,但是这种方式不灵活，很少用。第二种方法是在cfg.xml配置文件里面的的class后写上属性dynamic-update=true,但是在hibernate 5.0没找到，可能已经进行了优化。这种方法就是比较和缓存中是否有改动，如果改了就更新该字段，因此该方法对于更新detached对象的时候就没用，因此还是更新所有对象。因此如果想跨session进行update，也就是commit一个session，然后再重启一个session，再修改，此时可以用session.merge()方法。第三种方法建议用HQL（EJBQL）语句,Query q = session.createQuery("XXX").executeUpdate();

33
--
Clear the Session so the person entity becomes detached，这是session.clear()方法的作用，就是强制清除session的缓存。flush()方法是强制缓存中的额内容和数据库中的内容做同步，commit()方法会默认包含flush(),flush同步时机是由flushmode决定的，可以手动设置。

35
--
主要讲了对象之间一对一的关系映射，举例husband和wife，husband中有一个Wife引用，生成setWife，getWife方法，然后在getWifes方法上写上@OneToOne，这样就会自动建立两个表之间的关联。如果要想设置映射的外键的属性，可以用@JoinColumn(),里面可以设置name=“XXX”,表示生成列的名称。当然也可以在xml文件中设置\<many-to-one  unique="true"/> 标签,虽然是多对一，但是加上unique之后就变成一对一的了。不过貌似现在hibernate都提倡用annotation，xml配置的在user guide都没有了。以上是一对一单向外键关联。<br>

37
--
讲了对象之间OneToOne的双向外键关联，继续上面的，在husband和wife中都添加对方的引用，然后分别在相应的get方法上添加@OneToOne注解，这时两个表都会分别生成对方的关联，用powerdesigner就会看到两个表有各自指向对方的箭头。因此此时在Wife类的getHusband方法@OneToOne后添加(mappedBy="wife"),表示是由对方husband中的属性wife决定映射.只要有双向关联，mappedby一定要设，只要定义一边就可以。
38
--
@PrimaryKeyJoinColumn用于主键关联,马士兵说主键关联不重要。39讲了联合主键，<br>
```java
 @JoinColumns({
        @JoinColumn(name = "fname", referencedColumnName = "firstname"),
        @JoinColumn(name = "lname", referencedColumnName = "lastname")
    })
```
这是语法，表示联合的主键参考了谁，生成的列名称叫什么。并且联合主键类要实现Serializable接口，并且要重写equals方法以及hashcode()方法。

41.
--
讲的是组件映射，就是两个类，其中一个类B是另外一个类A的一部分，最后映射到数据库中是一张表，B称为A的组件，因此类B不用写@Entity，同时也不用写自己的主键，不用@Id.类A中要有B的引用，然后在getB的方法上添加上@Embedded，此时cfg.xml的\<mapping \>标签不用y映射B类了。在xml文件中也非常简单，只需要用\<component即可，同样，官方已经没有了。

42
--
讲的是多对一，方法是在多的这方加外键。比如一个类叫group，一个类叫user，多个user属于同一个group，在user类中引入Group的引用，然后在getGroup方法上添加@ManyToOne即可。<br>
43讲的是一对多的单向关联，在一的一方加入多的集合（set，map，数组都可以），比如private Set\<User\> users;然后在对应的get方法上添加@OneToMany 并且加上@JoinColumn，否则会生成一个group和user的中间表。<br>
44讲的是一对多，多对一的双向关联，此时需要在两个类中都加上注解标签，然后在少的一方，@OneToMany后加上（mappedBy="XXX"）xxx为对方类中的属性字段。

45
--
讲的是manytomany，多对多单向关联， 如果要自定义生成的表名和列名，要用jointable。 
```java
@JoinTable(name = "repository_properties",
        joinColumns = @JoinColumn(name = "repository_id"),
        inverseJoinColumns = @JoinColumn(name = "property_id")
    )
```
46讲的是manytomany的双向关联，要在两个类中都要写上@ManyToMany的注解，并且要写上 mappedBy,注意在 cfg.xml配置文件当中，如果是用注解配置的话要写上\<mapping class=" xxx.xxx.xxx/>  如果是xml配置的话，要写上\<mapping resource="xxx/xxx/xxx/xxx.hbm.xml/>

47.
--
讲的是增删改查，如果要插入一个数据
```java
public void testSaveUser{
 User u = new User();
 Group g= new Group();
 u.setGroup(g);
 SessionFactory s = sessionFactory.getCurrentSession();
 s.beginTransaction();
 s.save(g);//1
 s.save(u);//2
 s.getTansaction().commit();

}
```
注意1处和2处的程序，现在user和group有关联，所以如果只写s.save(u),不写s.save(g)的话，需要在user类的@ManyToOne注解后添加cascade属性，指明为ALL，表示增删改查的所有时候都会将group和user级联在一起。双向关系的时候要设置好双向关联，否则可能会出现有关联的两个表没有存进去。

48
--
讲的是查（read），User u=(User)s.get(User.class,1);此时在manytoone的模式中，即使没有指定cascade，也会把one取出来。但是如果是Group g=(group)s.get(Group.class,1);就是要取一的一方，不会吧把多的一方也取出来，即使cascade设置为all也不会取出来，此时的fetch默认为lazy。所以cascade管理的是CUD，没有R。fetch管理load，get等读的操作。fetch的选择有LAZY和EAGER两种。<br>
```java
---@JoinTable
@JoinTable(name = "t_mtm2_teacher_course", //@JoinTable指定中间表表名
joinColumns = { @JoinColumn(name = "cid") }, //joinColumns指定该表在中间表中的外键名称，没有指定则用默认值
inverseJoinColumns = { @JoinColumn(name = "tid") })//inverseJoinColumns指定另一方在中间表中的外键名称
```
49 如果设置为lazy，那么直到get的时候才会从内存去拿， 如果此时session已经commit的话，那么就会报错。

50
--
cascade可以选择的类型persist，merge表示调用这些方法的时候会cascade。如果想要删除某条user，可以首先打破user与group的关联关系，比如user.setGroup(null);或者也可以使用HQL或者EJBQL语句，比如ssession.createQuery("delete from User u where u.id=1")

53.
--
集合映射。可以在list添加@OrderBy（）进行排序。当然k也可以在HQL语言中用语句排序。如果要用map的话，有一个@MapKey注解m，表示用哪个作为key，比如@MapKey（name=“id”）
55
--
讲了继承映射，第一种方法：就父类一张表，继承类型叫SINGLE_TABLE。在父类中用@Inheritance注解类，@DiscriminatorColumn表示区分器类型，名字等，@DiscriminatorValue表示父类如果存到表中用什么 样的字符串表示父类类型。子类中不要写id，因为父类已经有了， 子类中写上@DiscriminatorValue（“student”），表示插入表中的字段名叫什么。<br>。
第二种方法:子类也有表，父表中拥有子表的所有属性，继承类型为TABLE_PER_CLASS。不过这种方法比较麻烦，因为学生和老师的id不能一样，因此id的生成策略不能自动，所以要用bytable<br>
第三种方法： 父表是共有字段，子表是独有字段，不过通过主键与父表连接。此时父类中@Inheritance（stragy=InterianceType.JOINED）.<br>
第一和第三种用的比较多。

60
--
讲的是QL查询语言，Query q=session.createQuery("from Category"); 这是面向对象的查询语言，所以这里的Category是类名而不是表名。<br>
"from Category c where c.id >:min )"此处冒号为占位符，q.setParameter("min",2);链式编程思想是将多个操作（多行代码）通过点号(.)链接在一起成为一句代码,使代码可读性好。如a(1).b(2).c(3)。q.setMaxResults(4)这个语句用于分页，表示每页最多多少条。<br>
```java
Query q = session.createQuery("select c.id,  c.name from Category c order by c.name desc");
		List<Object[]> categories = (List<Object[]>)q.list();
		for(Object[] o : categories) {
			System.out.println(o[0] + "-" + o[1]);
		}
```
此时是取出的两个字段，输出的q是一个object的数组。<br>
Hibernate 1+N问题<br>。
Query#list - executes the select query and returns back the list of results.<br>
Query#uniqueResult - executes the select query and returns the single result. If there were more than one result an exception is thrown.
```java
Query q = session.createQuery("select count(*) from Msg m");
long count = (Long)q.uniqueResult();
```
返回的是一个long类型

62
--
is empty是测试集合的属性是否为空。is null是测试某个值是否为空。"where p.name like 'Jo%'", Person.class "其中%表示0个或者多个,\_下划线表示一个。<br>
QL语句中用in还是exists，马士兵说用exists，因为相对于in，exists执行效率高。
```java
@NamedQueries(
    @NamedQuery(
        name = "get_person_by_name",
        query = "select p from Person p where name = :name"
    )
)
```
表示命名的query，对一个query语句进行命名。<br>
```java
List<Object[]> persons = session.createSQLQuery(
    "SELECT * FROM person" )
.list();
```
表示使用数据库本身的语言，现在person就不是一个类名了，而是表名

63.
--
讲了QBC（query by criteria）,通过criteria增加限制
```java
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like("name", "Fritz%") )
    .add( Restrictions.or(
        Restrictions.eq( "age", new Integer(0) ),
        Restrictions.isNull("age")
    ) )
    .list();
```
讲了QBE（query by example），设计思想就查找出最像example的那个
```java
Session session = sf.openSession();
		session.beginTransaction();
		Topic tExample = new Topic();
		tExample.setTitle("T_");
		
		Example e = Example.create(tExample)
					.ignoreCase().enableLike();
		Criteria c = session.createCriteria(Topic.class)
					 .add(Restrictions.gt("id", 2))
					 .add(Restrictions.lt("id", 8))
					 .add(e)
					 ;
					 
		
		for(Object o : c.list()) {
			Topic t = (Topic)o;
			System.out.println(t.getId() + "-" + t.getTitle());
		}
		session.getTransaction().commit();
		session.close();
```
64
--
讲了性能优化，session.clear()方法用于清除session，防止对内存的过多占用。<br>
Java有内存泄露吗？<br>
语法级别没有，但是在实际运用中可能会出现，比如打开一个连接池，没有f关闭。什么时候会遇到1+N的问题？
前提：Hibernate默认表与表的关联方法是fetch="select"，不是fetch="join",这都是为了懒加载而准备的。<br>
1）一对多(<set><list>) ，在1的这方，通过1条sql查找得到了1个对象，由于关联的存在 ，那么又需要将这个对象关联的集合取出，所以合集数量是n还要发出n条sql，于是本来的1条sql查询变成了1 +n条 。<br> 
2）多对一<many-to-one>  ，在多的这方，通过1条sql查询得到了n个对象，由于关联的存在,也会将这n个对象对应的1 方的对象取出， 于是本来的1条sql查询变成了1 +n条 。<br>
3）iterator 查询时,一定先去缓存中找（1条sql查集合,只查出ID），在没命中时，会再按ID到库中逐一查找， 产生1+n条SQL<br>
怎么解决1+N 问题？<br>
1 ）lazy=true， hibernate3开始已经默认是lazy=true了；lazy=true时不会立刻查询关联对象，只有当需要关联对象（访问其属性，非id字段）时才会发生查询动作。<br>
2) 当然你也可以设定fetch="join"，一次关联表全查出来，但失去了懒加载的特性。

65
--
讲的是list()方法和iterator方法的区别，首先iterator取出的是id，只有当用到对象的内容的时候才会取其中的内容。而list()会把所有内容立刻取出来。session中list第二次发出，仍然会到数据库中查询。iterator第二次会首先找session级的缓存。
下面是两个方法的代码
```java
//join fetch
	@Test
	public void testQueryList() {
		Session session = sf.openSession();
		session.beginTransaction();
		//List<Topic> topics = (List<Topic>)session.createCriteria(Topic.class).list();
		List<Category> categories = (List<Category>)session.createQuery("from Category").list();
		
		for(Category c : categories) {
			System.out.println(c.getName());
		}
		
		List<Category> categories2 = (List<Category>)session.createQuery("from Category").list();
		for(Category c : categories2) {
			System.out.println(c.getName());
		}
		session.getTransaction().commit();
		session.close();
		
	}
	
	//join fetch
	@Test
	public void testQueryIterate() {
		Session session = sf.openSession();
		session.beginTransaction();
		//List<Topic> topics = (List<Topic>)session.createCriteria(Topic.class).list();
		Iterator<Category> categories = (Iterator<Category>)session.createQuery("from Category").iterate();
		
		
		while(categories.hasNext()) {
			Category c = categories.next();
			System.out.println(c.getName());
		}
		
		Iterator<Category> categories2 = (Iterator<Category>)session.createQuery("from Category").iterate();
		
		while(categories2.hasNext()) {
			Category c = categories2.next();
			System.out.println(c.getName());
		}
		session.getTransaction().commit();
		session.close();
		
	}
```
66
--
主要讲的是缓存和隔离，Hibernate有 两级缓存，也有说是三级缓存的，一级，二级以及查询缓存。一级缓存是session级别的缓存，也就是不同的session不能公用该缓存，而二级缓存不同的session可以公用，也就是说二级缓存可以跨session，是sessionfactory级别的缓存。要想用二级缓存，首先要在cfg.xml配置文件里面打开二级缓存.马士兵在此用的是ehcache，然后导入了一个ehcache.xml，也就是ehcache的配置文件，里面详细说明了ehcache的配置.二级缓存适合放经常被访问，数据量不大，改动不大不会经常被访问的对象。要用的话要在类上写上注解@Cache，其中要写上@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.XXX)这个属性，XXX可以为READ_WRITE，READ_ONLY等等。 默认 情况下，load以及iterator会使用二级缓存。list会向二级缓存加数据，但是查询的时候不会首先去二级缓存找。查询缓存就是重复查询会使用的缓存。<br>
要想使用二级缓存，要在cfg.xml文件中设置cache.use_query_cache为true，然后再调用Cache.setCachable(true) 来调用二级缓存。打开二级缓存后不仅在同一个session里面两次查询只会发送一条SQL语句，跨session的 两次相同查询也会只发一条SQl语句。缓存算法有LRU，LFU，FIFO等<br>

68
--
事务的四个特性：A（Atomic）C（consistency）I(Isolation)D(durability).<br>
并发事务可能出现的问题：<br>
1:第一类丢失更新 2.脏读(读到了第二个事务还没有提交的数据）3，不可重复读（就是读取事务 读取了两遍，结果不一样）4第二类丢失更新（不可重复读的特殊 情况）5，幻读（讨论的是插入和删除，与不可重复读类似）

