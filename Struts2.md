1
--
JRE:Java  Runtime  Enviromental(java运行时环境)。也就是我们说的JAVA平台，所有的Java程序都要在JRE下才能运行。包括JVM和JAVA核心类库和支持文件。
与JDK相比，它不包含开发工具——编译器、调试器和其它工具。<br>
遇到404错误有可能是jsp文件的位置不对，应该在 WebContent文件夹下面创建jsp文件。点击指定的类，按下F1可以获得该类的帮助信息。要在struts.xml中写上\<constant name="struts.devMode" value="true" /> 这样调试的时候修改就可以立即出现反馈，不用再把服务器关闭再打开。

10
--
在struts.xml里面，package标签用于区分action可能会出现重名的情况，表明action来自哪一个包，namespace可以不写，表示接收一切action，囊括了其他namespace处理不了的action，如果要写要以/开头，可以是/XXX,当然在浏览器访问的时候就要添加上/XXX。对于action标签，如果没有写name，那么name默认值是"success"。遇到一个问题，如果从前一个项目拷贝一个新项目的时候，马士兵说要修改web project settings的web context root改为和新的工程名一样的名字，但是我改之后还是没有用。weird。之前出现的解决方法是把copy的新的工程从server中remove，然后再debug就可以了。

11
--
可以修改jsp源码的编码方式，输入中文，在window-preference-jsp。在action标签里面，可以写class=”xxx“，定义自己的action，这个xxx是一个普通的java类，
```java
public class IndexAction3 extends ActionSupport {
	@Override
	public String execute() {
		return "success";
	}
}
```
具体的struts.xml如下
```xml
  <package name="front" extends="struts-default" namespace="/">
        <action name="index" class="com.bjsxt.struts2.front.action.IndexAction1">
            <result name="success">/ActionIntroduction.jsp</result>
        </action>
    </package>
```
即可，返回类型为String类型，有一个execute()方法。找到与返回值对应的jsp。如果不配的话，会使用默认的ActionSupport 这个class，这个class里面有一个execute()方法，默认返回一个字符串"success".自己定义action，一般采用如上的从ActionSupport继承的方式。<br>
Struts中的路径是按照action的路径而不是jsp的路径来确定的，所以尽量用绝对路径，不要用相对路径，或者是指定basepath，然后使用相对路径，比如这样：
```xml
<base href="<%=basePath%>">
```
13
--
可以在配置文件中设定method=XXX来指定执行哪个方法，也可以在url地址中动态指定，动态方法调用DMI，后者比较推荐，前者会产生太多action。<br>
用action实现跳转：<br>
首先在总的jsp里面写上
```javascript
<a href="<%=basePath %>actions/jumpadd">添加</a>
    <a href="<%=basePath %>actions/jumpdelete">删除</a>
```
然后在strust.xml里面写上
```xml
<action name="jumpadd" class="com.hellostruts.IndexAction" >
<result>/studentadd.jsp</result>
</action>
```
也可以通过通配符配置，但是自己配置的一直没有成功，不知道为什么。遇到了一个非常奇怪的现象，jumpadd的action不可以，而jumpdelete的方法就可以，很奇怪。
找到原因了，因为自己写的class是继承于ActionSupport的，如果自己随便写一个方法，比如
```java
public String aaa{
return SUCCESS;
}
```
这个aaa方法在actionsupport中没有，所以就找不到，如果换成execute的话就可以了。
```xml
<action name="Student*" class="com.bjsxt.struts2.action.StudentAction" method="{1}">
<result>/Student{1}_success.jsp</result>
</action>
```
使用通配符可以大大简化配置量。
通过action+感叹号+方法名的跳转方法需要在struts.xml中添加\<constant name="struts.enable.DynamicMethodInvocation" value="true" />这样的一句话。

16
--
讲的是域模型（DomainModel），就是在一个类A中包含一个类B，A中有setB和getB，B中包括一系列属性，包括name，age等。这种方式用的比较多，如果用modeldriven的方式，类A要实现ModelDriven接口，user需要自己new出来。
```java
public class TestPara extends ActionSupport implements ModelDriven<User>{
	private User user =new User();
	@Override
	public User getModel() {
		// TODO Auto-generated method stub
		return user;
	}
}
```
19
--
讲了如果输入错误，如何给出提示的跳转。
首先在action用到的类中写上：
```java
if(!name.equals("admin")){
  this.addFieldError("name1", "fuck name error");
  System.out.println("namefukc=" + name);
  return ERROR;
		}
```
然后在错误跳转的jsp上写上
```javascript
<%@taglib uri="/struts-tags" prefix="s" %>
```
这里用到了标签，
然后在该jsp上写上即可
```
<s:fielderror fieldName="name1" theme="simple"/>
<s:property value="errors.name[0]"/>//取出value stack中的值
```
20
--
这个视频讲的是在后台写入一个数据，能够显示到网页当中，第一种方式具体做法如下，在action的类里面，
```java
public class LoginAction1 extends ActionSupport {
	private Map request;
	private Map session;
	private Map application;
	public LoginAction1() {
		request = (Map)ActionContext.getContext().get("request");//ActionContext是threadlocal的一个对象，这种方式在构造方法中得到的
		session = ActionContext.getContext().getSession();
		application = ActionContext.getContext().getApplication();
	}
	public String execute() {
		request.put("r1", "r1");  //下面这几个就是在后台放入数据，用户名和密码分别为r1，r1
		session.put("s1", "s1");
		application.put("a1", "a1");
		return SUCCESS; 
	}
}
```
然后在index.jsp里面写入
```javascript
<input type="button" value="submit1" onclick="javascript:document.f.action='login/login1';document.f.submit();" />
```
然后在登陆成功的jsp中写入
```javascript
<s:property value="#request.r1"/> | <%=request.getAttribute("r1") %> <br />//这句话中|是或者的关系，表示用#key还是用getAttribute都可以
```
上面的是第一种方式<br>
第二种方式是这样的:<br>
Action实现了SessionAware，ApplicationAware，RequestAware接口，用的是依赖注入的思想，不用在构造方法中自己去get，而是直接用接口的抽象方法去set，从struts2容器中去取，如下：
```java
@Override
	public void setRequest(Map<String, Object> request) {
		this.request = request;
	}
```
推荐使用这种方法。<br>
其中request，session，application可以是
```java
	private Map<String, Object> request;
	private Map<String, Object> session;
	private Map<String, Object> application;
```
这些一般e类型
也可以是
```java
private HttpServletRequest request;
	private HttpSession session;
	private ServletContext application;
```
这种特殊类型。用于这种特殊类型的时候要实现ServletRequestAware这种接口。<br>
可以在struts.xml文件中用\<include file="xxx.xml">把另外一个xml包含到struts.xml里面<br>
可以为struts定义一个默认的action，当找不到action的时候就会调用这个action，如下：
```xml
<default-action-ref name="XXX"></default-action-ref>
```

27
--
开始讲result，首先说result的类型，如果不配置，默认类型是dispatcher，dispatcher可以跳转到jsp，html等等，但是不能是action，但是如果类型是chain可以跳转到action，通过chain也可以跳转到其他package的action，相当于服务器端的跳转action，redirectaction相当于客户端的跳转action。e可以使用param标签。下面是服务器端跳转与客户端跳转的区别：<br>
1.使用服务器端跳转时，客户浏览器的地址栏并不会显示目标地址的URL，而是用客户端跳转时，地址栏当中会显示目标资源的URL；<br>
2. 服务器端跳转是由客户端发送一个请求，请求一个服务器资源——如JSP和Servlet——，这个资源又将请求转到另一个服务器资源，然后再给客户端发送一个响应，也就是说服务器端跳转是客户端发送一次请求，服务器端给出一次响应；而客户端跳转的流程则不同。客户端同样是发送一个请求给服务器端资源，这个服务器资源会首先给客户端一个响应，客户端再根据这个响应当中所包含的地址，再次向服务器端发送一个请求，也就是说客户端跳转是两次请求，两次响应；一次reques只对应一个值栈<br>
而redirect方式是客户端跳转。

30
--
讲的是global results，在struts.xml里面可以公用的result
```xml
<global-results>
    		<result name="mainpage">/main.jsp</result>
    	</global-results>
```
如果想要跨包的话，需要在package写上标签extends="xxx".
可以在配置文件里面动态跳转，
```
<action name="user" class="com.bjsxt.struts2.user.action.UserAction">
<result>${r}</result>
 </action> 
```
这里面r是一个变量，可以在UserAction中定义当条件不同时，r的值也不同，从而就实现了动态跳转，这是不带参数的结果集。
```
 <action name="user" class="com.bjsxt.struts2.user.action.UserAction">
<result type="redirect">/user_success.jsp?t=${type}</result>
 </action>  
```
由于服务器的跳转模式是只有一个request，发出的是action，所以会对应有一个值栈。 要想在客户端跳转的页面中取出参数，第一种方式不行，第二种方式可以。因为客户端跳转是两次request（redirect方式），第二次的request指向一个jsp，而jsp没有对应的值栈，只有action有，所以第一种从值栈中取的方式不行。
```
<body>
    User Success!
    from valuestack: <s:property value="t"/><br/> //不行
    from actioncontext: <s:property value="#parameters.t"/>//可以
    <s:debug></s:debug>
</body>
```
36.
--
这个讲的是OGNL表达式
瀑布式开发是一种老旧的，正在过时的计算机软件开发方法。最开始的软件行业普遍采用这种方法，但是这种方法套用自传统工业生产，不适应计算机软件开发的具体情况。瀑布式的主要的问题是它的严格分级导致的自由度降低，项目早期即作出承诺导致对后期需求的变化难以调整，代价高昂。瀑布式方法在需求不明并且在项目进行过程中可能变化的情况下基本是不可行的。<br>
在迭代式开发方法中，整个开发工作被组织为一系列的短小的、固定长度（如3周）的小项目，被称为一系列的迭代。每一次迭代都包括了定义、需求分析、设计、实现与测试。采用这种方法，开发工作可以在需求被完整地确定之前启动，并在一次迭代中完成系统的一部分功能或业务逻辑的开发工作。再通过客户的反馈来细化需求，并开始新一轮的迭代。<br>
首先讲的是给通过 OGNL给domainmodel传值，user.xxx只有传才会构造，想初始化domainmodel，可以自己new，也可以传参数值， 但是这时需要保持参数为空的构造 方法。<br>
OGNL是Object-Graph Navigation Language的缩写，它是一种功能强大的表达式语言，通过它简单一致的表达式语法，可以存取对象的任意属性，调用对象的方法，遍历整个对象的结构图，实现字段类型转化等功能<br>
如果只是调用普通方法，无论是对象中的普通方法还是action中的普通方法，直接调用就可以，如果要调用静态方法，要用"@类名（完整类名）@方法名"来实现，<br>
如果调用静态方法，马士兵说需要在struts.xml中写入
```xml
<constant name="struts.ognl.allowStaticMethodAccess" value="true"/>
```
这个在官网上 也查到了在后续的版本中需要添加。
可以在官网上查到每个类的具体方法以及constantproperties等东西。
