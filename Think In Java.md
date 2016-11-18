注意下面这个语句，会输出false和true，因为自己没有实现ListNode的equals方法，equals默认会比较句柄，而Integer实现了equals方法，比较的是内容，所以会输出true
 ```
ListNode v1 = new ListNode(1);
ListNode v2 = new ListNode(1);
Integer i1=1;
Integer i2=1;
System.out.println(v1.equals(v2));
System.out.println(i1.equals(i2));
```	
