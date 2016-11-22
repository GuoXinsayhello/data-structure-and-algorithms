1.mysql管理
--
使用不同的用户进入mysql会出现不同的数据库，root和一般的用户显现的数据库 不一样。在root模式下可以添加用户，主要存在于mysql这个数据库里面。
索引是对数据库表中一个或多个列（例如，employee 表的姓名 (name) 列）的值进行排序的结构。如果想按特定职员的姓来查找他或她，则与在表中搜索所有的行相比，
索引有助于更快地获取信息。<br>
CREATE TABLE table_name (column_name column_type);<br>
这句话可用于创建一个表，先写列名，然后写该列 的类型。 比如:<br>
```sql
tutorials_tbl(
   tutorial_id INT NOT NULL AUTO_INCREMENT,
   tutorial_title VARCHAR(100) NOT NULL,
   tutorial_author VARCHAR(40) NOT NULL,
   submission_date DATE,
   PRIMARY KEY ( tutorial_id )
);
```

