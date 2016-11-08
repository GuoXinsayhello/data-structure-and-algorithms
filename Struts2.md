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
讲的是域模型（DomainModel），就是在一个类A中包含一个类B，A中有setB和getB，B中包括一系列属性，包括name，age等。如果用modeldriven的方式，类A要实现ModelDriven接口，user需要自己new出来。
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
