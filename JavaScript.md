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
