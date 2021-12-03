# 一、引言

## 1.1 现有的数据存储方式

> - 内存存储
>   - 程序运行产生的数据(变量、数组、集合、对象)，保存在内存中
> - 文件存储
>   - 通过文件存储，保存在硬盘中

## 1.2 以上存储方式存在的缺陷

> - 内存存储
>   - 没有备份、恢复机制，随着程序运行结束数据会丢失
>   - 不适合存储大量级数据
> - 文件存储
>   - 没有数据类型区分
>   - 没有访问安全限制



------

# 二、数据库

## 2.1 概念

> ​		数据库是按照数据结构来**组织、存储、管理数据的仓库**。是一个长期存储在计算机内有组织、有共享、统一管理的数据集合。

## 2.2 分类

> - 网状结构数据库：IDS(Integrated Data Source, GE公司)，以节点形式存储和访问。
> - 层次结构数据库：IMS(Information Management System, IBM公司)，以定向有序的树状结构存储和访问。
> - 关系结构数据库：MySQL、SQL Server、Oracle、DB2，以表格形式存储，多表间建立关联关系，通过分类、合并、连接、选取等运输实现访问。
> - 非关系型数据库：MongoDB、Redis、ElastecSearch，多数使用哈希表，构建特定的键(key)和一个指向特定数据的指针(value)形成键值对以供使用。



------

# 三、数据库管理系统

## 3.1 概念

> **数据库管理系统 (Database Management System, DBMS)**：
>
> - 指一种操作、管理数据库的大型软件用于建立、使用、维护数据库。
> - 对数据库进行统一地管理和控制，以保证数据库的安全性和完整性。
> - 用户通过DBMS访问数据库中的数据。

## 3.2 常见的DBMS

> - Oracle：业界比较成功的关系型DBMS，可以运行在各主流操作系统平台，完全支持所有的工业标准，并获得最高级别的ISO标准安全性认证。
> - DB2：IBM公司的产品，采用多进程、多线索体系结构，其功能足以满足大中型企业需要，并可灵活地服务于中小型电子商务解决方案。
> - SQL Server：Microsoft公司推出的关系型DBMS，具有使用方便、可伸缩性好、与相关软件集成度高等优点。
> - SQLLite：应用在手机端的数据库。



------

# 四、MySQL

## 4.1 简介

> MySQL是一个关系型数据库管理系统(Relational Database Management System, RDBMS):
>
> - 由瑞典MySQL AB公司开发，后被Oracle收购。
> - 是最流行的RDBMS之一。
> - 在web应用方面，也是最好的RDBMS应用软件之一。

## 4.2 访问与下载

> 官方网站：https://www.mysql.com/
>
> 下载地址：https://dev.mysql.com/downloads/mysql/

## 4.3 安装

> 从官网下载安装包后进行安装，不断点击下一步直至安装完成。
>
> 在安装过程中需要输入登录数据库的密码。
>
> 在“系统偏好设置”中看到MySQL图标，表明安装成功。

![image-20211203102942521](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203102942521.png)

## 4.4 环境配置

### (1) 在终端加入环境路径

> 创建`.zshrc`文件

- `$cd ~/`

- `$touch zshrc`

> 添加MySQL环境路径

- 打开文件 `$open ~/.zshrc`

- 添加 `PATH=$PATH:/usr/local/mysql/bin`

> 激活`.zshrc`文件

- `$source .zshrc`

### (2) 登录

> 登录指令

`$mysql -uroot -p`

> 输入密码

输入安装过程中设置的密码

> 登录成功

出现如下界面则说明登录成功

![image-20211203103909165](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203103909165.png)

## 4.5 MySQL服务指令

> 启动服务

​	`$sudo /usr/local/mysql/support-files/mysql.server start`

> 停止服务

​	`$sudo /usr/local/mysql/support-files/mysql.server stop`

> 重启服务

​	`$sudo /usr/local/mysql/support-files/mysql.server restart`

## 4.6 MySQL目录结构

> 核心文件介绍

| **文件夹名称** |      **内容**      |
| :------------: | :----------------: |
|      bin       |      命令文件      |
|      lib       |       库文件       |
|    include     |       头文件       |
|     share      | 字符集、语言等信息 |

## 4.7 MySQL配置文件

> 核心配置参数

|          参数          |         描述         |
| :--------------------: | :------------------: |
| default-character-set  |  客户端默认字符编码  |
|  character-set-server  |  服务端默认字符编码  |
|          port          | 客户端和服务端端口号 |
| default-storage-engine | 默认存储引擎(InnoDB) |



------

# 五、SQL语言

## 5.1 概念

> SQL (Structured Query Language)：结构化查询语言，用于存取数据、更新、查询和管理RDBMS的程序设计语言。

- **经验：通常执行对数据库的“增删改查”，简称"CRUD"，即Create、Read、Update、Delete**

## 5.2 MySQL应用

> 对于数据库的操作，需要进入MySQL环境下进行指令输入，并在一句指令的末尾以分号结束。

## 5.3 基本命令

> 查看MySQL中所有的数据库

``` sh
mysql> show databases;
```

|     数据库名称     |                             描述                             |
| :----------------: | :----------------------------------------------------------: |
| information_schema | 信息数据库，保存着关于所有数据库的信息(元数据)。<br/>元数据是关于数据的数据，如数据库名、表名、列的数据类型、访问权限等。 |
|       mysql        | 核心数据库，主要负责存储数据库等用户、权限设置、关键字等，<br/>以及需要使用的控制和管理信息，不可以删除。 |
| performance_schema |  性能优化的数据库，MySQL5.5版本后新增的一个性能优化的引擎。  |
|        sys         | 系统数据库，MySQL5.7版本后新增的可以快速了解元数据信息的系统库。<br/>便于发现数据库的多样信息，解决性能瓶颈问题。 |

> 创建自定义数据库

```sh
# 创建一个名称为mydb1的数据库
mysql> create database mydb1;

# 创建一个名称为mydb1、字符编码为utf-8的数据库
mysql> create database mydb1 character set utf8;

# 若名称为mydb1的数据库不存在，则创建一个名称为mydb1、字符编码为utf-8的数据库
mysql> create database if not exists mydb1 character set utf8;
```

> 查看数据库信息

```sh
mysql> show create database mydb1;
```

> 修改数据库

```sh
# 更改数据库的字符编码为gbk
mysql> alter database mydb1 character set gbk;
```

> 删除数据库

```sh
mysql> drop database mydb1;
```

> 使用数据库

```sh
mysql> use mydb1;
```

> 查看当前使用的数据库

```shell
mysql> select database();
```



------

# 六、客户端 (Navicat)

> 下载地址

https://www.macwk.com/soft/navicat-premium/

> 新建MySQL连接

![image-20211203183842532](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203183842532.png)

> 登录MySQL

![image-20211203183759031](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203183759031.png)

> 出现以下界面，则登录成功

![image-20211203184333275](/Users/shaobo/Library/Application Support/typora-user-images/image-20211203184333275.png)



------

# 七、数据表操作

## 7.1 数据类型

> MySQL支持多种数据类型，主要包括三类：数值、日期/时间、字符串。

(1) 数值类型

|     类型     |              大小(字节)               |                         范围(有符号)                         | 范围(无符号) |     用途     |
| :----------: | :-----------------------------------: | :----------------------------------------------------------: | :----------: | :----------: |
|     INT      |                 4字节                 |                        [-2^31,2^31-1]                        |  [0,2^64-1]  |     整数     |
|    DOUBLE    |                 8字节                 |                       [-2^1024,2^1024]                       |              | 双精度浮点数 |
| DOUBLE(M,D)  | 8字节<br/>M表示长度<br/>D表示小数位数 | 依赖于M和D的值<br/>例如：DOUBLE(5,2)<br/>范围：[-999.99,999.99] |              | 双精度浮点数 |
| DECIMAL(M,D) | 8字节<br/>M表示长度<br/>D表示小数位数 | 依赖于M和D的值，但M的上限为65<br/>例如：DOUBLE(5,2)<br/>范围：[-999.99,999.99] |              |     小数     |

(2) 日期类型

|   类型    | 大小(字节) |                   范围                    |         格式         |            用途             |
| :-------: | :--------: | :---------------------------------------: | :------------------: | :-------------------------: |
|   DATE    |     3      |          [1000-01-01,9999-12-31]          |      YYYY-MM-DD      |            日期             |
|   TIME    |     3      |          [-838:59:59,838:59:59]           |       HH:MM:SS       |       时间或持续时间        |
|   YEAR    |     1      |                [1901,2155]                |         YYYY         |            年份             |
| DATETIME  |     8      | [1000-01-01 00:00:00,9999-12-31 23:59:59] | YYYY-MM-DD  HH:MM:SS |         日期和时间          |
| TIMESTAMP |     4      |                [0,2^31-1]                 |         INT          | 距1970-01-01 00:00:00的秒数 |

(3) 字符串类型

|            类型            | 大小(字节) |       用途       |
| :------------------------: | :--------: | :--------------: |
|            CHAR            |  0-2^8-1   |    定长字符串    |
|          VARCHAR           |  0-2^16-1  |    变长字符串    |
| BLOB (binary large object) |  0-2^16-1  | 二进制形式长文本 |
|            TEXT            |  0-2^16-1  |    长文本数据    |

- CHAR和VARCHAR
  - 保存和检索的方式不同
  - 最大长度和尾部是否留空不同
  - 存储和检索过程中均不进行大小写转换
- BLOB
  - 二进制大对象，可容纳可变长度的数据
  - 具体有四种BLOB类型：TINYBLOB、BLOB、MEDIUMBLOB、LONGBLOB，对应不同的可容纳长度
