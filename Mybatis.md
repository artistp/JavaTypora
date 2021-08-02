# 1.简介

## 1.1 什么是Mybatis

- MyBatis 是一款优秀的持久层框架，
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

## 1.2 获取Mybatis

- maven 仓库
- Github：https://github.com/mybatis/mybatis-3
- 中文文档：https://mybatis.org/mybatis-3/zh/index.html

## 1.3 持久化

数据持久化

- 持久化就是将程序的数据 瞬时状态 向 持久状态 转化的过程
- 内存：断电即失
- 数据库, io文件持久化

## 1.4 持久层

完成持久化操作的代码块

# 2. 使用Mybatis

每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。

从 XML 文件中构建 SqlSessionFactory 的实例非常简单，建议使用类路径下的资源文件进行配置。 但也可以使用任意的输入流（InputStream）实例，MyBatis 包含一个名叫 Resources 的工具类，它包含一些实用方法，使得从类路径或其它位置加载资源文件更加容易。

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

SqlSessionFactory 是用来获得执行SQL语句的对象，SqlSession提供了执行SQL语句的方法

SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
}
```

```java
// 工具类
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//sqlsessionFactory --> sqLSessionpublic 
class Mybatisutils {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            //使用Mybatis第一步:获取sqLsessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们就可以从中获得 SqlSession的实例了。
    // SqlSession完全包含了面向数据库执行SQL命令所需的所有方法。
    public static SqlSession getsqlsession() {
        return sqlSessionFactory.openSession();
    }
}

```

# mybatis原理解析

https://www.jianshu.com/p/ec40a82cae28
