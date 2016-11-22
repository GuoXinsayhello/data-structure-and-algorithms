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
向一个表中插入数据的语法如下：<br>
```sql
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```
比如：<br>
```sql
mysql> INSERT INTO tutorials_tbl 
     ->(tutorial_title, tutorial_author, submission_date)
     ->VALUES
     ->("Learn PHP", "John Poul", NOW());
```
使用搜索的时候试了一下，发现SELECT * from tutorials_tbl WHERE tutorial_author='Sanjay';使用单引号还是双引号都可以。<br>
更新语句的语法：
```java
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]
```
DELETE FROM table_name [WHERE Clause]这句话用于删除表中的数据；<br>
对于like子句，如果是这样
```sql
SELECT * from tutorials_tbl  WHERE tutorial_author LIKE '%jay%';
 ```
 表示选择那些中间有jay的，如果不加%表示精确搜索。
