JavaScript是一种脚本语言。<br>
如果重新声明 JavaScript 变量，该变量的值不会丢失：
在以下两条语句执行后，变量 carname 的值依然是 "Volvo"：
```
var carname="Volvo"; 
var carname;
```
这就与Java语言不同了，如果是Java语言，重复声明变量会有编译错误。<br>

而且JavaScript字符串用单引号和双引号都可以，Java单引号表示字符，双引号表示字符串。<br>
(condensed array):在创建数组对象的时候赋值<br>
```
var cars=new Array("Audi","BMW","Volvo");
```
(literal array):不创建变量，直接赋值
```
var cars=["Audi","BMW","Volvo"];
```
而且js两个不同类型的变量之间可以赋值。<br>
js定义函数需要用到function关键字，而java并没有这个关键字。<br>

JavaScript 变量的生存期
--
JavaScript 变量的生命期从它们被声明的时间开始。
局部变量会在函数运行以后被删除。
全局变量会在页面关闭后被删除。<br>
```
var x = "John";              
var y = new String("John");
(x === y) // 结果为 false，因为 x 是字符串，y 是对象
```
js里面有一个===运算符表示绝对等于，也就是值和类型都相等才会输出true。<br>
js里面有`for in`语句，类似于java里面的for each语句。<br>
js里面的typeof操作符类似于java里面的getClass()方法
