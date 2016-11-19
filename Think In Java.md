注意下面这个语句，会输出false和true，因为自己没有实现ListNode的equals方法，equals默认会比较句柄，而Integer实现了equals方法，比较的是内容，所以会输出true
 ```
ListNode v1 = new ListNode(1);
ListNode v2 = new ListNode(1);
Integer i1=1;
Integer i2=1;
System.out.println(v1.equals(v2));
System.out.println(i1.equals(i2));
```	
2016-11-18 看到3.1.6<br>
注意对一个整数取反，比如i=30；~i=-31；表现在二进制上是对i的二进制所有位数取反。而-i=-30；<br>
看到157页<br>
<li>System.gc()是强制执行垃圾收集器
<ref>

