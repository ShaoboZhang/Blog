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

------

# 三、MyBatis开发步骤

> 项目地址：https://gitee.com/shaobo1103/study/tree/master/MyBatis/01-hello-mybatis

## 3.1 构建Maven项目

> 在Idea新建空白Maven项目

说明：

- 在项目创建完成后，可删除多余文件、只保留`.idea`文件夹和`pom.xml`，将该项目作为父工程使用。在父工程中构建子模块，用以学习和测试MyBatis不同功能，这样可以避免相同依赖需要重复导入。
- 在父工程的pom.xml中配置项目通用依赖，在子模块的pom.xml中添加独有依赖。
- 在子模块的资源目录下添加该模块的配置文件。

## 3.2 环境搭建

### 3.2.1 连接数据库

> 连接本地数据库，并创建数据库和数据表

```mysql
# 创建数据库
CREATE DATABASE IF NOT EXISTS my_test CHARACTER SET utf8;

# 使用刚创建的数据库
USE my_test;

# 创建数据表
CREATE TABLE Student (
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(10) NOT NULL,
    sex CHAR(1) NOT NULL,
    born_date DATE NOT NULL
)CHARSET=utf8;
```

### 3.2.2 添加项目依赖

> 使用Maven构建项目，将相关依赖添加到**父工程**的pom.xml中

```xml
<dependencies>
  <!--mybatis-->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
  </dependency>
  <!--mysql-connector-->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.27</version>
  </dependency>
  <!--junit-->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
  </dependency>
  <!--lombok-->
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.22</version>
  </dependency>
</dependencies>
```

### 3.2.3 创建数据库属性文件

> 在工程resouces目录下创建数据库的properties属性文件

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/my_test
username=root
password=12345678
```

### 3.2.4 创建Mybatis配置文件

> 在工程resouces目录下创建Mybatis的xml配置文件

```xml-dtd
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="db.properties"/>

    <environments default="main">
        <environment id="main">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

注意：基于约定，通常将Mybatis的配置文件命名为`mybatis-config.xml`。

## 3.3 定义实体类

> 在entity包中定义对应数据表的实体类

```java
package entity;

import lombok.Data;
import java.util.Date;

@Data
public class Student {
    private int id;
    private String name;
    private String sex;
    private Date bornDate;
}
```

注意：

- 在数据表中，日期类型为`sql.util.Date`，但MyBatis会自动将其转换为`java.util.Date`类型。
- 所以，在定义实体类时仍可以将日期类型声明为`java.util.Date`类型。

## 3.4 定义映射接口

> 在mapper包中定义获取数据的方法

```java
package mapper;

import entity.Student;

public interface StudentMapper {
    Student getStudentById(int id);
}
```

## 3.5 编写映射文件

> 在resouces文件夹下定义使用映射接口的方法

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!--文件头-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--映射配置-->
<!--namespace标明该映射对应程序接口-->
<mapper namespace="mapper.StudentMapper">
    <!--id标明程序接口对应的方法名-->
    <!--resultType标明方法返回类型-->
    <select id="getStudentById" resultType="entity.Student">
        <!--该方法执行的SQL语句-->
        SELECT id, name, sex, born_date as bornDate
        FROM student
        WHERE id = #{id};
    </select>
</mapper>
```

注意：

- 当执行方法需要传入参数时，使用`#{参数名}`的方式传入，例如#{id}。

- 由于在数据表中，出生日期使用的字段是`born_date`，而实体类中字段是`bornDate`。
- 因此，在选择字段时不能使用`SELECT *`，需要使用`SELECT id, name, sex, born_date as bornDate`。

## 3.6 注册映射文件

> 在Mybatis配置文件中注册映射文件

```xml-dtd
<mappers>
		<mapper resource="mappers/student-mapper.xml"/>
</mappers>
```

## 3.7 获取SqlSession对象

> 使用工厂模式，从MyBatis配置文件中构建SqlSessionFactory，进而获取SqlSession对象

```java
package util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MyBatisUtil {
    // SQL会话工厂对象
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            // 将MyBatis配置文件读取到输入流
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            // 从输入流构建SQL会话工厂
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
            // 关闭输入流
            is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 提供获取SQL会话对象的公共方法
    public static SqlSession getSqlSession() {
        // 通过SqlSessionFactory获取SqlSession对象
        return sqlSessionFactory.openSession();
    }
}
```

## 3.8 执行接口方法

> 编写测试文件，测试接口方法

```java
import entity.Student;
import mapper.StudentMapper;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import util.MyBatisUtil;

public class Main {
    @Test
    public void test01() {
        SqlSession sqlSession = MyBatisUtil.getSqlSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.getById(1);
        System.out.println(student); // Student(id=1, name=张三, sex=男, bornDate=Tue Feb 15 00:00:00 CST 2000)
	      sqlSession.close();
    }
}
```

注意：在程序结束前关闭SqlSession。

------

# 四、MyBatis配置文件

> MyBatis的核心配置包括：properties, settings, typeAliases, environments, mappers

[**注意：MyBatis对配置的定义顺序有严格的要求，必须按上述顺序进行定义。**]()

### 4.1 属性 (properties)

> 通过属性设置，便于在整个MyBatis配置文件中替换需要动态设置的属性值

```properties
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>
```

- 属性值既通过`resource`指定属性文件路径获得，也可以在`properties`的子元素中设置。
- 但出于利于统一维护、管理的考虑，最好都通过文件的形式进行设置。

### 4.2 设置 (settings)

> MyBatis中极为重要的调整设置，它们会改变MyBatis的运行时行为

| 名称                     | 描述                                             | 有效值                                             | 默认值 |
| ------------------------ | ------------------------------------------------ | -------------------------------------------------- | ------ |
| cacheEnabled             | 全局性地开启或关闭所有映射文件中已配置的任何缓存 | true\|false                                        | true   |
| defaultStatementTimeout  | 超时时间，决定等待数据库相应的描述               | 任意正整数                                         | null   |
| mapUnderscoreToCamelCase | 是否开启驼峰命名自动映射                         | true\|false                                        | false  |
| logImpl                  | 指定MyBatis所用日志的具体实现                    | SLF4J<br/>LOG4J2<br/>STDOUT_LOGGING<br/>NO_LOGGING | 未设置 |
| logPrefix                | 指定MyBatis增加到日志名称的前缀                  | 任意字符串                                         | 未设置 |



### 4.3 类型别名 (typeAliases)

> 可为Java类的全限定名设置一个别名，意在降低冗余的全限定类名书写

#### (1) 通过子元素配置

```properties
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
</typeAliases>
```

#### 4.3.2 通过包名配置

```properties
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

#### (3) 通过注解配置

```java
@Alias("author")
public class Author {
  ...
}
```

说明：

- 通过包名配置时，MyBatis会在包名下搜索需要的Java Bean。
- 在没有注解的情况下，会使用首字母小写的名称作为其别名。
- 若有注解，则别名为其注解值。

### 4.4 环境 (environments)

> 该元素定义了如何配置数据库连接环境

#### (1) environment子元素

```java
// 使用指定环境配置
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);

// 使用默认环境配置
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
```

- MyBatis可配置多种环境，用于将SQL映射应用于多种数据库
- 尽管可以配置多个环境，但每个SqlSessionFactory实例只能选择一种环境。
- 通过`id`为每个环境作标记，在父元素中通过`default`指定默认环境配置。
- 在构建SqlSessionFactory对象时，可将环境配置id作为参数传入；如果忽略了环境参数，则会加载默认环境。

#### 4.4.2 事务管理器 (transactionManager)

> 选择`JDBC`或`MANAGED`进行事务管理

#### 4.4.3 数据源 (dataSource)

> 选择`UNPOOLED`、`POOLED`或`JNDI`配置JDBC连接对象的资源

### 4.5 映射器 (mappers)

> 指定映射器注册方式
>
> - 文件路径 (通过映射文件实现映射)
> - 映射器接口 (通过注解实现映射)

#### 4.5.1 通过资源引用

```properties
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/CommentMapper.xml"/>
</mappers>
```

#### (2) 通过接口实现类

```properties
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.CommentMapper"/>
</mappers>
```

#### 4.5.3 通过包名导入

```properties
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

------

# 五、Mappers映射文件

> 映射文件使用户可以只专注于SQL代码，减少使用成本

## 5.1 查询 (select)

> 元素属性详情

| 属性          | 描述                                                     |
| ------------- | -------------------------------------------------------- |
| id            | 语句的标识符，通常设置为映射接口的相应方法名             |
| parameterType | 传入语句的参数类型，通常设置为映射接口相应方法的参数类型 |
| resultType    | 语句执行后的返回值类型，也即对应方法的返回值类型         |
| resultMap     | 对外部resultMap的命名引用，以解决复杂映射问题            |
| flushCache    | 设置为true后，语句执行后会清空本地缓存和二级缓存         |
| useCache      | 设置为true后，语句的执行结果会被二级缓存缓存起来         |

- `parameterType`和`resultType`设置的类型名称既可以是`类的全限定名`，也可以是`类型别名`
- 因为MyBatis可通过类型处理器推断出传入语句的参数类型，所以`parameterType`属性可以不设置。
- `resultType`和`resultMap`只能使用其中一种指定返回值类型。

## 5.2 增、删、改 (insert, update, delete)

> 元素属性详情

| 属性          | 描述                                                     |
| ------------- | -------------------------------------------------------- |
| id            | 语句的标识符，通常设置为映射接口的相应方法名             |
| parameterType | 传入语句的参数类型，通常设置为映射接口相应方法的参数类型 |
| flushCache    | 设置为true后，语句执行后会清空本地缓存和二级缓存         |

## 5.3 插入参数

### 5.3.1 #{}语法

> 在SQL语句中插入转义字符，作为执行语句的参数

- 通过创建参数占位符的方式设置参数，这种设置方式更安全，可避免SQL注入。
- 若某列允许传入null值，则需指定其`jdbcType`属性，如：`#{age,jdbcType=NUMERIC}`。
- 除非映射语句传入的参数类型是`HashMap`，MyBatis可自动推断出插入参数的类型。否则，需指定其`javaType`属性，如：`#{age,javaType=int}`。
- 对于数值型数据，可以设置`numericScale`属性指定小数点后保留的位数。

### 5.3.2 ${}语法

> 在SQL语句中插入不转义字符串

- 插入参数必须为字符串，且不进行转义，如：`ORDER BY ${columnName}`。
- 这种方式虽然不够安全，但更有高效。

## 5.4 结果映射

> MyBatis会自动将SQL语句的查询结果映射到一个实体对象中，这一过程利用ResultMap实现

### 5.4.1 隐式映射

- 使用`ResultType`指定返回值类型时，MyBatis会在幕后自动创建一个`ResultMap`，根据属性名将查询结果注入到实体对象中。

```xml-dtd
<select id="selectUsers" resultType="User">
  select
    user_id             as "id",
    user_name           as "username",
    hashed_password     as "password"
  from some_table
  where id = #{id}
</select>
```

