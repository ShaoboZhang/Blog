# 一、引言

## 1.1 现有的数据存储方式

> **内存存储**

- 程序运行产生的数据(变量、数组、集合、对象)，保存在内存中

> **文件存储**

- 通过文件存储，保存在硬盘中

## 1.2 以上存储方式存在的缺陷

> **内存存储**

- 没有备份、恢复机制，随着程序运行结束数据会丢失；
- 不适合存储大量级数据

> **文件存储**

- 没有数据类型区分
- 没有访问安全限制

------

# 二、数据库

## 2.1 概念

> ​		数据库是按照数据结构来**组织、存储、管理数据的仓库**。是一个长期存储在计算机内有组织、有共享、统一管理的数据集合。

## 2.2 分类

> **网状结构数据库**

- IDS(Integrated Data Source, GE公司)，以节点形式存储和访问。

> **层次结构数据库**

- IMS(Information Management System, IBM公司)，以定向有序的树状结构存储和访问。

> **关系结构数据库**

- MySQL、SQL Server、Oracle、DB2，以表格形式存储，多表间建立关联关系，通过分类、合并、连接、选取等运输实现访问。

> **非关系型数据库**

- MongoDB、Redis、ElastecSearch，多数使用哈希表，构建特定的键(key)和一个指向特定数据的指针(value)形成键值对以供使用。



------

# 三、数据库管理系统

## 3.1 概念

> **数据库管理系统 (Database Management System, DBMS)**

- 指一种操作、管理数据库的大型软件用于建立、使用、维护数据库。
- 对数据库进行统一地管理和控制，以保证数据库的安全性和完整性。
- 用户通过DBMS访问数据库中的数据。

## 3.2 常见的DBMS

> **Oracle**

- 业界比较成功的关系型DBMS，可以运行在各主流操作系统平台，完全支持所有的工业标准，并获得最高级别的ISO标准安全性认证。

> **DB2**

- IBM公司的产品，采用多进程、多线索体系结构，其功能足以满足大中型企业需要，并可灵活地服务于中小型电子商务解决方案。

> **SQL Server**

- Microsoft公司推出的关系型DBMS，具有使用方便、可伸缩性好、与相关软件集成度高等优点。

> **SQLLite**

- 应用在手机端的数据库。



------

# 四、MySQL安装与配置

## 4.1 简介

> **MySQL**

- 关系型DBMS(Relational Database Management System, RDBMS)

- 由瑞典MySQL AB公司开发，后被Oracle收购。
- 是最流行的RDBMS之一。
- 在web应用方面，也是最好的RDBMS应用软件之一。

## 4.2 访问与下载

> **官方网站**

https://www.mysql.com/

> **下载地址**

https://dev.mysql.com/downloads/mysql/

## 4.3 安装

> 从官网下载安装包后进行安装，不断点击下一步直至安装完成。
>
> 在安装过程中需要输入登录数据库的密码。
>
> 在“系统偏好设置”中看到MySQL图标，表明安装成功。

![image-20211203102942521](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203102942521.png)

## 4.4 环境配置

### 4.4.1 在终端加入环境路径

> **创建`.zshrc`文件**

- `$cd ~/`

- `$touch zshrc`

> **添加MySQL环境路径**

- 打开文件 `$open ~/.zshrc`

- 添加 `PATH=$PATH:/usr/local/mysql/bin`

> **激活`.zshrc`文件**

- `$source .zshrc`

### 4.4.2 登录

> **登录指令**

`$mysql -uroot -p`

> **输入密码**

输入安装过程中设置的密码

> **登录成功**

出现如下界面则说明登录成功

![image-20211203103909165](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203103909165.png)

## 4.5 MySQL服务指令

> **启动服务**

​	`$sudo /usr/local/mysql/support-files/mysql.server start`

> **停止服务**

​	`$sudo /usr/local/mysql/support-files/mysql.server stop`

> **重启服务**

​	`$sudo /usr/local/mysql/support-files/mysql.server restart`

## 4.6 MySQL目录结构

> **核心文件介绍**

| **文件夹名称** |      **内容**      |
| :------------: | :----------------: |
|      bin       |      命令文件      |
|      lib       |       库文件       |
|    include     |       头文件       |
|     share      | 字符集、语言等信息 |

## 4.7 MySQL配置文件

> **核心配置参数**

|          参数          |         描述         |
| :--------------------: | :------------------: |
| default-character-set  |  客户端默认字符编码  |
|  character-set-server  |  服务端默认字符编码  |
|          port          | 客户端和服务端端口号 |
| default-storage-engine | 默认存储引擎(InnoDB) |



------

# 五、SQL语言

## 5.1 概念

> **SQL (Structured Query Language)**

结构化查询语言，用于存取数据、更新、查询和管理RDBMS的程序设计语言。

**说明：通常执行对数据库的“增删改查”，简称"CRUD"，即CREATE、Read、Update、Delete。**

## 5.2 MySQL应用

对于数据库的操作，需要进入MySQL环境下进行指令输入，并在一句指令的末尾以分号结束。

## 5.3 客户端 (Navicat)

> **下载地址**

https://www.macwk.com/soft/navicat-premium/

> **新建连接**

![image-20211203183842532](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203183842532.png)

> **数据库登录**

![image-20211203183759031](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203183759031.png)



------

# 六、数据库操作

## 6.1 数据库查看 (SHOW)

> **查看所有数据库**

``` mysql
SHOW DATABASES;
```

|     数据库名称     |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
| information_schema | 信息数据库，保存着关于所有数据库的信息(元数据)。<br/>元数据是关于数据的数据，如数据库名、表名、列的数据类型、访问权限等。 |
|       mysql        | 核心数据库，主要负责存储数据库等用户、权限设置、关键字等，<br/>以及需要使用的控制和管理信息，不可以删除。 |
| performance_schema |  性能优化的数据库，MySQL5.5版本后新增的一个性能优化的引擎。  |
|        sys         | 系统数据库，MySQL5.7版本后新增的可以快速了解元数据信息的系统库。<br/>便于发现数据库的多样信息，解决性能瓶颈问题。 |

> **查看特定数据库信息**

```mysql
SHOW CREATE DATABASE mydb1;
```

## 6.2 数据库创建 (CREATE)

> **基础创建方式**

```mysql
# 创建一个名称为mydb1的数据库
CREATE DATABASE mydb1;
```

> **创建时指定字符编码**

```mysql
# 创建一个名称为mydb1、字符编码为utf-8的数据库
CREATE DATABASE mydb1 CHARACTER SET utf8;
```

> **仅当原文件不存在时创建**

```mysql
# 若名称为mydb1的数据库不存在，则创建一个名称为mydb1、字符编码为utf-8的数据库
CREATE DATABASE IF NOT EXISTS mydb1 CHARACTER SET utf8;
```

## 6.3 数据库修改 (ALTER)

```mysql
# 更改数据库的字符编码为gbk
ALTER DATABASE mydb1 CHARACTER SET gbk;
```

## 6.4 数据库删除 (DROP)

```mysql
DROP DATABASE mydb1;
```

## 6.5 数据库使用 (USE)

```mysql
USE mydb1;
```

## 6.6 当前数据库查看 (**SELECT**)

```mysql
SELECT DATABASE();
```



------

# 七、数据表基础知识

> 一个数据库里可包含多个**数据表(table)**，表中的一行数据代表一个**实体(entity)**，表中的一列数据代表实体的一个**属性(attribute)**。

**说明：基于数据表、实体和属性的意义**

- **参照Java程序中的类(class)，通常用首字母大写、其余字母小写的方式为数据表命名。**
- **参照Java程序中的字段(field)，通常采用驼峰命名法为属性命名。**
- **在创建数据表时，需要指定属性的名称、数据类型和约束条件。**

## 7.1 数据类型

> MySQL支持多种数据类型，主要包括三类：数值、日期/时间、字符串。

### 7.1.1 数值类型

|     类型     |              大小(字节)               |                         范围(有符号)                         | 范围(无符号) |     用途     |
| :----------: | :-----------------------------------: | :----------------------------------------------------------: | :----------: | :----------: |
|     INT      |                 4字节                 |                      [-2^31^, 2^31^-1]                       |  [0,2^64-1]  |     整数     |
|    DOUBLE    |                 8字节                 |                     [-2^1024^, 2^1024^]                      |              | 双精度浮点数 |
| DOUBLE(M,D)  | 8字节<br/>M表示长度<br/>D表示小数位数 | 依赖于M和D的值<br/>例如：DOUBLE(5,2)<br/>范围：[-999.99,999.99] |              | 双精度浮点数 |
| DECIMAL(M,D) | 8字节<br/>M表示长度<br/>D表示小数位数 | 依赖于M和D的值，但M的上限为65<br/>例如：DOUBLE(5,2)<br/>范围：[-999.99,999.99] |              |     小数     |

### 7.1.2 日期类型

|   类型    | 大小(字节) |                   范围                    |         格式         |            用途             |
| :-------: | :--------: | :---------------------------------------: | :------------------: | :-------------------------: |
|   DATE    |     3      |          [1000-01-01,9999-12-31]          |      YYYY-MM-DD      |            日期             |
|   TIME    |     3      |          [-838:59:59,838:59:59]           |       HH:MM:SS       |       时间或持续时间        |
|   YEAR    |     1      |                [1901,2155]                |         YYYY         |            年份             |
| DATETIME  |     8      | [1000-01-01 00:00:00,9999-12-31 23:59:59] | YYYY-MM-DD  HH:MM:SS |         日期和时间          |
| TIMESTAMP |     4      |                [0,2^31^-1]                |         INT          | 距1970-01-01 00:00:00的秒数 |

### 7.1.3 字符串类型

|            类型            | 大小(字节) |       用途       |
| :------------------------: | :--------: | :--------------: |
|            CHAR            |  0~2^8^-1  |    定长字符串    |
|          VARCHAR           | 0~2^16^-1  |    变长字符串    |
| BLOB (binary large object) | 0~2^16^-1  | 二进制形式长文本 |
|            TEXT            | 0~2^16^-1  |    长文本数据    |

> **CHAR和VARCHAR的区别**

- 保存和检索的方式不同
- 最大长度和尾部是否留空不同
- 存储和检索过程中均不进行大小写转换

> **BLOB详细介绍**

- 二进制大对象，可容纳可变长度的数据
- 具体有四种BLOB类型：TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB，对应不同的可容纳长度

## 7.2 约束条件

### 7.2.1 实体完整性约束

实体完整性的作用是标识每个实体的唯一性。

> **主键约束 (PRIMARY KEY)**

标识表中每个实体的该属性需要互不相同、且不能为空。

> **唯一约束 (UNIQUE)**

标识表中每个实体的该属性需要互不相同，但允许为空。

> **自动增长 (AUTO_INCREMENT)**

可以给主键数值列添加该标签，从1开始，每次加1。

该标签不能单独使用，必须和有主键的数值列配合使用。

### 7.2.2 域完整性约束

域完整性的作用是约束实体某个属性的正确性。

> **非空约束 (NOT NULL)**

标识表中每个实体的该属性需要不为空。

> **默认值约束 (DEFAULT)**

为属性赋予默认值，当新增数据不指定该属性值而是指定为“DEFAULT”时，用设置的默认值替代。

> **引用完整性约束 (FOREIGN KEY)**

- 语法：`CONSTRAINT fk_从表名_列名 FOREIGN KEY(引用列名) REFRENCES 主表名(被引用列名)`
- 意义：FOREIGN KEY引用外部表的某一列，新增数据时，约束此属性值必须为外部表被引用的列中存在的值。
- 注意：若两张表存在引用关系时
  - **在创建从表(引用表)之前，必须先创建主表(被引用表)**。
  - **在删除主表(被引用表)之前，必须先删除从表(引用表)。**



------

# 八、数据表操作

> 一个数据库里可包含多个数据表，对数据表可以进行创建、修改、删除的操作。

## 8.1 数据表创建 (CREATE)

> **语法**

```sql
CREATE TABLE 表名 (
    列名 数据类型 [约束],
    列名 数据类型 [约束],
    ...
    列名 数据类型 [约束]	// 最后一列的末尾不允许加逗号
) [CHARSET=utf8];		 // 可根据需要指定表的某些属性
```

> **创建Class数据表**

|   列名    |  数据类型   |      约束      |   说明   |
| :-------: | :---------: | :------------: | :------: |
|  classId  |     INT     | 主键、自动增长 | 班级编号 |
| className | VARCHAR(20) |   唯一、非空   | 班级名称 |

```sql
CREATE TABLE Class (
	classId INT PRIMARY KEY AUTO_INCREMENT,
	className VARCHAR(20) UNIQUE NOT NULL
)CHARSET=UTF8;
```

> **创建Student数据表**

|    列名     |  数据类型   |                 约束                  | 说明 |
| :---------: | :---------: | :-----------------------------------: | :--: |
|  studentId  |     INT     |            主键、自动增长             | 学号 |
| studentName | VARCHAR(20) |                 非空                  | 姓名 |
|     sex     |   CHAR(2)   |           非空、默认为“男”            | 性别 |
|  bornDate   |    DATE     |                 非空                  | 生日 |
|    phone    | VARCHAR(11) |                  无                   | 电话 |
|   classId   |     INT     | 非空、外键约束(引用Class表的class_id) | 班级 |

```sql
CREATE TABLE Student (
	studentId INT PRIMARY KEY AUTO_INCREMENT,
	studentName VARCHAR(20) NOT NULL,
	sex CHAR(1) NOT NULL DEFAULT '男',
	bornDate DATE NOT NULL,
	phone VARCHAR(11),
	classId INT NOT NULL,
	CONSTRAINT fk_Student_classId FOREIGN KEY(classId) REFERENCES Class(classId)
)CHARSET=UTF8;
```

## 8.2 数据表修改 (ALTER)

> **向表中添加列 (ADD)**

```mysql
ALTER TABLE 表名 ADD 列名 数据类型 [约束]；
```

注意：

- **若表中没有数据，在添加列时加上任意约束都是可行的。**
- **若表中已有数据，在添加列时不能加上会与已有数据产生冲突的约束。**

> **修改表中的列 (MODIFY)**

```mysql
ALTER TABLE 表名 MODIFY 列名 数据类型 [约束];
```

注意：**修改表中某列时，要写全列的名称、数据类型和约束条件。**

> **删除表中的列 (DROP)**

```mysql
ALTER TABLE 表名 DROP 列名;
```

注意：**删除列时，每次只能删除一列。**

> **修改列名 (CHANGE)**

```mysql
ALTER TABLE 表名 CHANGE 列名 新列名 数据类型 [约束]。
```

> **修改表名 (RENAME)**

```mysql
ALTER TABLE 表名 RENAME 新表名;
```

## 8.3 数据表删除 (DROP)

```mysql
DROP TABLE 表名;
```

# 九、数据查询【重点】

## 9.1 基本结构

> - 关系型数据库中的数据以表格的形式进行数据存储，表格由行、和列组成。
> - 每一行代表一个实体
> - 每一列代表实体的一个属性

**注意：对数据表执行查询语句时，返回的结果是一张临时虚拟表**

## 9.2 基本查询

> **SELECT 列名 FROM 表名**

| 关键字 | 描述           |
| ------ | -------------- |
| SELECT | 指定要查询的列 |
| FROM   | 指定要查询的表 |

### 9.2.1 查询部分列

```mysql
SELECT 列名1,列名2,... FROM 表名;
```

### 9.2.2 查询所有列

```mysql
SELECT 所有列的列名 FROM 表名;
```

```mysql
SELECT * FROM 表名;
```

**注意：生产环境下，优先使用列名查询，因为使用"*"的方式需要转换，其效率低、可读性差。**

### 9.2.3 列的运算

| 算术运算符 | 描述           |
| ---------- | -------------- |
| +          | 两列做加法运算 |
| -          | 两列做减法运算 |
| *          | 两列做乘法运算 |
| /          | 两列做除法运算 |

**注意：**

- **在mysql中，%是占位符，而非取模运算符。**
- **在查询过程中进行的运算，不会影响原表格数据。**

### 9.2.4 列的别名

```mysql
# 从表中查询员工的编号、名字、年薪并修改别名
SELECT id AS "编号", name AS "名字", salary*12 AS "年薪" 
FROM t_employees;
```

### 9.2.5 结果去重

```mysql
# 查询员工表中所有经理的id
SELECT DISTINCT managerId FROM t_employees;
```

## 9.3 排序查询

> SELECT 列名 FROM 表名 **ORDER BY 列名 [规则]**

| 排序规则 | 描述符 |
| -------- | ------ |
| ASC      | 升序   |
| DESC     | 降序   |

### 9.3.1 单列排序

```mysql
# 查询员工ID、薪资，按工资升序排序
SELECT id, salary 
FROM t_employees 
ORDER BY salary ASC;
```

### 9.3.2 多列排序

```mysql
# 查询员工id、薪资，先按工资将序、再按id升序
SELECT id, salary 
FROM t_employees 
ORDER BY salary DESC, id ASC;
```

## 9.4 条件查询

> SELECT 列名 FROM 表名 **WHERE 条件**

| 关键字 | 描述                                 |
| ------ | ------------------------------------ |
| WHERE  | 在查询结果中，筛选符合条件的进行展示 |
| 条件   | 布尔表达式                           |

> **等值判断 (=)**

```mysql
SELECET * FROM t_employees 
WHERE id=100;
```

**注意：与java等语音不同，mysql中使用"="进行逻辑判断。**

> **不等值判断 (>, <, >=, <=, !=, <>)**

**注意：mysql中"!="和"<>"都表示不等于，两者具有相同的效果。**

> **区间查询 (BETEWEEN A AND B)**

```mysql
SELECT * FROM t_employees
WHERE salary BETWEEN 10000 AND 20000;
```

**注意：使用BETWEEN A AND B时，A的值必须小于B的值，否则返回为空。**

> **逻辑判断 (AND, OR, NOT)**

```mysql
SELECT * FROM t_employees
WHERE salary>10000 AND salary<20000;
```

**注意：其与BETWEEEN A AND B是有区别的，后者是一条逻辑，而前者是两条逻辑。**

>  **空值判断 (IS NULL, IS NOT NULL)**

```mysql
SELECT * FROM t_employees
WHERE managerId IS NULL;
```

> **枚举查询 (IN)**

```mysql
SELECT * FROM t_employees
WHERE managerId in (70,80,90);
```

**注意：相比于OR，枚举查询的效率较低，所以是否使用需根据实际情况而定。**

> **模糊查询 (LIKE)**

```mysql
# 查询表中姓张、且名字为三个字的人
SELECT * FROM t_employees
WHERE name LIKE "张__";

# 查询表中名字以“张”开头的人
SELECT * FROM t_employees
WHERE name LIKE "张%";
```

**注意："_"匹配任意单个字符，"%"匹配任意长度字符。**

> **分支查询 (CASE)**

```mysql
SELECT * FROM t_employees
CASE
	WHEN salary>=20000 THEN "A"
	WHEN salary>=10000 AND salary<20000 THEN "B"
	ELSE "C"
END AS "level"
FROM t_employees;
```

**注意：**

- "CASE"语句会产生一新列，默认列名为语句内部内容，所以通常在"END"之后为其设置别名。
- "ELSE"为可选项，视需求决定是否有该逻辑。

