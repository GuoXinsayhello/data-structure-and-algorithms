第2章
--
##第1条：用静态工厂方法代替构造器
优点：<br>
      1、静态工厂方法不必在每次调用它们的时候都创建一个新对象，确保实例受控类，比如singleton（单例类）<br>
      2、可以返回原返回类型的任何子类型对象<br>
      3、在创建参数化类型实例的时候，它们使代码变得简洁<br>
缺点：<br>
      1、类如果不含公有的或者受保护的构造器，就不能被子类化<br>
      2、它们与其他的静态方法实际上没有任何区别
##第2条：遇到多个构造器参数时要考虑用构造器
`重叠构造器模式`：为了适应不同情况，重复写具有不同个数参数的构造函数<br>
`JavaBeans模式`：调用一个无参构造器来创建对象，然后调用setter方法来设置每个必要的参数<br>
`Builder模式：` 不直接生成想要的对象，而是让客户端利用所有必要的参数调用<br>
`通配符`是一种特殊语句，主要有星号(*)和问号(?)，用来模糊搜索文件。当查找文件夹时，可以使用它来代替一个或多个真正字符；当不知道真正字符
或者懒得输入完整名字时，常常使用通配符代替一个或多个真正的字符。 实际上用“*Not?pad”可以对应Notpad\MyNotpad【*可以代表任何文字】;
Notpad\Notepad【?仅代表单个字】;Notepad\Notepod【ao代表a与o里二选一】，其余以此类推。
##第3条：用私有构造器或者枚举类型强化Singleton属性
JAVA`反射机制`是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。<br>
在外部调用`静态方法`时，可以使用"类名.方法名"的方式，也可以使用"对象名.方法名"的方式。而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象。静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法；实例方法则无此限制。<br>
单元素的枚举类型已经成为实现singleton的最佳方法。如下
```java
public enum Elvis{
INSTANCE;
public void leaveTheBuilding(){...}
}
```
`抽象方法`必须用abstract关键字进行修饰。如果一个类含有抽象方法，则称这个类为`抽象类`，抽象类必须在类前用abstract关键字修饰。因为抽象类中含有无具体实现的方法，所以不能用抽象类创建对象。<br>
####抽象类与接口类的区别
该部分来自http://www.cnblogs.com/dolphin0520/p/3811437.html
#####1.语法层面上的区别

　　1）抽象类可以提供成员方法的实现细节，而接口中只能存在public abstract 方法；<br>

　　2）抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的；<br>

　　3）接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法；<br>

　　4）一个类只能继承一个抽象类，而一个类却可以实现多个接口。<br>

#####2.设计层面上的区别

　　1）抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。举个简单的例子，飞机和鸟是不同类的事物，但是它们都有一个共性，就是都会飞。那么在设计的时候，可以将飞机设计为一个类Airplane，将鸟设计为一个类Bird，但是不能将 飞行 这个特性也设计为类，因此它只是一个行为特性，并不是对一类事物的抽象描述。此时可以将 飞行 设计为一个接口Fly，包含方法fly( )，然后Airplane和Bird分别根据自己的需要实现Fly这个接口。然后至于有不同种类的飞机，比如战斗机、民用飞机等直接继承Airplane即可，对于鸟也是类似的，不同种类的鸟直接继承Bird类即可。从这里可以看出，继承是一个 "是不是"的关系，而 接口 实现则是 "有没有"的关系。如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是有没有、具备不具备的关系，比如鸟是否能飞（或者是否具备飞行这个特点），能飞行则可以实现这个接口，不能飞行就不实现这个接口。<br>

　　2）设计层面不同，抽象类作为很多子类的父类，它是一种模板式设计。而接口是一种行为规范，它是一种辐射式设计。什么是模板式设计？最简单例子，大家都用过ppt里面的模板，如果用模板A设计了ppt B和ppt C，ppt B和ppt C公共的部分就是模板A了，如果它们的公共部分需要改动，则只需要改动模板A就可以了，不需要重新对ppt B和ppt C进行改动。而辐射式设计，比如某个电梯都装了某种报警器，一旦要更新报警器，就必须全部更新。也就是说对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动。<br>
##第4条：通过私有构造器强化不可实例化的能力
让类包含私有构造器，它就不能被实例化了
```java
public class UtClass
{
 private UtClass(){
   throw new AssertionError();
}
}
```
||类内部|本包|子类|外部包|
|---------|-----|-----|----|------|
|public| 是|是|是|是|
|procted|是|是|是|否|
|default|是|是|否|否|
|private|是|否|否|否|
java几种关键字的作用范围如上所示
##第5条：避免创建不必要的对象
如果某些变量只用一次而不改变它的值就不要每次都创建，用static final修饰即可<br>
优先使用基本类型（比如long）而不是装箱基本类型（比如Long），注意无意识的自动装箱
##第6条：消除过期的对象引用
如果一个栈先增长，后收缩，从栈中弹出的对象不会被当做垃圾回收，因为栈维护着对这些对象的过期引用<br>
只要类是自己管理内存，应该警惕内存泄漏。另外内存泄漏的另外一个来源是缓存。第三个来源是监听器和其他回调<br>
###对象的引用分类
来自http://zhangjunhd.blog.51cto.com/113473/53092/
####⑴强引用（StrongReference）
强引用是使用最普遍的引用。如果一个对象具有强引用，那垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。
 
####⑵软引用（SoftReference）
如果一个对象只具有软引用，则内存空间足够，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存（下文给出示例）。
软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收器回收，Java虚拟机就会把这个软引用加入到与之关联的引用队列中。
 
####⑶弱引用（WeakReference）
弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。
弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。
 
####⑷虚引用（PhantomReference）
“虚引用”顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。
虚引用主要用来跟踪对象被垃圾回收器回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。

##第7条：避免使用终结方法
如果有资源确实需要终止，只需要提供一个显式的终止方法。终结方法链不会自动执行，如果类有终结方法，并且子类覆盖了终结方法，则子类的终结方法必须手工调用超类的终结方法。你应该在try块中终结子类，并在相应的finally块中调用超类的终结方法。<br>
###匿名内部类的实现方法
来自http://blog.csdn.net/cntanghai/article/details/6094481
匿名内部类的两种实现方式：第一种，继承一个类，重写其方法；第二种，实现一个接口（可以是多个），实现其方法。
```java
public class TestAnonymousInterClass{   
    public static void main(String args[]){   
        TestAnonymousInterClass test=new TestAnonymousInterClass();   
        test.show();   
    }   
    //在这个方法中构造了一个匿名内部类   
    private void show(){   
        Out anonyInter=new Out(){// 获取匿名内部类实例   
               
            void show(){//重写父类的方法   
                System.out.println("this is Anonymous InterClass showing.");   
            }   
        };   
        anonyInter.show();// 调用其方法   
    }   
}    
  
// 这是一个已经存在的类，匿名内部类通过重写其方法，将会获得另外的实现   
class Out{   
    void show(){   
        System.out.println("this is Out showing.");   
    }   
}  
```
`instaceof`关键字它的作用是测试它左边的对象是否是它右边的类的实例，返回boolean类型的数据。举个例子：
```java
　　String s = "I AM an Object!";
　　boolean isObject = s instanceof Object;
```
Object类中包含一个方法名叫getClass，利用这个方法就可以获得一个实例的类型类。类型类指的是代表一个类型的类，因为一切皆是对象，类型也不例外，在Java使用类型类来表示一个类型。所有的类型类都是Class类的实例。例如，有如下一段代码：
```java
A a = new A();
if(a.getClass()==A.class)
System.out.println("equal");
else System.out.println("unequal");
```
结果就是打印出 “equal”。
第3章 对于所有对象都通用的方法
--
##第8条：覆盖equals遵守通用约定
equals方法实现了等价关系：自反性，对称性，传递性，一致性
##第9条：覆盖equals时总要覆盖hashCode
java中有三种移位运算符<br>
`<<`:左移运算符，num << 1,相当于num乘以2<br>
`>>`:右移运算符，num >> 1,相当于num除以2<br>
`>>>`:无符号右移，忽略符号位，空位都以0补齐<br>
##第11条：谨慎地覆盖clone
Java 5.0添加了对`协变返回类型`的支持，即子类覆盖（即重写）基类方法时，返回的类型可以是基类方法返回类型的子类。协变返回类型允许返回更为具体的类型。
```java
import java.io.ByteArrayInputStream;
import java.io.InputStream;

class Base
{
    //子类Derive将重写此方法，将返回类型设置为InputStream的子类
   public InputStream getInput()
   {
    　　return System.in;
   }
}
public  class Derive extends Base
{
    
    @Override
    public ByteArrayInputStream getInput()
    {
        
        return new ByteArrayInputStream(new byte[1024]);
    }
    public static void main(String[] args)
    {
        Derive d=new Derive();
        System.out.println(d.getInput().getClass());
    }
}
/*程序输出：
class java.io.ByteArrayInputStream
*/
```
###复制对象 or 复制引用
来自http://blog.csdn.net/zhangjg_blog/article/details/18369201
```
		
		Person p = new Person(23, "zhang");
		Person p1 = p;
		
		System.out.println(p);
		System.out.println(p1);
```
当Person p1 = p;执行之后， 是创建了一个新的对象吗？ 首先看打印结果：<br>
com.pansoft.zhangjg.testclone.Person@2f9ee1ac<br>
com.pansoft.zhangjg.testclone.Person@2f9ee1ac<br>
p和p1只是引用而已，他们都指向了一个相同的对象Person(23, "zhang") 。 可以把这种现象叫做引用的复制
```
		Person p = new Person(23, "zhang");
		Person p1 = (Person) p.clone();
		
		System.out.println(p);
		System.out.println(p1);
```
从打印结果可以看出，两个对象的地址是不同的，也就是说创建了新的对象， 而不是把原对象的地址赋给了一个新的引用变量：<br>
com.pansoft.zhangjg.testclone.Person@2f9ee1ac<br>
com.pansoft.zhangjg.testclone.Person@67f1fba0<br>
###深拷贝与浅拷贝
 直接将源对象中的name的引用值拷贝给新对象的name字段，或者是根据原Person对象中的name指向的字符串对象创建一个新的相同的字符串对象，将这个新字符串对象的引用赋给新拷贝的Person对象的name字段。这两种拷贝方式分别叫做浅拷贝和深拷贝。<br>
 clone是浅拷贝的
 第4章 类和接口
 --
##第13条：使类和成员的可访问性最小化
尽可能使每个类或者成员不被外界访问，如果一个类实现了一个接口，那么接口中所有类方法在这个类中也都必须被声明为公有的，因为接口中的所有方法都隐含着公有访问级别。此外，如果方法覆盖了超类中的一个方法，子类中的访问级别就不允许低于超类中的访问级别。<br>
实例域就是指定义类时的最外层的那两个大括号那个范围。 
##第14条：在公有类中使用访问方法而不是公有域，比如：
```java
class Point{
private double x;
private double y;
public Point(double x,double y)
{
this.x=x;
this.y=y;
}
public double getX(){return x;}
public double getY(){return y;}
public void setX(double x){this.x=x;}
public void setY(double y){this.y=y;}
}
```
这是一种比较妥当的方式，但是下面的方式建议不要采用
```java
class Point
{
  public double x;
  public double y;
 }
```
##第15条：使可变性最小
Java包含许多不可变的类，有string，基本类型包装类，BigInteger，BigDecimal
##第16条：复合优先于继承
http://www.cnblogs.com/JohnTsai/p/5304438.html 这个博客说的还行<br>
继承打破了封装性，子类依赖于其超类中特定功能的实现细节<br>
http://www.importnew.com/12907.html 这篇文章对于这一条说的非常详细，非常好<br>
`如果存在一种IS-A的关系（比如Bee“是一个”Insect），并且一个类需要向另一个类暴露所有的方法接口，那么更应该用继承的机制。
`如果存在一种HAS-A的关系（比如Bee“有一个”attack功能），那么更应该运用组合。
##第17条，要么为继承而设计，并提供文档说明，要么就禁止继承
好的API文档应该描述一个给定的方法做了什么工作<br>
以下来自http://blog.sina.com.cn/s/blog_6eef4a8601014jn8.html
```java
class Super {
    public Super() {
        overrideMe();
    }

    public void overrideMe() {
        System.out.println("调用父类方法");
    }
}

public class Sub extends Super {
    private ArrayList list = new ArrayList();

    Sub() {
    }

    @Override
    public void overrideMe() {
        System.out.println("调用子类方法");
    }

    public static void main(String[] args) {
        Super s = new Sub();
        s.overrideMe();
    }
}
```
输出结果是：
调用子类方法
调用子类方法

因为超类中的构造器调用overrideMe方法是会调用子类中的覆盖版本，这是多态的特性<br>
```java
class Super {
    public Super() {
        overrideMe();
    }

    public void overrideMe() {
    }
}

public class Sub extends Super {
    private ArrayList list = new ArrayList();

    Sub() {
    }

    @Override
    public void overrideMe() {
        System.out.println(list.size());
    }

    public static void main(String[] args) {
        Super s = new Sub();
        s.overrideMe();
    }
}
```
这时会抛出NullPointerException，因为子类中的overrideMe覆盖版本将会在子类实例化完成之前就被父类构造器调用，而此时list尚未被实例化，因此抛出异常。
总结：构造器绝不能调用可以被覆盖的方法
有两种方法禁止子类化：<br>
1把类声明为final的 2 把所有的构造器都变成私有的，包括包级私有的，并增加一些公有的静态工厂方法代替构造器
##第18条：接口优于抽象类
通过对导出的每个重要接口提供一个抽象的骨架实现类，把接口和抽象类的优点结合。其实，骨架实现类就是抽象接口，就是一个抽象类实现多个接口，可以新建的类再继承自这个抽象方法，就是抽象类和接口的结合，因为每个类只能继承一个类，而实现接口的话要对接口中的每个方法都要实现一下，如果有多个类，都实现了同一个接口，如果后来要对该接口的方法进行改变，就要对每个类进行修改。
https://10kloc.wordpress.com/2012/12/03/abstract-interfaces-the-mystery-revealed/ 这篇文章说的比较好<br>
`模拟多重继承`是实现了接口的类可以把对于接口方法的调用转发到一个内部私有类的实例上，这个内部私有类扩展了骨架实现类。
##第19条：接口只用于定义类型，不应该被用来导出常量
如果要导出常量可以用枚举类型或者不可实例化（让构造器私有化）的工具类
##第20条：类层次优于标签类
就是不要把不同种类的东西写在一个类中，通过类的继承把类的层次分开
##第22条：优先考虑静态成员类
非静态成员类的实例方法内部可以调用外围实例上的方法，或者利用this获得外围实例的引用。如果声明成员类不要求访问外围实例，就要始终把static修饰符放在声明中，如果是非静态的话会消耗时间和空间。<br>
一共有四种嵌套类：匿名类、静态成员类、非静态类，局部类
第5章 泛型
--
这章没怎么看懂，还要多看几遍
##第23条：请不要在新代码中使用原生态类型
原生态类型List和参数化类型List<Object>之间的区别是前者逃避了泛型检查，后者则明确告诉编译器，它能够有任意类型的对象。<br>
者条意思就是说尽量不要使用List 要使用List<Integer>、List<E>等等
##第24条：消除非受检警告
classcastexception通常是进行强制类型转换时候出的错误<br>
如果无法消除警告，同时可以证明引起警告的代码是类型安全的，只有这时才可以用一个@SupressWarnings注释来禁止警告，应该始终在尽可能小的范围中使用supresswarnings,并且每当使用时都要添加一条注释
##第25条：列表优先于数组
数组是协变的，也就是如果sub是super的子类型，那么数组类型sub[]就是super[]的子类型。所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类Number的子类
##第28条：利用有限制通配符来提升API的灵活性
比如
```java
Stack<Number> numberStack=new Stack<Number>();
Iterable<Interable> integers=...;
numberStack.pushAll(integers);
```
以上代码编译会出错，所以可以用有限制的通配符类型来处理，修改pushAll方法
```java
public void pushAll(Iterable<? extends E> src){
  for(E e:src)
    push(e);
    }
```
Iterable<? extends E>表示E的某个子类型<br>
Collection<? super E>表示E的某个超类型<br>
第5章泛型真的不懂在说些什么
##第29条：优先考虑类型安全的异构容器
ThreadLocal是线程的局部变量，如果一段代码被认为是Atomic，则表示这段代码在执行过程中，是不能被中断的。<br>
String.class属于Class<String>类型，Integer.class属于Class<Integer>类型，当一个类的字面文字被用在方法中来传达编译时和运行时的类型信息时，就被称作type token（类型令牌）
第6章：枚举和注释
--
##第30条：用enum代替int常量
 若Planet是一个enum枚举类型，则Planet.values()函数按照声明顺序返回值数组，将不同的行为与每个枚举常量关联起来：在枚举类型中声明一个抽象的apply方法，并在特定于常量的类主体中用具体的方法覆盖每个常量的抽象apply方法，这种方法被称作特定于常量的方法实现。<br>
当两个case的行为相同的时候，switch语句可以这样用
```java
switch(....)
{
 case A:case B:
    sa=sa+1;
}
```
##第31条：用实例域代替序数
ordinal方法返回每个枚举常量在类型中的数字位置
##第33条：用EnumMap代替序数索引
作者之所以说用ordinal得到序数不合适的原因是如果改变了enum枚举类型中变量的顺序，ordinal的值就会发生改变。所以最好不要用序数来索引数组，而要使用EnumMap，如果是多维的，可以用EnumMap<...,EnumMap<...>>
##第34条：用接口模拟可伸缩的枚举
```java
private static <T extends Enum<T> & Operation> void test(..){...}
```
返回值表示是Enum<T>的子类型同时又是Operation类。<br>
虽然无法编写可扩展的枚举类型，但是可以扩展接口，让枚举类型去实现扩展后的接口。
##第35条：注解优先于命名模式
JUnit 是一个回归测试框架，被开发者用于实施对应用程序的单元测试，加快程序编制速度，同时提高编码的质量。<br>
元注解：元注解的作用就是负责注解其他注解。Java5.0定义了4个标准的meta-annotation类型，它们被用来提供对其它 annotation类型作说明。Java5.0定义的元注解：<br>
　　　　1.@Target,是声明在哪里使用<br>
　　　　2.@Retention,声明什么时候运行。CLASS、RUNTIME和SOURCE这三种，分别表示注解保存在类文件、JVM运行时刻和源代码中。只有当声明为RUNTIME的时候，才能够在运行时刻通过反射API来获取到注解的信息<br>
　　　　3.@Documented,<br>
　　　　4.@Inherited<br>
Java Package.isAnnotationPresent()方法用法实例教程。方法返回true，如果指定类型的注释存在于此元素上,否则返回false。这种方法的设计主要是为了方便访问标记注释
##第36条：坚持使用Override注释
覆盖（override）就是重写，要求与被覆盖的方法名，返回类型，参数列表完全相同。<br>
这一条的意思就是如果在方法声明中使用Override注解来覆盖超类声明，编译器就可以防止大量的错误
第7章：方法
--
`assert`关键字的用法<br>
1、assert condition;<br>
    这里condition是一个必须为真(true)的表达式。如果表达式的结果为true，那么断言为真，并且无任何行动
如果表达式为false，则断言失败，则会抛出一个AssertionError对象。这个AssertionError继承于Error对象，
而Error继承于Throwable，Error是和Exception并列的一个错误对象，通常用于表达系统级运行错误。<br>
2、asser condition:expr;<br>
    这里condition是和上面一样的，这个冒号后跟的是一个表达式，通常用于断言失败后的提示信息，说白了，它是一个传到AssertionError构造函数的值，如果断言失败，该值被转化为它对应的字符串，并显示出来。
2016/7/9看到159页
##第39条：必要时进行保护性拷贝
这一条没看懂。
##第41条：慎用重载
要调用哪个重载方法是在编译时做出决定的，而对于被覆盖的方法是在运行时进行的。不乱用重载机制的策略是不要导出两个具有相同参数数目的重载方法。
```java
public class Test {
	 public static void main(String[] args) { 
		 Set<Integer> set=new TreeSet<Integer>();
		 List<Integer> list=new ArrayList<Integer>();
		 for(int i=-3;i<3;i++)
		 {
			 set.add(i);
			 list.add(i);
		 }
		 for(int i=0;i<3;i++)
		 {
			 set.remove(i);
			 list.remove(i);
		 }
		 System.out.println(set+" "+list);
	 }
}
```
比如上面这段程序，输出的结果是【-3，-2，-1】【-2,0,2】，因为set.remove(i)调用选择去除元素值的重载方法，而list.remove(i)选择调用去除第几个元素的重载方法，所以输出的结果会有不同。
##第42条：慎用可变参数
```java
public class Test {
	 public static void main(String[] args) { 
		int a= sum(1,2,3);
		System.out.println(a);
	
	 }
	 private static int sum(int... args)
	 {
		 int sum=0;
		 for(int arg:args)
			 sum+=arg;
		 return sum;
	 }
}
```
例如上面这段代码用的是可变参数，返回几个整数的和。
```java
int[] a={1,2,3};
System.out.println(Arrays.toString(a));
```
Arrays.toString()方法可以把数组变成字符串。
##第43条：返回零长度的数组或者集合，而不是NULL
第8章：通用程序设计
--
##第45条：将局部变量的作用域最小化
使局部变量的作用域最小化，最有力的方法就是在第一次使用它的地方声明。如果在循环终止之后不再需要循环变量的内容，for循环就优先于while循环，作者的意思是如果有误用变量，在编译的时候就会报错，而不是等到运行的时候才会报错。
##第46条：for-each循环优先于传统的for循环
for-each循环比较简洁，而且能够有效地预防bug，并且没有性能损失
2016/7/10看到187页
