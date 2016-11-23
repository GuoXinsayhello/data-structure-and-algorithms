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
 表示选择那些中间有jay的，如果不加%表示精确搜索。<br>
 ```sql
 SELECT * from tutorials_tbl ORDER BY tutorial_author ASC
 ```
 这句话表示按照升序排序，如果是DESC表示按照降序排序<br>
 下面是表的联结（join）行为，从两个或者多个表里面搜索，
 ```sql
 mysql> SELECT a.tutorial_id, a.tutorial_author, b.tutorial_count
    -> FROM tutorials_tbl a, tcount_tbl b
    -> WHERE a.tutorial_author = b.tutorial_author;
```
下面的是左联结（left join），左联结会把不符合条件的但是出现在左表中的也输出。
```sql
mysql> SELECT a.tutorial_id, a.tutorial_author, b.tutorial_count
    -> FROM tutorials_tbl a LEFT JOIN tcount_tbl b
    -> ON a.tutorial_author = b.tutorial_author;
```
2016-11-22看到using join<br>
正则表达式：寻找以元音字母开始并以 'ok' 结尾的名称，查询如下：
```sql
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';
```
有很多种支持事务表可供选择，但其中最常见的是 InnoDB。可以用如下的语句构建支持事务的表：<br>
```sql
mysql> create table tcount_tbl
    -> (
    -> tutorial_author varchar(40) NOT NULL,
    -> tutorial_count  INT
    -> ) TYPE=InnoDB;
```
删除、添加列或对其重新定位:<br>
```sql
 ALTER TABLE testalter_tbl DROP i;
 ALTER TABLE testalter_tbl ADD i INT;
 ```
 这两句是删除表中名字叫i的列以及添加一个名字叫i的列，
 ```sql
 ALTER TABLE testalter_tbl ADD i INT AFTER c;
 ```
 这句是添加一个列令其位于c列之后，这样就可以使列重新定位；<br>
 要想改变列的定义，需要使用 MODIFY 或 CHANGE 子句,CHANGE 的语法稍有不同。必须把所要改变的列名放到 CHANGE 关键字的后面，然后指定新的列定义。<br>
 ```sql
 mysql> ALTER TABLE testalter_tbl MODIFY c CHAR(10);
 mysql> ALTER TABLE testalter_tbl CHANGE i j BIGINT;
 ```
 MyISAM 和 InnoDB 讲解 InnoDB和MyISAM是许多人在使用MySQL时最常用的两个表类型，这两个表类型各有优劣，视具体应用而定。基本的差别为：MyISAM类型不支持事务处理等高级处理，而InnoDB类型支持。MyISAM类型的表强调的是性能，其执行速度比InnoDB类型更快.<br>
 ```sql
 CREATE UNIQUE INDEX AUTHOR_INDEX
ON tutorials_tbl (tutorial_author DESC)
```
上面的语句是创建唯一索引，索引能够提高查询速度，mysql> SHOW INDEX FROM table_name\G 后面的\G表示以垂直方式输出。在当前用户会话终止时，临时表会被清除。<br>
如何克隆一个表<br>
使用 SHOW CREATE TABLE 或 CREATE TABLE 语句显示源表的结构、索引以及所有的内容。调整语句，将表名改为克隆表的名称，执行语句。这样就对表进行了克隆。
另外，如果想要克隆表的全部内容，也可以使用 INSERT INTO ... SELECT 语句。不要使用 INSERT ，使用 INSERT IGNORE。如果一个记录没有复制一个已存在的记录，MySQL 就会将它照常插入。如果该记录与现存的某个记录重复，IGNORE 关键字就会让 MySQL 默默地将其摒弃，不会产生任何错误。使用 REPLACE 而不是 INSERT。如果记录是一个新记录，使用 INSERT 就可以了。如果是一个重复记录，新的记录将会替换旧有记录。强制唯一性的另一种办法是为表添加 UNIQUE 索引而不是主键。<br>
group by子句的含义是分类，by后面是条件，据极客学院的wiki说这样也可以消除重复的数据。因为相同的数据都被分为一类了，但是这样的话只会把重复的数据的第一个拿出来，后面重复的数据都不会出现,比如：<br>
```sql
mysql> SELECT last_name, first_name
    -> FROM person_tbl
    -> GROUP BY last_name;
```
