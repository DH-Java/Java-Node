 1、1数据库介绍

- 数据库慨念
- 术语介绍

1、2MySQL数据库

- 下载、安装、配置、卸载
- MySQL客户端工具的安装及应用

1、3SQL结构化查询语言 

- 什么是SQL
- SQL操作数据（CRUD操作：添加、查询、修改、删除）

1、4SQL高级

- 存储过程
- 索引
- 触发器、视图

1、5数据库设计

- 数据库设计步骤
- 数据库设计样式
- E-R图
- PowerFesigner建模工具、PDMan

1、6数据库事务

- 什么是事务
- 事务特性ACID
- 事务隔离级别
- 事务管理

# **一、数据库介绍**

## **1.1.1数据库慨念**

数据库就是存放数据的仓库

数据库（DataBase  简称DB）是长期存储在计算机内部有结构的、大量的、共享的数据集合。

- 长期存储：持久存储

- 有结构：

类型：数据库不仅可以存放数据，而且存放的数据还是有类型的

关系：存储数据与数据之间的关系

- 大量：大多数数据库都是文件系统的，也就是说存储在数据库中的数据实际上就是存储在磁盘的文件中
- 共享：多个应用程序可以通过数据库实现数据的共享

## **1.1.2关系型数据库与非关系数据库**

- 关系型数据库

关系型数据库，采用关系模型来组织数据的存储，以及和列的形式存储数据并记录数据与数据之间的关系--将数据存储在表中，可以通过建立表格与表格之间关联来维护数据与数据之间的关系

学生信息--学生表

班级信息--班级表

- 非关系数据库

非关系型数据库，采用键值对的模型来存储数据，只完成数据的记录，不会记录数据与数据之间的关系。

在非关系型数据库中基于其特定的存储结构来解决一些大数据应用的难题

NoSQL数据库来指代非关系型数据库

## **1.1.3常见的数据库产品**

关系型数据库产品

- MySQL

MariaDB

Percona Server

- PostgreSQL
- Oracle
- SQL Server
- Access
- Sybase
- 达梦数据库

非关系型数据库产品

- 面向检索的列式存储

HaBase子系统

BigTable（Google）

- 面向高并发的缓存存储Key-value

**Redis**

MemcaheDB

- 面向海量数据访问的文档存储Document-Oriented

**MongoDB**

CouchDB

## **1.1.4数据库术语**

**数据库（DataBase）：存储数据的集合，提供数据存储的服务**

**数据（Date）：实际上指的是描述事物的符号记录**

**数据库管理系统（Database Management System , DBMS）**；数据库管理系统，是位于用户与操作系统之间的一层数据管理软件

**数据库系统管理员（Datebase Aminstrator ,简称DBA）**；负责数据库创建，使用以及维护的专门人员

**数据库系统（Datebase System，DBS）**；数据库系统管理员、数据库管理系统以及数据库组成整个单元

# **二、MySQL数据库环境准备**

MySQL下载、安装、配置、卸载、安装DBMS、使用DBMS

3、1MySQL版本及下载

3.1.1版本

- MySQL是Oracle的免费的关系型数据库
- MySQL目前的最新版本为8.0.27，在企业项目中主流版本：5.0--5.5--5.6--5.7--8.0.26

3、1.2下载

- 官网下载：mysql.com

镜像下载：https://www.filehorse.com/

# **三、SQL**

4.1、SQL发展

- SQL是在1981年由IBM公司推出，一经推出基于其简洁的语法在数据库中得到了广泛的应用，成为主流数据库的通用规范
- SQL由ANSI组织确定规范
- 在不同的数据库产品中遵守SQL的通用规范，但是也对SQL有一些不同的改进，形成了一些数据库的专有指令

MySQL：limit

SQL Server：top

Oracle：rownum

## **3.1.1SQL分类**

根据SQL指令完成的数据库操作的不同，可以将SQL指令分为四类

- DDL Data Definition Language 数据定义语言

用于完成对数据对象（数据库、数据表、视图、索引等）的创建、删除、修改

- DML Data Manipulation Language 数据操作

用于完成对数据表中的数据的添加、删除、修改操作

1. 添加：将数据存储到数据表
2. 删除：将数据从数据表中移除
3. 修改：对数据表中的数据进行修改

- DQL Data Query Language 数据查询语言

用于将数据表中的数据查询出来

- DCL Data Control Language 数据控制语言

用于完成事务管理等控制性操作

## **3.1.2SQL基本语法**

SQL指令不区分大小写

每条SQL表达式结束之后都用;结束

SQL关键字之间以空格进行分隔

SQL之间可以不限制换行

## **3.1.3DDL数据定义语言**

DDL数据库操作

//使用DDL语句可以创建数据库、查询数据库、修改数据库、删除数据库 //查询数据库 //显示当前mysql中的数据库列表 show databases; //显式指定名称的数据的创建的SQL指令 show create database db_test;  //创建数据库 //创建数据库，dbName表示创建的数据库名称，可以自定义 create databae <dbName> //创建数据库，当指定名称的数据库不存在时执行创建 create database if not exists <dbName>; //在创建数据库的同时指定数据库的字符集（字符集：数据存储在数据库中采用的编码格式（UTF-8 GBK）） create database <dbName> character set utf8;  //修改数据库 修改数据库字符集 alter database dbName character set utf8; //删除数据库 删除数据库时会删除当前数据库中所有的数据表以及数据表中的数据 //删除数据库 drop database dbName； //如果数据库存在则删除数据库 drop database if exists<dbName> //使用/切换数据库 use dbName

## **3.1.4MySQL创建表**

**ubique:一列的数据不能重复**

**char:不可变长度**

**varchar:可变长度**

create table students( stu_num char(8) not null unique , stu_name varchar(20) not null , stu_sex char(2) not null , stu_age int not null , stu_pho char(11) not null unique , stu_qq varchar(10) unique );

查询数据表

show tables

查询表结构

desc students;

删除数据表

drop table studens; //当前数据表存在时删除数据表 drop table if exists students;

修改数据表名

alter table sutdents rename to student;

数据包也是有字符集的，默认字符集和数据一样

alter table student character set utf8;

添加字段（列）

alter table student add stu_pass varchar(200);

修改字符的列表和类型

alter table <表名> change 旧属性名 新属性名 数据类型; 

只修改数据类型

alter table students modify  stu_user varchar(20);

删除字段（列）

alter table students drop stu_user;

删除数据表中的主键约束

alter table students drop primary Key;

创建表之后添加主键约束

alter table studentds modify stu_num char(10) primary Key;

## **3.1.5MySQL数据类型**

数据类型，指的是数据表中的列中支持存放的数据的类型

数值类型

| 类型            | 内存空间大小   | 范围                             | 说明                                       |
| --------------- | -------------- | -------------------------------- | ------------------------------------------ |
| tinyint         | 1byte          | 有符号-128-127无符号0-255        | 小型整数（年龄）                           |
| smallint        | 2byte（16bit） | 有符号-32768-32767无符号0-65535  | 小型整数                                   |
| mediumint       | 3byte          | 有符号-2^31—2^31-1无符号0-2^32-1 | 中型整数                                   |
| **int/integer** | 4byte          |                                  | 整数                                       |
| bigint          | 8byte          |                                  | 大型整数                                   |
| float           | 4byte          |                                  | 单精度                                     |
| double          | 8byte          |                                  | 双精度                                     |
| decimalre       | 第一参数+2     |                                  | decimal(10，2)表示数值一共有10位小数有两位 |

**字符串类型**

存储字符序列的类型

| 类型         | 字符长度     | 说明                                                         |
| ------------ | ------------ | ------------------------------------------------------------ |
| **cha**r     | 0-255字节    | 定长字符串，最多可以存储255个字符，当我们指定数据表字段为char(n)此列中的数据最长为n个字符，如果添加的数据少于n，则补\u0000至n长度 |
| **varchar**  | 0-65536字节  | 可变长度字符串，此类型的类最大长度为65535                    |
| tinyblob     | 0-255字节    | 存储二进制字符串                                             |
| blob         | 0-65535      | 存储二进制字符串                                             |
| mediumblob   | 0-1677215    | 存储二进制字符串                                             |
| longblob     | 0-4294967295 | 存储二进制字符串                                             |
| tinytext     | 0-255        | 文本数据（字符串）                                           |
| text         | 0-65535      | 文本数据（字符串）                                           |
| mediumtext   | 0-1677215    | 文本数据（字符串）                                           |
| **longtext** | 0-4294967295 | 文本数据（字符串）                                           |

**日期类型**

在MySQL数据库中，我们可以使用字符串来存储时间，但是如果我们需要基于时间字段进行查询操作（查询在某个时间段内的数据）就不便于查询实现

| 类型         | 格式                | 说明                        |
| ------------ | ------------------- | --------------------------- |
| **date**     | 2021-11-12          | 日期，只存储年月日          |
| time         | 16;77;38            | 时间，只存储时分秒          |
| year         | 2021                | 年份                        |
| **datetime** | 2021-09-13 11:12:13 | 日期-时间，存储年月日时分秒 |
| timestamp    | 20210913111213      | 日期-时间（时间戳）         |

# **四、字段约束**

在创建数据库的时候，指定的对数据库的列的数据限制性的要求（对表的列中的数据进行限制）

为什么要给表中的列添加约束呢？

- 保证数据的有效性
- 保证数据的完整性
- 保证数据的正确性

字段常见的约束有哪些？

- **非空约束（not null）:限制此列的值必须提供，不能为null**
- **唯一约束（unique）：在表中的多条数据，此列的值不能重复**
- **主键约束（primary key）：非空+唯一，能够唯一标识数据表中的一条数据**
- **外键约束（foreign）：建立不同表之间的关联关系**

## **4.1.1主键自动增长**

在我们创建一张数据表时，如果数据表中有列可以作为主键（例如：学生的学号，图书表的编号）我们可以

直接这是这个列作为主键；

当有些数据表中没有合适的列作为主键时，我们可以额外定义一个与记录本身无关的列（id）作为主键，此

列数据无具体的含义主要用于标识一条记录，在mysql中我们可以将此列定义为int，同时设置为**自动增长**，

当我们先数据表中新增一条记录时，无需提供id的值，它会自动生成。

定义主键自动增长

- 定义int类型字段自动增长 ： 	

create table types(    types_id int primary Key auto_increment,    types_name varchar(20) not null,    types_remark varchar(100) );

注意：自动增长从1开始，每添加一条记录，自动的增加的列会自定加1，当我们把某条记录删除之后在添加数据，自动增长的数据也不会重复生成（自动增长只保证唯一性，不保证连续性）

## **4.1.2联合主键**

create table students(    stu_name varchar(10) not null  ,    stu_age int not null,    stu_number BIGINT not null unique ,    primary key (stu_age,stu_name) );

注意：在实际企业项目的数据库设计中，联合主键使用频率并不高；当一张数据表中没有明确的字段可以作为主键时，我们可以额外添加一个ID字段作为主键。

5、3外键约束

在多表关联部分讲解

# **五、数据操作语言**

用于完成对数据表中数据的插入、删除、修改操作

## **5.1.1插入数据**

语法

**insert into   (columnName..)  value(value1,value2...)**

//向数据表中指定的列添加数据（不允许为空的列必须提供数据） insert into tableName (columnName1,columnName2) values(value1.value2) //数据表名的字段名顺序可以不与表中一致，但是values中的值的顺序必须与表名后字段名顺序对应 //当要向表中的所有列添加数据时，数据表后面的字段列表可以省略，但是values中的值的顺序要与数据报表定义的字段保持一致： //不过在项目开发中，即使要向所有列添加数据也要把columnName写出来。 insert into students values ('20602063','刘鑫宇',18,'男','2960157343');

JDBC语法 String sql = "insert into xs.students (stu_id,stu_name,stu_age,stu_sex) values('20602061','邓杰',19,'男')";

## **5.1.2删除数据**

**从数据表中删除满足特定条件（所有）的数据**

语法

delete form <TableName> where conditions

//输出id为20602063的信息 delete from students where stu_id='20602063'; //删除年龄大于18的信息（如果满足where的消息有多条则删除多条） delete from students where stu_age>18; //如果删除语句没有where子句，则删除数据表中所有数据（敏感操作） delete from students;  //删除字段 alter table students drop column stu_date;

## **5.1.3修改数据**

**对数据表中已经添加的记录进行修改**

语法

update <tableName> set columnName=value [where condittions]

示例

//将姓名为邓勇的信息的名字改为'邓朝均' update students set stu_name='邓朝均' where stu_name='邓勇'; //将姓名为邓朝均的信息，年龄修改为40，qq修改为88888888 （修改多条） update students set stu_age=40,stu_qq='2524851299' where stu_name='邓朝均';

# **六、DQL数据查询语言**

从数据表中提取满足特定条件的记录

- 单表查询
- 多表联合查询

查询的基础语法

语法

//select 关键字指定要显示查询到的记录的那些列 select colnumnName1,[colnumnName2,colnumnName3] from students; //如果要显示查询到的记录的所有列，则可以使用*代替字段名列表（在项目中不建议使用） select * from students;

# **七、where子句**

**在删除，修改及查询的语句后都可以添加where子句（条件），用于筛选满足特定的添加的数据进行删除、修改和查询操作。**

delete from tableName where condittions; update tableName set.... where condittions; select ..... from tableName where condittions;

条件

//  = 等于 select * from students where stu_id = '20602061'; // !=  <>  不等于、 select * from students where stu_id != '20602061'; select * from students where stu_id <> '20602061'; // > 大于 select * from students where stu_age > 18; // < 小于  select * from students where stu_age <18; // >= 大于等于 select * from students where stu_age >=18; //< = 小于等于 select * from students where stu_age <=18; //between and  区间查询  between v1 and v2   [v1,v2] select * from students where stu_age between 18 and 30;

## **7.1.1条件逻辑运算符**

在where子句中，可以将多个条件通过逻辑运算（and or not）进行连接，通过多个条件来筛选要操作的数据

//and 筛选多个条件同时满足的记录 select * from students where stu_age<20 and stu_sex='男'; //or 或者 筛选多个条件中至少满足一个条件的记录 select * from students where stu_qq='676366223' or stu_qq='2413027560'; //not 取反 select * from students where stu_age not between 18 and 30;

## **7.1.2LIKE子句**

在where子句的条件中，我们可以使用like关键字来实现模糊查询

语法

select * from students where stu_name like '%豪%';

- 在like关键字后的%符号

1. %表示任意多个字符【%豪%包含豪】
2. _表示任意一个字符【_o%第二个字母为o】

## **7.1.3对查询结果的处理**

设置查询的列

声明显示查询结果的指定列

select colnumnName1， colnumnName2 from students where stu_age>18;

计算列

对从数据表中查询的记录的列进行一定的运算之后显示出来

//出生年份 = 当前年份 - 年龄 select stu_name , 2021-students.stu_age from students; 邓豪,2002 邓杰,2002 邓娟,1991 邓朝均,1981

字段别名

我们可以为查询结果的列名 取一个语义性更强的别名

as关键字

select stu_name , 2021-students.stu_age as stu_brith from students;

消除重复行

从查询的结果中将重复的记录删除 **disinct**

select distinct stu_age from students;

## 7.1.4order by排序

将查询到的满足条件的记录按照指定的列的值的升序/降序排列

语法

select * from students where condittions order by columnName；

- order by columnName 表示将查询的结果按照指定的列排序
- order by columnName  asc 按照指定的列升序。（默认）
- order by columnName  desc 按照指定的列降序

多字段排序 ：先满足第一个排序规则，当第一个排序的值相同时再按照第二个列的规则排序

select * from students where stu_name like '邓%' order by stu_name desc ,stu_qq asc ;

# **八、聚合函数**

SQL中提供了一些可以对查询的记录的列进行计算的函数--聚合函数

- count（）**统计函数，统计满足条件的指定字段值的个数（记录数）

select count(stu_id) from students where stu_sex='男';

- **max（）**获取最大值，查询满足条件的记录中指定列的最大值

select max(stu_age) from students where stu_sex='男';

- **min（）**获取最小值，查询满足条件的记录中指定列的最小值

select min(stu_age) from students where stu_sex='男';

- **sum（）**计算和，查询满足条件的记录中指定列的总和

select sum(stu_age)from students where stu_sex = '男';

- **avg（）**平均值，查询满足条件的记录中指定列的平均值

select avg(stu_age) from students where stu_sex='男';

## **8.1.1日期函数**

当我们向日期类型的列添加数据时，可以通过字符串类型赋值（字符串的格式必须为yyyy-MM-dd hh:mm:ss）

如果我们想要获取当前系统时间添加到日期类型的列，可以使用now（）或者sysdate（）

通过字符串类型，给日期类型的列赋值 insert into students(stu_id, stu_name, stu_age, stu_sex, stu_qq, stu_date) VALUES ('20602065','张三',20,'男','313414142','2021-09-01 16:27'); 通过now（）获取当前时间 insert into students(stu_id, stu_name, stu_age, stu_sex, stu_qq, stu_date) VALUES ('20602066','李四',21,'男','313414143',now()); 通过sysdate()获取当前时间 insert into students(stu_id, stu_name, stu_age, stu_sex, stu_qq, stu_date) VALUES ('20602067','王五',22,'男','313414144',sysdate()); 通过now和sysdate获取当前时间 select now()/sysdate();

## **8.1.2字符串函数**

就是通过SQL指令对字符串进行处理

示例

//语法 select concat(colnum1,colnum2....)form tableName; select concat(stu_name,'=',stu_sex) from students; 张杰=男 邓杰=男 邓朝均=男  upper（colnum）将字段的值转换为大写 select upper(stu_name) from students; MIKE  lower（colnum）将字段的值转换为小写 select lower(stu_name) from students; mike substring（colnum，start，len）从指定列中截取部分显示，start从1开始 select stu_name , substring(stu_qq,7,4) from students;

# **九、分组查询**

分组-就是将数据表中的的记录按照指定的类进行分组

select 分组字段/聚合函数 from 表名 [where 条件] group by 分组列名 [having 条件] [order by 排序字段] select stu_age ,count(stu_id) from students where stu_sex='男' group by stu_age;

- select后使用*显示对查询的结果进行分组之后，显示每组的第一条记录（这种显示通常是无意义的）
- select后通常显示分组字段和聚合函数对分组后的数据进(统计、求和、平均值等)

语句执行属性

1. 先根据where条件从数据库查询记录
2. group by 对查询记录进行分组
3. 执行having对分组后的数据进行筛选

select stu_age ,count(stu_id) from students where stu_sex='男' group by stu_age order by stu_age desc ; 统计男生各年龄段的人数和进行倒序排列

统计各个年龄段人数大于1的正序排列出来

select stu_age,count(stu_id) from students  group by stu_age having count(stu_id)>1 order by stu_age;

# **十、分页查询**

当数据表中的记录比较多的时候，如果一次性全部查询出来显示给用户，用户的可读性/体验性就不太好，因为此我们可以将这些数据进行分页进行展示

语法

select ...from..where..limit param1,param2

- **param1 int,表示获取查询语句的结果中的第一条数据的索引（索引从0开始）**
- **param2 int 表示获取的查询记录的条数（如果剩下的数据条**

案例：对数据表中的学生信息进行分页显示，总共10条数据，我们每页显示3条

总记录数 count 10

每页显示 pageSize 3

总页数 pageCount = count%pageSize==0? count/pageSzie :count/pageSize +1;

//查询第一页： select * from tableName [where ...] limit 0 , 3; //查询第二页 select * from tableName [where ...] limit 3 , 3; //查询第三页 select * from tableName [where ...] limit 6 , 3; //查询第四页 select * from tableName [where ...] limit 9 , 3; //如果一张数据表中，pageNum表示查询的页码，pageSize表示每页显示的条数： select * from tableName [where 条件] limit（pageNum-1）* pageSize;

# **十一、数据表的关联关系**

## **11.1.1关联关系介绍**

MySQL是一个关系型数据库，不仅可以存储数据，还可以维护数据之间的关系--通过在数据表中添加字段建立外键约束

数据与数据之间的关系分为四种

- 一对一关联
- 一对多关联
- 多对一关联
- 多对多关联

## **11.1.2一对一关联**

人——身份证  一个人只有一个身份证，一个身份证只对应一个人

学生——学籍  一个学生只有一个学籍，一个学籍也对应唯一的一个学生

用户——用户详情 一个用户只有一个详情，一个详情也只对应一个用户

**方案一：主键关联--两张数据表中主键相同的数据为相互对应的数据**

**方案二：唯一外键--在任意一张表中添加一个字段添加外键约束与另一张表主键关联，并且将外键列添加唯一约束**

## **11.1.3一对多与多对一**

班级-学生 （一对多） 一个班级包含多个学生

学生-班级 （多对一） 多个学生可以属于同一个班级

图书-分类  	商品-商品类别

**方案：在多的一端添加外键，与一的一端主键进行关联**

## **11.1.4多对多关联**

学生-课程 一个学生可以选择多门课，一门课程也可以由多个学生选择

会员-社团 一个会员可以参加多个社团，一个社团也可以招纳多个会员

**方案：额外创建一张关系表来维护多对多关联--在关系表中定义两个外键，分别与两个数据表的主键进行关联**

# **十二、外键约束**

外键约束--将一个列添加外键约束与另一张表的主键（唯一列）进行关联之后，这个外键约束的列添加的数据必须要在关联的主键字段中存在

案列：学生表 与 班级表

## **12.1.1、先创建班级表**

create table classes(    class_id int primary key auto_increment,    class_name varchar(40) not null unique ,    class_remark varchar(200) );

## **12.1.2、****创建学生表（在学生表中添加外键与班级表的主键进行关联）**

\#【方式一】在创建表的时候，定义cid字段，并添加外键约束 #由于cid列 要与classes表的class_id进行关联，因此cid字段和长度要与class_id一致 create table students(    stu_num char(8) primary key ,    stu_name varchar(20) not null ,    stu_gender char(2) not null ,    stu_age int not null ,    cid int,    constraint Fk_STUDENTS_CLASSES foreign key(cid) references classes(class_id) ); #【方式二】先创建表，为cid添加外键约束 alter table students add constraint FK_STUDENTS_CLASSES foreign key(cid) references classes(classes_id); #删除外键约束 alter table students drop foreign key Fk_STUDENTS_CLASSES;

## **12.1.3、向班级表添加班级信息**

insert into classes (class_name, class_remark) VALUES ('Java2104','...'); insert into classes (class_name, class_remark) VALUES ('Java2105','...'); insert into classes (class_name, class_remark) VALUES ('Java2106','...'); insert into classes (class_name, class_remark) VALUES ('Java2107','...'); 1,Java2104,... 2,Java2105,... 3,Java2106,... 4,Java2107,...

## **12.1.4、向学生表中添加学生信息**

insert into students(stu_num, stu_name, stu_gender, stu_age, cid) VALUES ('20210101','张三','男',20,'1'); #添加学生时，设置给cid外键列的值必须在其关联的主表classes的classes_id列存在 insert into students(stu_num, stu_name, stu_gender, stu_age, cid)  VALUES ('20210101','张三','男',20,'5');

## **12.1.5、外键约束修改和删除**

当学生表中学生信息关联班级表的某条记录时，就不能对班级表的这条记录进行修改外键数据和删除操作

班级表中class_id的班级信息，被学生表中的记录关联了

我们就不能修改classes表中的class_id，并且不能删除

mysql> desc students; +------------+-------------+------+-----+---------+-------+ | Field      | Type        | Null | Key | Default | Extra | +------------+-------------+------+-----+---------+-------+ | stu_num    | char(8)     | NO   | PRI | NULL    |       | | stu_name   | varchar(20) | NO   |     | NULL    |       | | stu_gender | char(2)     | NO   |     | NULL    |       | | stu_age    | int         | NO   |     | NULL    |       | | cid        | int         | YES  | MUL | NULL    |       | +------------+-------------+------+-----+---------+-------+ 5 rows in set (0.00 sec) mysql> select * from classes; +----------+------------+--------------+ | class_id | class_name | class_remark | +----------+------------+--------------+ |        1 | Java2104   | ...          | |        2 | Java2105   | ...          | |        3 | Java2106   | ...          | |        4 | Java2107   | ...          | +----------+------------+--------------+ 4 rows in set (0.00 sec) mysql> select * from students; +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210101 | 张三     | 男         |      20 |    1 | | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | | 20210105 | 唐三     | 男         |      24 |    1 | +----------+----------+------------+---------+------+ 5 rows in set (0.00 sec) mysql> update classes set class_id=5 where class_id = 1; ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`db_test01`.`students`, CONSTRAINT `Fk_STUDENTS_CLASSES` FOREIGN KEY (`cid`) REFERENCES `classes` (`class_id`)) mysql> delete from classes where class_id = 1; ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`db_test01`.`students`, CONSTRAINT `Fk_STUDENTS_CLASSES` FOREIGN KEY (`cid`) REFERENCES `classes` (`class_id`))

如果一定要修改classes表中的class_id,该如何实现呢？

- 将关联在classes表中的class_id的学生记录中的cid改为null
- 在修改班级表中的class_id
- 在学生信息表中的cid设置为null的记录的cid重新修改为classes表中的新的class_id

1、update students set cid=null where cid=1;#结果如下 +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210101 | 张三     | 男         |      20 | NULL | | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | | 20210105 | 唐三     | 男         |      24 | NULL | +----------+----------+------------+---------+------+  2、update classes set class_id = 5 where class_id=1;#结果如下   +----------+------------+--------------+ | class_id | class_name | class_remark | +----------+------------+--------------+ |        2 | Java2105   | ...          | |        3 | Java2106   | ...          | |        4 | Java2107   | ...          | |        5 | Java2104   | ...          | +----------+------------+--------------+  3、update students set cid = 5 where cid is null;#结果如下 +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210101 | 张三     | 男         |      20 |    5 | | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | | 20210105 | 唐三     | 男         |      24 |    5 | +----------+----------+------------+---------+------+

## **12.1.6我们可以使用级联操作来实现：**

1、再添加外键时，设置级联修改和级联删除

\#删除原有的外键 alter table students drop foreign key Fk_STUDENTS_CLASSES; #重新添加外键，并设置级联修改和级联删除 alter table students  add constraint FK_STUDENTS_CLASSES foreign key(cid) references classes(class_id) ON UPDATE CASCADE ON DELETE CASCADE;

2.测试级联修改：

\#班级信息 +----------+------------+--------------+ | class_id | class_name | class_remark | +----------+------------+--------------+ |        2 | Java2105   | ...          | |        3 | Java2106   | ...          | |        4 | Java2107   | ...          | |        5 | Java2104   | ...          | +----------+------------+--------------+ #学生信息 +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210101 | 张三     | 男         |      20 |    5 | | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | | 20210105 | 唐三     | 男         |      24 |    5 | +----------+----------+------------+---------+------+ #直接修改classes表中的class_id，关联class_id这个id的学生记录的cid也会同步修改 update classes set class_id =1 where class_id=5; #班级信息 +----------+------------+--------------+ | class_id | class_name | class_remark | +----------+------------+--------------+ |        1 | Java2104   | ...          | |        2 | Java2105   | ...          | |        3 | Java2106   | ...          | |        4 | Java2107   | ...          | +----------+------------+--------------+ #学生信息 +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210101 | 张三     | 男         |      20 |    1 | | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | | 20210105 | 唐三     | 男         |      24 |    1 | +----------+----------+------------+---------+------+

3、测试级联删除

\#删除class_id的班级信息，学生表引用此班级信息的记录也会被同步删除 delete from classes where class_id = 1; +----------+------------+--------------+ | class_id | class_name | class_remark | +----------+------------+--------------+ |        2 | Java2105   | ...          | |        3 | Java2106   | ...          | |        4 | Java2107   | ...          | +----------+------------+--------------+ #学生信息表 +----------+----------+------------+---------+------+ | stu_num  | stu_name | stu_gender | stu_age | cid  | +----------+----------+------------+---------+------+ | 20210102 | 李四     | 男         |      21 |    2 | | 20210103 | 王五     | 男         |      22 |    3 | | 20210104 | 刘六     | 男         |      23 |    4 | +----------+----------+------------+---------+------+

# **十三、连接查询**

通过对DQL的学习，我们可以很轻松的从一张数据表中查询出需要的数据；在企业的应用开发中，我们经常需要从多张表中查询数据（例如：我们查询学生信息的时候需要同时查询学生的班级信息），可以通过连接查询从多张数据表提取数据：

在MySQL中可以使用join实现多表的联合查询--连接查询，join按照其功能不同分为三个操作；

- inner join 内连接
- left join 左连接
- right join 右连接

## **13.1.1创建数据表**

 创建班级信息表和 学生信息表

CREATE TABLE classes( class_id int PRIMARY KEY auto_increment, class_name VARCHAR(40) not NULL UNIQUE, class_remark VARCHAR(200) ); CREATE TABLE students( stu_num char(8) PRIMARY KEY, stu_name VARCHAR(20) not NULL, stu_gender CHAR(2) NOT NULL, stu_age int NOT NULL, cid INT, CONSTRAINT FK_STUDENTS_CLASSES FOREIGN KEY (cid) REFERENCES classes(class_id) ON UPDATE CASCADE ON DELETE CASCADE );

## **13.1.2添加数据**

添加班级信息

\#Java2104包含三个学生信息 INSERT INTO classes(class_name,class_remark) VALUES('Java2104','...'); #Java2105包含二个学生信息 INSERT INTO classes(class_name,class_remark) VALUES('Java2105','...'); #以下两个班级在学生表中没有对应的学生信息 INSERT INTO classes(class_name,class_remark) VALUES('Java2106','...'); INSERT INTO classes(class_name,class_remark) VALUES('Java2107','...');

添加学生信息

\#以下三个学生信息，属于class_id=1的班级（Java2104） INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210101','陈平安','男',18,1); INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210102','崔东山','男',14,1); INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210103','姜尚真','男',19,1); #以下两个学生信息，属于class_id=2的班级（Java2105） INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210104','李柳','女',17,2); INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210105','李宝瓶','女',16,2); #以下两个没有设置班级信息 INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210105','裴钱','女',16); INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid) VALUES('20210105','曹晴朗','男',16);

## **13.1、3内连接INNER JOIN**

语法

select ... from tableName1 inner join tableName2;

### **13.1.1笛卡尔积**

- 笛卡尔积（A集合&B集合）：使用A中的每个记录一次关联B中每个记录，笛卡尔积的总数=A总数*B总数
- 如果直接执行select  ... from tableName1 inner  join tableName2；会获取两种数据表中的数据集合的笛卡尔积（依次使用tableName1表中的每一条记录去匹配table2的每条记录）

### **13.1.2内连接条件**

两张表使用innerjoin 连接查询之后生产的笛卡尔积数据中很多数据都是无意义的，我们如何消除无意义的数据呢？--添加两张进行连接查询时的条件

- 使用on设置两张表连接查询的匹配条件

--使用where设置过滤条件：先生成笛卡尔积再从笛卡尔积中过滤数据（效率很低） SELECT*FROM students INNER JOIN classes WHERE students.cid = classes.class_id; --使用on设置连接查询条件：先判断连接条件是否成立，如果两张表的数据进行组合生成一条结果记录 SELECT*FROM students INNER JOIN classes ON students.cid = classes.class_id;

- 结果：只获取两种表中匹配条件成立的数据，任何一张表在另一张表如果没有找到对应匹配则不会出现在查询结果中（例如：裴钱和曹晴朗没有对应的班级信息，Java2106和Java2107没有对应的学生）。

### **13.2、左连接LEFT JOIN**

左连接：显示左表中的所有数据，如果在有右表中存在与左表记录满足匹配条件的数据，则进行匹配；如果右表中不存在匹配数据，则显示为Null

需求:请查询出所有的学生信息，如果学生有对应的班级信息，则将对应的班级信息也查询出来

语法

SELECT*FROM leftTable LEFT JOIN rightTable ON students.cid = classes.class_id;

左连接，显示左表中的所有数据

SELECT*FROM students LEFT JOIN classes ON students.cid = classes.class_id;

​    ![0](https:////note.youdao.com/yws/res/b/WEBRESOURCE2065d278641c606c379dfbf99e2ed2bb)

### **13.3右连接RIGHT JOIN**

语法

SELECT*FROM leftTable RIGHT JOIN rightTable ON students.cid = classes.class_id;

右连接，显示右表中的所有数据

SELECT*FROM students RIGHT JOIN classes ON students.cid = classes.class_id;

​    ![0](https:////note.youdao.com/yws/res/f/WEBRESOURCE41b8c297d7ce3e17e844523beb2fcf2f)

## **13.1.4数据库别名**

如果在连接查询的多张表中存在相同名字的字段，我们可以使用表名、字段名来区分，如果表名太长则不便于SQL语句的编写，我们可以使用数据库别名

使用示例

SELECT s.*,class_name

FROM students s

RIGHT JOIN classes c

ON s.cid = c.class_id;

## **13.1.5子查询/嵌套查询**

子查询-先进行一次查询，第一次查询的结果作为第二次查询的源/条件（第二次查询是基于第一次的查询结果来进行的）

案例1：查询班级名称为‘Java2104’班级中的学生信息（只知道班级名称，而不知道班级ID）

- 传统的方式

-- a.查询Java2104班的班级编号 select class_id FROM classes WHERE class_name = 'Java2104' -- b.查询此班级编号下的学生信息 SELECT * FROM students WHERE cid = 1

- 子查询

如果子查询返回的结果集是一个值（单行单列），条件可以直接使用关系运算符（=   !=） select * from students where cid = (select class_id from classes WHERE class_name = 'Java2104')

案例2：查询所有的Java班级编号

- 传统的方式

-- a.查询所有Java班级编号 select class_id from classes where class_name like  'Java%'; -----1 -----2 -----3  -- b.查询此班级编号的学生信息（union将多个查询语句的结果整合在一起） SELECT * FROM students WHERE cid = 1 union SELECT * FROM students WHERE cid = 2 union SELECT * FROM students WHERE cid = 3

- 子查询

如果查询返回的结果是多个值（单列多行），条件使用in/not in SELECT * FROM students WHERE cid IN (select class_id from classes where class_name like  'Java%')

案例3：查询cid=1的班级中性别为男的学生信息

-- 多条件查询 select * FROM students WHERE  cid = 1 and stu_gender = '男' -- 子查询:先查询cid=1班级中的所有学生信息，将这些信息作为一个整体虚拟表（多行多列） --在基于这个虚拟表查询性别为男的学生信息（‘虚拟表需要别名’） select * FROM (select * from students WHERE cid = 1) t WHERE t.stu_gender = '男' 

# **十四、存储过程**

## **14.1.1存储过程介绍**

​    ![0](https:////note.youdao.com/yws/res/2/WEBRESOURCE707ffa059dc584b7a6243d9032275572)

从SQL执行的流程中我们分析存在的问题

1. 如果我们需要重复多次执行相同的SQL，SQL执行都需要通过连接传递到MySQL，并且需要经过编译和执行的步骤；
2. 如果我们需要连续执行多个SQL指令，并且第二个SQL指令需要使用第一个SQL指令执行的结果作为参数；

存储过程的介绍

存储过程：

将能够完成特定功能的SQL指令进行封装（SQL指令集），编译之后存储在数据库服务器上，并且为其取一个名字，客户端可以通过名字直接调用这个SQL指令集，获取执行结果。

## **14.1.2存储过程优缺点分析**

存储过程优点

1. SQL指令无需客户端编写，通过网络传送，可以节省网络开销，同时避免SQL指令在网络传输过程中被恶意篡改保证安全性
2. 存储过程经过编译创建并保存在数据库中的，执行过程无需重复的进行编译操作，对SQL指令的执行过程进行了性感提升
3. 存储过程中多个SQL指令之间存在逻辑关系，支持流程控制语句（分支、循环），可以实现更为复杂的业务

存储过程缺点

1. 存储过程是根据不同的数据库进行编译、创建并存储在数据库中；当我们需要切换到其它的数据库产品时，需要重新编写针对于新数据库的存储过程
2. 存储过程受限于数据库产品，如果需要高性能的优化会成为一个问题
3. 在互联网项目中，如果需要数据库的高（连接）并发访问，使用存储过程会增加数据库的连接执行时间（因为我们将复杂的业务交给了数据库进行处理）

## **14.1.3创建存储过程**

存储过程创建语法

--语法 create procedure <proc_name>([IN/OUT args]) begin        ---SQL end;

示例

--创建一个存储过程实现加法运算：Java语法中，方法是有参数和返回值的                                        存储过程中，是有输入参数 和 输出参数的 CREATE PROCEDURE proc_test(IN a int ,in b INT , OUT c INT) begin 	SET c = a + b; end;

## **14.1.4调用存储过程**

--定义变量@m SET @m = 0; --调用存储过程，将3传递给a,将2传递给b,将@m传递给c CALL proc_test(3,2,@m)  --显示变量值 select @m FROM DUAL

## **14.1.5存储过程中变量的使用**

存储过程中的变量分为两种：局部变量和用户变量

定义局部变量

局部变量：定义在存储过程中的变量，只能在存储过程内部使用

- 局部变量定义语法

--局部变量定义在存储过程中，而且必须定义在存储过程开始 declare <arr_name> <type> [default value];

- 局部变量定义示例

CREATE PROCEDURE proc_text2(IN a int , OUT i int) begin DECLARE x int DEFAULT 0;--定义x int类型，默认值为0 DECLARE y int DEFAULT 1;--定义y int类型，默认值为1 set x = a*a; set y = a/2; set i = x+y; end;

定义用户变量

用户变量：相当于全局变量，定义的用户变量可以通过select @arrName from  dual进行查询

- 定义用户变量

--用户变量会存储在mysql数据库的字典中（dual） --用户变量定义使用set关键字直接定义，变量名要以@开头 set @n =1;

给变量设置值

- 无论是局部变量还是用户变量，都是使用set关键字修改值

set @s = 0; call proc_text2(6,@s); SELECT @s FROM DUAL;

## **14.1.6将查询结果赋值给变量**

在存储过程中使用select...into给变量赋值

--查询学生数量 CREATE PROCEDURE proc_text3(OUT d INT) BEGIN SELECT COUNT(stu_num) INTO d FROM students;--并查询到学生数量赋值给d END; --调用存储过程 CALL proc_text3(@s); SELECT @s FROM DUAL;

## **14.1.7存储过程的参数**

MySQL存储过程的参数一共有三种：IN\OUT\INOUT

- IN输入参数

输入参数--在调用存储过程中传递数据给存储过程的参数(在调用的过程必须为具有实际值的变量或者字面值)

--创建存储过程，添加学生信息 CREATE PROCEDURE text4(IN num char(8) , IN name1 VARCHAR(20) , IN gender CHAR(2) , IN age int , In cid_x int) begin INSERT INTO students (stu_num,stu_name,stu_gender,stu_age,cid)VALUES (num ,name1,gender,age,cid_x); end; CALL text4('20210108','宁姚','女',18,1);

- OUT输出参数

输出参数--将存储过程中产生的数据返回给过程调用着，相当于Java方法的返回值，但不同的是一个存储过程可以有多个输出参数

--创建存储过程，根据学生学号，查询学生姓名 CREATE PROCEDURE proc_test5(IN num CHAR(8) , OUT sname VARCHAR(20)) BEGIN select stu_name  INTO sname FROM students where stu_num = num; end; set @n =''; CALL proc_test5('20210108',@n); SELECT @n FROM DUAL;

- INOUT输入输出参数

CREATE PROCEDURE proc_test6(INOUT str CHAR(8)) BEGIN select stu_name  INTO str FROM students where stu_num = str; end; set @n ='20210108'; CALL proc_test6(@n); SELECT @n FROM DUAL;

## **14.1.8存储过程中流程控制**

在存储过程中支持流程控制语句用于实现逻辑的控制

- 分支语句

- - if then else

--单分支：如果条件成立，则执行SQL CREATE PROCEDURE proc_test6(IN a int) BEGIN if a=1 then //如果参数为真，则添加一条数据 INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210109','石柔','女',24); END IF; end;

--双分支：如果条件成立则执行SQL，否则执行SQL2 CREATE PROCEDURE proc_test7(IN a int) BEGIN if a=1 then INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210110','陈灵均','男',17); ELSE INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210111','王猛','男',20); END IF; end;

- case

CREATE PROCEDURE proc_test8(IN a int) BEGIN case a when 1 then INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210112','马甜儿','女',17); when 2 THEN INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210113','周枫','男',20); Else INSERT INTO students (stu_num,stu_name,stu_gender,stu_age) VALUES('20210114','张小胖','男',22); END case; end;

## **14.1.9循环语句**

- while

CREATE PROCEDURE proc_test9(IN a INT) BEGIN DECLARE x int DEFAULT 0; while x<a do INSERT into classes(class_name,class_remark)VALUES(CONCAT('Go',x) ,'...'); set x = x+1; end WHILE; END; call proc_test9(3)

- repeat

CREATE PROCEDURE proc_test10(IN a INT) BEGIN DECLARE x int DEFAULT 0; REPEAT  INSERT into classes(class_name,class_remark)VALUES(CONCAT('Python',x) ,'...'); set x = x+1; UNTIL x > a end REPEAT; END;

- Loop

CREATE PROCEDURE proc_test11(IN a INT) BEGIN DECLARE x int DEFAULT 0; MyLoop:LOOP INSERT into classes(class_name,class_remark)VALUES(CONCAT('C++',x) ,'...'); set x = x+1; IF x=a then LEAVE MyLoop; END IF; end LOOP; END;

## **14.1.10查询存储过程**

> ​	存储过程是属于某个数据库的，也就是说当我们将存储过程创建在某个数据库之后，只能在当前数据库中调用此存储过程。
>
> 查询存储过程：查询某个数据库中有哪些存储过程

```my
--根据数据库名，查询当前数据库中的存储过程 
show PROCEDURE STATUS WHERE db='db_test01'; 
--查询存储过程的创建细节
show create procedure db_test01.proc_test1;
```



## **14.1.11修改存储过程**

修改存储过程值得是修改存储过程的特征/特性

```my
alter procedure <proc_name> 特征1 [特征2 特征3....]
```

存储过程的特征参数：

- CONTAINS SQL 表示子程序包含SQL语句，但不包含读或写数据的语句

- NO SQL 表示子程序中不包含SQL语句

- READS SQL DATA 表示子程序中包含读数据的语句

- MODIFIES SQL DATAA 表示子程序中包含写数据的语句

- SQL SECURITY{DEFINER|INVOKER}指明谁有权利来执行

- - DEFINER表示只有定义着自己才能够执行
  - INVOEKR表示调用者可以执行

- COMMENT 'String' 表示注释信息

```mysq
alter procedure proc.test1 READS SQL DATA;
```

## 14.1.12删除存储过程

--删除存储过程 

 --drop删除数据库中的对象，数据库、数据表、列、存储过程、视图、触发器、索引

 --delect删除数据表中的数据 

```mysql
drop procedure proc_test1;
```

## 14.1.13存储过程练习案列

```sql
--创建数据库
create database db_test02;

--使用db_test02数据库
use db_tset02;



--创建图书信息表
CREATE TABLE books(
book_id INT PRIMARY key auto_increment,
book_name VARCHAR(50) not NULL,
book_author varchar(20) not NULL,
book_price decimal(10,2) not null,
book_stock int not null,
book_desc VARCHAR(200)
);

-- 添加图书信息
INSERT into books(book_name,book_author,book_price,book_stock,book_desc) VALUES ('Java程序设计','亮亮',38.80,12,'亮亮老师带你学Java');
INSERT into books(book_name,book_author,book_price,book_stock,book_desc) VALUES ('Java王者之路','成哥',44.40,9,'千峰成哥,Java王者领路人');
-- 创建学生信息表
CREATE table students(
stu_num char(8) PRIMARY key,
stu_name varchar(20) not NULL,
stu_gender char(2) not null,
stu_age int not null
);

-- 添加学生信息
INSERT INTO students(stu_num,stu_name,stu_gender,stu_age) VALUES ('1001','张三','男',20);
INSERT INTO students(stu_num,stu_name,stu_gender,stu_age) VALUES ('1002','李四','女',20);
INSERT INTO students(stdb_tset02;u_num,stu_name,stu_gender,stu_age) VALUES ('1003','王五','男',20);
```

**业务分析**

> 那个学生借那本书,借了多少本
>
> 操作
>
> - 保存借书记录
> - 修改图书库存
>
> 条件
>
> - 判断学生是否存在?
> - 判断图书是否存在  , 库存是否充足?

**创建借书记录表**

```sql
create table records (
rid  int PRIMARY key auto_increment,
snum char(8) not null,
bid int not null,
borrow_num int not null,
is_return int not null,
borrow_date date not null,
constraint FK_RECORDS_STUDENTS foreign key (snum) references students(stu_num),
constraint FK_RECORDS_BOOKS foreign key (bid) references books(book_id)
);
```

## 14.1.14创建存储过程实现借书业务

```sql
-- 实现借书业务:
-- 参数1:  a 输入参数  学号
-- 参数2:  b 输入参数  图书编号
-- 参数3   m 输入参数  图书的数量
-- 参数4   state 输入参数 借书的状态 (1.结束成功 2.学号错误 3.图书不存在 4.库存不足)

create procedure proc_borrow_book(in a char(8),in b int , in m int ,out state int)
begin
	declare stu_count int default 0;
	declare book_count int default 0;
	declare stock int default 0;
-- 	判读学号是否存在,根据参数a到学生信息表查询是否有stu_num=a 的记录
	select count(stu_num) into stu_count from students where stu_num = a;
	if stu_count > 0 THEN
-- 	学号存在
-- 判断图书ID是否存在:根据参数b 查询图书记录总数
	select count(book_id) into book_count from books where book_id = b;
	if book_count >0 then 
-- 	图书存在
-- 判断图书库存是否充足,查询当前图书库存,然后和参数m进行比较
	select book_stock into stock from books where book_id = b;
	if stock > m	then
-- 	执行借书
-- 	操作一
-- 在借书记录表中添加记录
	insert into records(snum,bid,borrow_num,is_return,borrow_date) values(a,b,m,0,sysdate());
-- 	操作二
-- 修改图书库存
	update books set book_stock=stock-m where book_id= b;
	set state = 1;
	else
-- 	库存不足
	set state = 4;
	end if;
	else
-- 	图书不存在
	set state = 3;
	end if;
	else
-- 	学号不存在
	set state = 2;
	end if;
end;

-- 调用存储过程借书
-- 学号不存在
set @state=0;
call proc_borrow_book('1004',1,2,@state);
select @state from dual;

-- 调用存储过程借书
-- 图书编号不存在
set @state=0;
call proc_borrow_book('1001',3,2,@state);
select @state from dual;

-- 调用存储过程借书
-- 图书库存不足
set @state=0;
call proc_borrow_book('1001',1,15,@state);
select @state from dual;

-- 调用存储过程借书
set @state=0;
call proc_borrow_book('1001',1,2,@state);
select @state from dual;
```

## 14.1.15游标

> 问题:如果我们要创建一个存储过程,需要返回查询语句查询到的多条数据,该如何实现那?

1.游标的概念

游标可以用来依次取出查询结果集中的每一条数据--逐条读取查询结果集中的记录

2.游标的使用步骤

1)声明游标

- 声明游标语法:

```sql
Declare cursor_name  CURSOR FOR select_statement;
```

- 实例

```sql
declare mycursor cursor for select book_name,book_author,book_price from books;
```

2)打开游标

- 语法

```sql
open mycursor;
```

3)使用游标

- -- 使用游标:提取游标当前指向的记录(提取之后,游标自动下移)

```sql
fetch mycursor into bname,bauthor,bprice;
```

4)关闭游标

 ```sql
 close mycursor;
 ```

5)案列

```sql
create procedure test01(out result varchar(200))
begin
declare bname varchar(20);
declare bauthor varchar(20);
declare bprice decimal(10,2);
declare num int;
declare i  int;
declare str varchar(50);
-- 此查询语句执行之后返回的是一个结果集(多条记录),使用游标可以来遍历查询结果集
declare mycursor cursor for select book_name,book_author,book_price from books;
select count(1) into num from books;
-- 打开游标
open mycursor;
-- 使用游标要结合循环语句
set i = 0;
while i < num do 
-- 使用游标:提取游标当前指向的记录(提取之后,游标自动下移)
fetch mycursor into bname,bauthor,bprice;
set i = i+1;
select concat_ws('~',bname,bauthor,bprice) into str;
set result = CONCAT_WS('~',result,str);
end while;
-- 关闭游标
close mycursor;
end;

set @r = '';
call test01(@r);
select @r from dual;
```

# 十五.触发器

## 15.1触发器的介绍

触发器,就是一种特殊的存储过程,触发器和存储过程一样是一个能够完成特定功能,存储在数据库服务器上SQL片段,但是触发器无需调用,当对数据表中的数据执行DML操作时自动触发这个SQL片段的执行,无需手动调用.

> 在MySQL只有执行insert\delete\update操作才能触发触发器的执行.

## 15.2触发器的使用

15.2.1**案例说明**

```sql
-- 创建学生信息表
CREATE table students(
stu_num char(8) PRIMARY key,
stu_name varchar(20) not NULL,
stu_gender char(2) not null,
stu_age int not null
);

-- 创建学生信息操作日志表
CREATE table stulogs(
id int PRIMARY key auto_increment,
time TIMESTAMP,
log_text VARCHAR(200)
);


-- 当向students表中添加学生信息时,同时要在 students表中添加一条操作日志
INSERT into students(stu_num,stu_name,stu_gender,stu_age) VALUES ('1004','陈平安','男',20);
-- 手动进行记录日志
INSERT into stulogs(time,log_text) VALUES (now(),'添加1004学生信息');
```

案列:当向学生信息表添加,删除,修改学生信息时,使用触发器自动进行日志记录

15.2.2创建触发器

语法

```sql
create trigger tri_name
before | after            				--定义触发时机
insert | delete | update			--定义DML类型
on table_name
for each row					 --声明为行级触发器(只要操作一条记录就触发触发器执行一次)
sql _statement					 --触发器操作
```

```sql
--创建触发器:当学生信息表发生添加操作时,则向日志信息表中记录一条日志
create trigger tri_test01
after INSERT 
on students
for each row
INSERT into stulogs(time,log_text) VALUES (now(),CONCAT_WS(new.stu_num,'添加','学生信息'));
```

## 15.2.3查看触发器

```sql
show triggers;
```

## 15.2.4测试触发器

- 我们创建的触发器实在students表发生insert操作时触发,我们只需要执行学生信息的添加操作

```sql
--测试1:添加一个学生信息,触发器执行一次
INSERT into students(stu_num,stu_name,stu_gender,stu_age) VALUES ('1005','崔东山','男',20);
--测试2:一条sql指令添加了2条学生信息,触发器执行2次
INSERT into students(stu_num,stu_name,stu_gender,stu_age) VALUES ('1006','宁姚','女',20),('1007','裴钱','女',17);
```

## 15.2.5删除触发器

```sql
drop trigger tri_test01;
```

## 15.3NEW与OLD

> 触发器用于监听对数据表中数据的insert，update，delete操作，在触发器中通常处理一些DML的关联操作；我们可以使用==NEW==和==OLD==关键字在触发器中获取触发这个触发器的DML操作d 数据
>
> - NEW ： 在触发器中用于获取insert操作添加的数据，update操作修改后的记录
> - OLD ： 在触发器中用于获取delete操作删除前的数据，update操作修改前的数据

15.3.1、NEW

- insert操作中 ： NEW表示添加的新纪录

```sql
create trigger tri_test01
after INSERT 
on students
for each row
INSERT into stulogs(time,log_text) VALUES (now(),CONCAT_WS(new.stu_num,'添加','学生信息'));
```

- update操作中 ：NEW表示修改后的数据

```sql
--创建触发器：在监听update操作的触发器中，可以使用NEW获取修改后的数据
create trigger tri_test02
after Update 
on students
for each row
INSERT into stulogs(time,log_text) VALUES (now(),CONCAT_WS(new.stu_num,'修改',NEW.stu_name));
```

10.5.2、OLD

- delete操作中：OLD表示删除的记录

```sql
create trigger tri_test03
after DELETE 
on students
for each row
INSERT into stulogs(time,log_text) VALUES (now(),CONCAT_WS(OLD.stu_num,'删除',OLD.stu_name,'学生'));
```

- update操作中：OLD表示修改前的记录

```sql
create trigger tri_test04
after Update 
on students
for each row
INSERT into stulogs(time,log_text) VALUES (now(),CONCAT('将学生姓名从【',OLD.stu_name,'】改为【',new.stu_name,'】'));
```

## 15.4触发器使用总结

### 15.4.1优点

- 触发器是自动执行的，当对触发器相关的表执行响应的DML操作时立即执行；
- 触发器可以实现表中的数据的级联操作（关联操作），有利于保证数据的完整性；
- 触发器可以对DML操作的数据进行更为复杂的合法性效验

### 15.4.2缺点

- 使用触发器实现的业务逻辑如果出现问题将难以定位，后期维护困难；
- 大量使用触发器容易导致代码结构杂乱，增加程序的复杂性；
- 当触发器操作的数据量比较大时，执行效率会大大降低。

### 15.4.3使用建议

- 在互联网项目中，应避免使用触发器
- 对于并发量不大的项目可以选择使用存储过程，但是在互联网引用中不提倡使用存储过程（原因：存储过程时将实现业务的逻辑交给数据库处理，一则增建了数据库的负载，二则不利于数据库的迁移）

# 十六、视图

## 16.1、视图的概念

视图，就是 ==由数据库中一张表或者多张表根据特定的条件查询出的数据构成的== 虚拟表

## 16.2、视图的作用

- 安全性：如果我们直接将数据表授权给用户操作，那么用户可以CRUD数据表中所有数据，假如我们想要对数据表中的部分数据进行保护，可以将公开的数据生成视图，授权用户访问视图，用户通过查询视图可以获取数据表中公开的数据，从而达到将数据表中的部分数据对用户隐藏。

- 简单性：如果我们需要查询的数据来源于多张数据表，可以使用多表查询来实现；我们通过视图将这些连表查询的结果对用户开放，用户则可以直接通过查询视图获取多表数据，操作更便捷。

  
  
  ## 16.3、创建视图



16.3.1、语法

```sql
create view <view_name>
AS
select_statement;
```

16.3.2、实例

- 实例1：

```sql
--创建试图实例1：将学生表中性别为男的学生生成一个视图
create view view_test01
AS
select * from student where stu_gender = '男';

--查询视图
select * from view_test01;
```

- 实例2：

```sql
create view view_test02
AS
select s.stu_name,b.book_name,borrow_num from books b inner join records r inner join students s on b.book_id = r.bid and r.snum = s.stu_num;

--查询视图
select * from view_test02;
```

## 16.4、视图数据的特性

> 视图是虚拟表，查询视图的数据是来源与数据表的，当对视图进行操作时，对数据表中的数据是否有影响呢？

**查询操作：**如果在数据表中添加了新的数据，而且这个数据满足创建视图时查询语句的条件，通过查询视图也可以查询出新增的数据；当删除原表中满足查询条件的数据时，也会从视图中删除。

**新增数据：**如果在视图中添加数据，数据会被添加到原数据表

**删除数据：**如果从视图删除数据，数据也将从原表中删除

**修改数据：**如果通过修改数据，则也将修改原数据表中的数据

==视图的使用建议==：对复杂查询简化操作，并且不会对数据进行修改的情况下可以使用视图。

## 16.5、查询视图结构

```sql
-- 查询视图结构
desc view_test02;
```

## 16.6、修改视图

```sql
--方式1
create or replace view view_test01
as
select * from students where stu_gender = '女';
--方式2
alter view view_test01
as
select * from students where stu_gender = '男';
```

## 16.7、删除视图

- 删除数据表时会同时删除数据表中的数据，删除视图时不会影响原数据表中的数据

```sql
--删除视图
drop view view_test01;
```

# 十七、索引

> 数据库是用来存储数据，在互联网应用中数据库中存储的数据可能会很多（大数据），==数据表中数据的查询速度会随着数据量的增长逐渐变慢==，从而导致响应用户请求的速度变慢--用户体验差，我们如何提高数据库的查询效率呢？

## 17.1、索引的介绍

索引，就是用来提高数据表中数据的查询效率的

> 索引，就是将数据表中某一列/某几列的值取出来构造成便于查找的结构进行存储，生成数据表的==目录==当我们进行数据查询的时候，则先在==目录==中进行查找得到对应的数据的地址，然后在到数据表中根据地址快速的获取数据记录，避免全表扫描。

## 17.2、索引的分类

MySQL中的索引，根据创建索引的列的不同，可以分为：

- 主键索引：在数据表的主键字段创建的索引，这个字段必须被primary  key修饰，每张表只能有一个主键
- 唯一索引：在数据表中的唯一列创建的索引（unique），此列的所有值只能出现一次，可以为null
- 普通索引：在普通字段上创建的索引，没有唯一性的限制
- 组合索引：两个及以上字段联合起来创建的索引

==说明：==

1. 在创建数据表时，将字段说明为主键（添加主键约束），会自动在主键字段创建主键索引；
2. 在创建数据表时，将字段声明为唯一键（添加唯一约束），会自动在唯一字段创建唯一索引；

## 17.3、创建索引

### 17.3.1、唯一索引

```sql
--创建唯一索引；创建唯一索引的列的值不能重复
--create unique index <index_name> on 表名（列名）
create unique index index_test01 on tb_testindex(tid);
```

### 17.3.2、普通索引

```sql
--创建普通索引：不要求创建索引的列的值的唯一性
--create index <index_name> on 表名（列名）
create  index index_test02 on tb_testindex(tid);
```

### 17.3.3、组合索引

```sql
--创建组合索引
--create index <index_name> on 表名（列名1，列名2....）
create index <index_name> on 表名 (tid,name);
```

### 17.3.4、全文索引

> MySQL5.6版本新增的索引，可以通过此索引进行全文索引检查，因为MySQL全文索引不支持中文，因此这个全文索引不被开发者关注，在应用开发中通常是通过搜索引擎（数据库中间件）实现全文索引

```SQL
create fulltext index <index_name> on 表名（字段名）；
```

## 17.4、索引的使用

> 索引创建完之后无需调用，当根据创建索引的列进行数据查询的时候，会自动使用索引；
>
> 组合索引需要根据创建索引的所有字段进行查询时触发。

- 在命令行窗口中可以查看查询语句的查询规划

```sql
--命令行窗口要加斜杠G   /G；
explain select * from tb_testindex where tid=250000;
```

## 17.5、查看索引

```sql
--命令行
show create table tb_testindex/G;
--查询数据表的索引
show indexs from tb_testindex;
--查询索引
show keys from tb_testindex; 
```

## 17.6、删除索引

```sql
--删除索引：索引是建立在表的字段上的，不同的表可能会出现相同名称的索引
--		      因此删除索引时需要指定表名
drop index index_test01 on tb_testindex;
```

## 17.7、索引的使用总结

17.7.1、优点

- 索引大大降低了数据库服务器在执行查询操作时扫描的数据，提高查询效率
- 索引可以避免服务器排序、将随机IO编程顺序IO

17.7.2、缺点

- 索引是根据数据表列的值创建的，当数据表中数据发生DML操作时，索引页需要更新；
- 索引文件也会占用磁盘空间

17.7.3、注意事项

- 数据表中数据不多时，全表扫描可以更快，不要使用索引；
- 数据量大但是DML操作很频繁时，不建议使用索引；
- 不要在数据重复度高的列上创建索引（性别）；
- 创建索引之后，要注意查询SQL语句的编写，避免索引失效。

# 十八、数据库事务

## 18.1、数据库事务介绍

- 我们把完成特定的业务的多个数据库DML操作步骤称之为一个事务
- 事务：就是完成同一个业务的多个DML操作

```sql
--借书业务
--操作1：在借书记录中添加记录
insert into records(snum,bid,borrow_num,is_return,borrow_date)value('1001',1,1,0,sysdate());
--操作2：修改图书库存
update books set book_stock = book_stock-1 where book_id = 1;
```

## 18.2、数据库事务特性

> ACID特性，高频面试题

**原子性**（Atomicity）:一个事务中的多个DML操作，要么同时执行成功，要么同时执行失败

**一致性**（Consistency）：事务执行之前和执行之后，数据库中的数据是一致的，完成性和一致性不能被破坏

**隔离性**（Isolation）：数据库允许多个事务同时执行（张三借Java书的同时，允许李四借Java书），多个并行的事务之间不能相互影响

**持久性**（Durability）：事务完成之后，对数据的操作是永久的

## 18.3、MySQL事务管理

### 18.3.1、自动提交

- 在MySQL中，默认DML指令的执行是自动提交的，当我们执行一个DML指令之后，自动同步到数据库中

### 18.3.2、事务管理

> 开启事务，就是关闭自动提交

- 在开始事务第一个操作之前，执行start transaction开启事务
- 依次执行事务中的每个DML操作
- 如果在执行的过程中的任何位置出现异常，则执行rollback回滚事务
- 如果事务中所有的DML操作都执行成功，则在最后执行commit提交事务

```sql
--借书业务

--【开启事务】（关闭自动提交--手动提交）
start transaction；

--操作1：在借书记录中添加记录
insert into records（snum,bid,borrow_num,is_return,borrow_date）values('1007',4,2,0,sysdate())；

--select aaa;
--【事务回滚】（清楚连接缓存中的操作，撤销当前事务已经执行的操作）
--rollback；

--操作2：修改图书库存
update books set book_stock = book_stock-2 where book_id = 4 ;

--【提交事务】 （将连接缓存中的操作写入数据文件）
commit;
```

## 18.4、事务隔离级别

> 数据库允许多个事务并行，多个事务之间是隔离的，相互独立的；如果事物之间不相互隔离并且操作同一数据时，可能会导致数据的一致性被破坏。

MySQL数据库事务隔离级别：

### **读未提交**(read uncommitted)

T2可以读取T1执行但未提交的数据，可能会导致出现脏读

> 脏读，一个事务读取到了另一个事务中未提交的数据

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\脏读.png)

### **读已提交**（read committed）

T2 只能读取T1已经提交的数据；可能会导致不可重复读（虚读）

> 不可重复读（虚读）：在同一个事务中，两次查询操作读取到数据不一致
>
> 例如之前，T1修改并提交了数据，T2进行第二次查询时读取到的数据和第一次查询读取到的数据不一致。

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\虚读.png)

### **可重复读**（repeatable read）

T2执行第一次查询之后，在事务结束之前其他事务不能修改对应的数据，避免了不可 重复读（虚读），但可能会导致幻读

> 幻读，T2对数据表中的数据进行修改然后查询，在查询之前T1向数据表中新增了一条数据，就导致T2以为修改了所有数据，但却查询出了与修改不一致的数据（T1事务新增的数据）

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\虚读.png)

### **串行化**（serializable）

同时只允许一个事务对数据表进行操作；避免了脏读，虚读，幻读等问题

| 隔离级别         | 脏读 | 不可重复读（虚读） | 幻读 |
| ---------------- | ---- | ------------------ | ---- |
| read uncommitted | √    | √                  | √    |
| read committed   | X    | √                  | √    |
| repeatable read  | X    | X                  | √    |
| serializable     | X    | X                  | X    |

### 设置数据库事务隔离级别

> 我们可以通过设置数据库默认的事务隔离级别来控制事务之间的隔离性
>
> 也可以通过客户端与数据库连接设置来设置事务间的隔离性（在应用程序中设置-Spring）；
>
> MySQL数据库默认的隔离级别为==可重复读==

- 查看MySQL数据库默认的隔离级别

```sql
--在MySQL 8.0.3之前
select @@tx_isolation;

--在MySQL 8.0.3之后
select @@transaction_isolation;

```

- 设置MySQL默认隔离级别

```sql
set session transaction isolation level <read committed>;
```

# 十九、数据库设计

> MySQL数据库作为数据存储的介质为应用系统提供数据存储的服务，我们如何设计出合理的数据库、数据表以满足应用系统的数据存储需求呢？
>

- 车库：是用来存放车辆的，车库都需要划分车位，车子杂乱无章的存放可能会导致车辆堵塞，同时也可能造成场地的浪费--有限的场地能够停放最多的车辆，同时方便每一辆车的出入
- 数据库，是用来存放数据的，我们需要设计合理的数据表--能够完成数据的存储，同时能够方便的提取应用系统所需的数据

## 19.1、数据库设计流程

> 数据库是为应用系统服务的，数据库存储什么样的数据也是由应用系统来决定的。
>
> 当我们进行应用系统开发时，我们首先要明确应用系统的功能需求--软件系统的需求分析

1.**根据应用系统的功能、分析数据实体**（实体，就是要存储的数据对象）

​		电商系统：商品、用户、订单....

​		教务管理系统：学生、课程、成绩.....

 2.提**供实体的数据项**（数据项，就是实体的属性）

​		商品（商品名称、商品图片、商品描述...）

​		用户（姓名、登录名、登陆密码...）

3.**根据数据库设计三泛式规范视图的数据项**  检查实体的数据项是否满足数据库设计三范式

​		==如果实体的数据项不满足三范式，可能会导致数据的沉余，从而引起数据维护困难、破坏数据一致性等问题==

4.**绘制E-R图**（实体关系图，直观的展示实体与实体之间的关系）

5.**数据库建模**

- 三线图进行数据表设计
- PowerDseigner
- PDMan

6.**建库建表**   编写SQL指令创建数据库、数据表

7.**添加测试数据、SQL测试**

## 19.2、数据库设计案例

> 学校图书馆图书管理系统（借书）

### 19.2.1、数据实体

- 学生
- 类别
- 图书
- 借书记录
- 管理员

### 19.2.2、提取数据项

- 学生（学号、姓名、性别、年龄、院系编号、院校名称、院系说明...）
- 类别（类别ID、类别名称、类别描述）
- 图书（图书ID、图书名称、图书作者、图书封面、图书价格、图书库存...）
- 借书记录（记录ID、学号、图书编号、数量、是否归还、借书日期、还书日期）
- 管理员（管理员ID、登录名、登陆密码、员工编号、员工姓名、员工联系方式 、手机、qq、邮箱）

### 19.2.3、数据库设计三范式

==第一范式==：要求数据表中的字段（列）不可再分

以下表不满足第一范式（在数据库中创建不出不满足第一范式的表）

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\不正确的第一范式.png)

将细分的列作为单独的一列

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\第一范式.png)

==第二范式==：不存在非关键字段对关键字段的部份依赖

以下表不满足第二范式

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\不正确的第二范式.png)

将每个关键字段列出来\关键字段的组合也列出来，依次检查每个非关键字段

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\第二范式.png)

==第三范式==：不存在非关键字段之间的传递依赖

以下数据表不满足第三范式

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\不正确的第三范式.png)

将关键字段和被依赖的非关键字段分别作为主键，依次检查所有的非关键字段的依赖关系

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\第三范式.png)

### 19.2.4绘制E-R图

> E-R 实体关系图，用于直观的体现实体与实体之间的关联关系（一对一，一对多，多对多）

E-R图基本图例

<img src="C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\E-R图.png" style="zoom:200%;" />

### 19.2.5、数据库建模（PD）

> E-R图实际上就是数据建模的一部分；
>
> - E-R图 数据表设计 建库建表
> - power Designer建模工具 导出数据表
> - PDMan建模工具

power designer使用

- 概念数据模型（选择workspace--右键--Conceptual Data Model），相当于E-R

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\概念数据模型.png)

- 逻辑数据模型（打开概念数据模型--tools--Generate Logical Data Model），体现了实体的主外键关联

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\逻辑关联.png)

- 物理数据模型（打开逻辑数据模型--tools--Generate Physical Data Mode---选择数据库类型及版本）
  - 可以对物理数据模型进行微调
  - 可以通过物理数据模型生成建库建表的SQL语句（在物理数据模型的窗口上--Database工具条--Generate Database--生成SQL文件）
  - 通过数据库的管理工具执行SQL文件就可以完成数据表的创建

![](C:\Users\强上女闺蜜\Pictures\Camera Roll\MySQL\物理数据模型.png)

- 面向对象模型（打开概念数据模型/逻辑数据模型/物理数据模型----tools---Generate  Object-Orentited Model）

  - 可以根据语言设置，生成实体类
  - 如果想要借助PD建模工具生成Java代码，创建概念的模型时实体名、属性名要符合java程序的命名规范。

  > 在企业开发中，我们通常是不会使用建模工具来生成数据表、实体类的、因为生成的代码规范不合乎我们的代码需求
