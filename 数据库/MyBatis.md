# 	一、引言

## 1.1 什么是框架

[软件的半成品，解决工程的普适性问题]()，从而简化开发步骤，提升了开发效率。

## 1.2 什么是ORM框架

- ORM (Object Relational Mapping): 对象关系映射，将程序中的一个对象与表中的一行数据一一对应。
- ORM框架提供了持久化类与表的映射关系，在运行时参照映射文件的信息，把对象**持久化**到数据库中。

## 1.3 JDBC完成ORM的缺点

- 存在大量的冗余代码
- 需手动创建connection、statement等
- 需手动将查询结果封装成实体对象
- 查询效率低，没有对数据访问进行优化(缓存)

------

# 二、MyBatis介绍

## 2.1 概念

- MyBatis前身是Apache软件基金会的一个开源项目iBatis，现已被迁移至Github。
- MyBatis是一个优秀的基于Java的持久层框架，支持自定义SQL、存储过程和高级映射。
- MyBatis对JDBC操作进行了封装，几乎消除了所有JDBC代码，使开发者只需设计SQL语句本身。
- MyBatis可以使用xml文件或注解进行开发，并自动完成ORM操作，将执行结果返回。

## 2.2 官方地址

官方文档：https://mybatis.org/mybatis-3/zh/index.html

下载地址：https://github.com/mybatis/mybatis-3/releases

# 三、环境搭建