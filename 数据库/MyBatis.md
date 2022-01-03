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

### 3.1.1 新建空白Maven项目

说明：

- 在项目创建完成后，可删除多余文件、只保留`.idea`文件夹和`pom.xml`，将该项目作为父工程使用。在父工程中构建子模块，用以学习和测试MyBatis不同功能，这样可以避免相同依赖需要重复导入。
- 在父工程的pom.xml中配置项目通用依赖，在子模块的pom.xml中添加独有依赖。
- 在子模块的资源目录下添加该模块的配置文件。

### 3.1.2 添加项目依赖

> 将相关依赖添加到pom.xml文件中

```xml-dtd
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

## 3.2 环境搭建

### 3.2.1 新建子模块

> 在父工程下新建子模块

### 3.2.2 连接数据库

> 通过idea连接数据库

说明：第一次使用时，需创建数据库和数据表

```mysql
# 创建数据库
CREATE DATABASE IF NOT EXISTS my_test CHARACTER SET utf8;

# 使用刚创建的数据库
USE my_test;

# 创建数据表
CREATE TABLE student (
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    name VARCHAR(10) NOT NULL,
    gender CHAR(1) NOT NULL,
    born_date DATE NOT NULL
)CHARSET=utf8;
```

### 3.2.3 添加项目依赖

> 由于是子模块，可直接继承父工程依赖

### 3.2.4 创建数据库属性文件

> 在工程resouces目录下创建db.properties属性文件

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/my_test
username=root
password=12345678
```

### 3.2.5 创建Mybatis配置文件

> 在工程resouces目录下创建mybatis-config.xml配置文件

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
    private String gender;
    private Date bornDate;

    public Student(int id, String name, String gender, Date bornDate) {
        this.id = id;
        this.name = name;
        this.gender = gender;
        this.bornDate = bornDate;
    }
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

## 3.5 编写映射配置文件

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
        SELECT id, name, gender, born_date as bornDate
        FROM student
        WHERE id = #{id};
    </select>
</mapper>
```

注意：

- 当执行方法需要传入参数时，使用`#{参数名}`的方式传入，例如#{id}。

- 由于在数据表中，出生日期使用的字段是`born_date`，而实体类中字段是`bornDate`。
- 因此，在选择字段时不能使用`SELECT *`，需要使用`SELECT id, name, gender, born_date as bornDate`。

## 3.6 注册映射配置文件

> 在Mybatis配置文件中注册映射文件

```xml-dtd
<mappers>
		<mapper resource="mappers/StudentMapper.xml"/>
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
        System.out.println(student); // Student(id=1, name=张三, gender=男, bornDate=Tue Feb 15 00:00:00 CST 2000)
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

> 元素属性

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

> 元素属性

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

### 5.4.1 隐式配置 (ResultType)

- 使用`ResultType`指定返回值类型时，MyBatis会自动在幕后创建一个`ResultMap`，根据属性名将查询结果注入到实体对象中。
- 但是，此时MyBatis只会将值注入到与列名完全一致的对象属性中。
- 因此，若出现列名与对象字段不一致时，则需在执行语句中指定列的别名完成注入。

```java
public class User {
    private int id;
    private String username;
    private String password;
}
```

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

### 5.4.2 显式配置 (ResultMap)

```xml-dtd
<!-- 定义列名映射方式 -->
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>

<!-- 定义查询语句 -->
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

> 元素属性

| 属性 | 描述               |
| ---- | ------------------ |
| id   | 该结果映射的标识符 |
| type | 查询结果的类名     |

- type可使用类的全限定名、或是类的别名。

> 子元素

| 属性   | 描述                 |
| ------ | -------------------- |
| id     | 注入到字段的ID结果   |
| result | 注入到字段的普通结果 |

- `id`和`result`都会将一个列映射到对象的一个字段，但两者唯一区别在于：id元素对应的属性会被标记为对象的标识符，在比较对象实例时使用。
- 使用`id`可提高整体性能，尤其是进行**缓存和嵌套结果映射**的时候。因此，建议对主键列使用。

> id和result元素的属性

| 属性     | 描述                             |
| -------- | -------------------------------- |
| properpy | 映射对象的字段                   |
| column   | 数据表的列名                     |
| javaType | 当映射对象是HashMap时使用        |
| jdbcType | 可能为空值的列需指定映射结果类型 |

# 六、CURD实现

> 项目地址：https://gitee.com/shaobo1103/study/tree/master/MyBatis/02-curd

## 6.1 环境搭建

### 6.1.1 新建子模块

> 在父工程目录下新建子模块

### 6.1.2 连接数据库

> 通过idea连接数据库

### 6.1.3 添加项目依赖

> 由于是子模块，可直接继承父工程依赖

### 6.1.4 创建数据库属性文件

```properties
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/my_test
username=root
password=12345678
```

### 6.1.5 创建MyBatis配置文件

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"/>

    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <typeAliases>
        <package name="entity"/>
    </typeAliases>
    
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    
   <mappers>
       <package name="mapper"/>
   </mappers>
</configuration>
```

注意：

- 从代码规范的角度考虑
  - `entity`：在该包中创建实体类，部分工程也可起名为`pojo`。
  - `mapper`：在该包中创建映射接口、以及相对应的映射配置文件，部分工程也可起名为`dao`。
  - 与接口相对应的映射配置文件名应与接口名相同，例如StudentMapper接口对应的映射配置文件为`StudentMapper.xml`。
- 因此，尽管还没有创建实体类、映射接口、映射器配置文件，但这里已经可以根据约定，定义类型别名、设置映射器配置文件路径。

## 6.2 定义实体类

```java
package entity;

import lombok.Data;

import java.util.Date;

@Data
public class Student {
    private int id;
    private String name;
    private String gender;
    private Date bornDate;

    public Student(int id, String name, String gender, Date bornDate) {
        this.id = id;
        this.name = name;
        this.gender = gender;
        this.bornDate = bornDate;
    }
}
```

## 6.3 定义映射接口

```java
package mapper;

import entity.Student;

import java.util.Date;
import java.util.List;

public interface StudentMapper {
    // 查询所有学生
    List<Student> getStudents();

    // 根据id查询学生
    Student getStudentById(int id);

    // 根据姓名查询学生
    List<Student> getStudentByName(String name);

    // 通过传入对象新增一个学生
    void addStudentByObj(Student student);

    // 通过传入字段新增一个学生
    void addStudentByAttr(String name, String gender, Date bornDate);

    // 根据id删除一个学生
    void dropStudent(int id);

    // 根据id修改学生信息
    void updateStudent(int id);
}
```

注意：

- **因为映射器配置文件中使用方法名作为id，因此接口中不能定义重载的方法！**
- 因为可能存在重名，所以根据姓名查询的方法返回值类型为`List<Student>`。
- 根据使用需求，在进行增、删、改的操作时，也可以设置返回值类型为`int`，代表语句执行影响的行数。

## 6.4 编写映射配置文件

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="mapper.StudentMapper">
    <!-- 结果映射 -->
    <resultMap id="stuResultMap" type="Student">
        <id column="id" property="id"/>
        <result column="born_date" property="bornDate"/>
    </resultMap>

    <!-- 查询所有学生 -->
    <select id="getAllStudents" resultType="Student">
        select *
        from my_test.student;
    </select>

    <!-- 根据id查询学生 -->
    <select id="getStudentById" resultType="Student">
        select *
        from my_test.student
        where id = #{id};
    </select>

    <!-- 根据姓名查询学生 -->
    <select id="getStudentByName" resultType="Student">
        select *
        from my_test.student
        where name = #{name};
    </select>

    <!-- 通过传入对象新增一个学生 -->
    <insert id="addStudentByObj" parameterType="Student">
        insert into my_test.student (id, name, gender, born_date)
        values (#{id}, #{name}, #{gender}, #{bornDate});
    </insert>

    <!-- 通过传入字段新增一个学生 -->
    <insert id="addStudentByAttr">
        insert into my_test.student (name, gender, born_date)
        values (#{arg0}, #{arg1}, #{arg2});
    </insert>

    <!-- 根据id删除一个学生 -->
    <delete id="deleteStudent">
        delete
        from my_test.student
        where id = #{id};
    </delete>

    <!-- 清空表中所有数据 -->
    <delete id="deleteAllStudents">
        truncate my_test.student;
    </delete>

    <!-- 根据id修改学生信息 -->
    <update id="updateStudentById" parameterType="Student">
        update my_test.student
        set name=#{name},
            gender=#{gender},
            born_date=#{bornDate}
        where id=#{id};
    </update>
</mapper>
```

注意：

- 若没有显式设置`parameterType="Map"`时，在执行语句内不能以方法中的参数名进行命名，而需要通过`arg0,arg1,...`或`param1,param2,...`的方式传入，除非该方法只需要一个参数。

- 例如：在映射`addStudentByAttr`方法时，因为没有通过map型数据传入参数，所以执行语句中使用`arg0,arg1,arg2`传入方法的参数。

## 6.5 注册映射配置文件

> 已经通过包名指定配置文件路径，因此这一步无需任何操作。

## 6.6 获取SqlSession对象

```java
package util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtil {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(true);
    }
}
```

注意：`openSession(true)`设置为true表示语句执行后自动提交事物。

## 6.7 执行接口方法

```java
import entity.Student;
import mapper.StudentMapper;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import util.MybatisUtil;

import java.text.ParseException;
import java.text.SimpleDateFormat;

public class Main {
    @Test
    public void test() throws ParseException {
        SqlSession sqlSession = MybatisUtil.getSqlSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

        // 清空表中所有数据
        mapper.deleteAllStudents();

        // 新增数据
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
        Student student1 = new Student(1, "张三", "男", df.parse("1995-1-1"));
        mapper.addStudentByObj(student1);
        mapper.addStudentByAttr("李四", "男", df.parse("1999-1-1"));
        mapper.addStudentByAttr("王五", "女", df.parse("1996-11-10"));
        System.out.println("新增数据后，表中数据：");
        for (Student student : mapper.getAllStudents()) {
            System.out.println(student);
        }

        // 分割线
        System.out.println("-----------------------------------------");

        // 修改数据
        student1.setGender("女");
        mapper.updateStudentById(student1);

        // 根据id查询数据
        System.out.println("根据id查询结果：" + mapper.getStudentById(1));

        // 根据姓名查询学生
        System.out.println("根据姓名查询结果：" + mapper.getStudentByName("张三"));

        // 分割线
        System.out.println("-----------------------------------------");

        // 根据id删除一个学生
        mapper.deleteStudent(3);
        System.out.println("删除一个学生后，表中数据：");
        for (Student student : mapper.getAllStudents()) {
            System.out.println(student);
        }

        // 清空表中所有数据
        mapper.deleteAllStudents();
        System.out.println("删除所有学生后，表中数据：");
        for (Student student : mapper.getAllStudents()) {
            System.out.println(student);
        }
        // 分割线
        System.out.println("-----------------------------------------");

        sqlSession.close();
    }
}
```

