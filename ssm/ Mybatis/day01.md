# MyBatis入门  
## Mybatis的环境搭建
* 第一步：创建maven工程并导入坐标
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.wzl</groupId>
    <artifactId>day01_eesy_01mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <!--Mybatis依赖，必需-->
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <!--数据库依赖，必需-->
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <!--日志依赖-->
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <!--单元测试-->
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>

</project>
```
* 第二步：创建实体类User.java和dao的接口IUserDao.java

```
package com.heima.domain;

import java.util.Date;

public class User {
    //属性名必须和表字段名一致
    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;

   //getter和setter
   ...

   //toString
   ...
}
```
```
package com.heima.dao;

import com.heima.domain.User;

import java.util.List;

/**
 *
 * 用户的持久层接口
 */


public interface IUserDao {
    /**
     * 查询所有操作
     */
    List<User> findAll();
}

```
* 第三步：创建MyBatis的主配置文件SqlMapConfig.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--以上为Config约束-->
<configuration>
    <!--配置环境-->
    <environments default="mysql">
        <!--配置mysql环境-->
        <environment id="mysql">
            <!--配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源（连接池）-->
            <dataSource type="POOLED">
                <!--配置连接数据库的四个基本信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql//localhost:8081/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="000413"/>
            </dataSource>
        </environment>
    </environments>

    <!--指定映射配置文件的位置，映射位置文件指的是每个dao独立的配置文件-->
    <mappers>
        <mapper resource="com/heima/dao/IUserDao.xml"/>
    </mappers>
</configuration>
```
* 第四步：创建映射配置文件IUserDao.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--以上为Mapper约束-->
 <mapper namespace="com.heima.dao.IUserDao">

    <!--配置查询所有-->
    <select id="findAll">
        select * from user;
    </select>
</mapper>
```
## 环境搭建的注意事项：
* 第一个：创建映射文件IUserDao.xml 和 操作接口IUserDao.java时取名为dao是为了和我们之前的知识保持一致。在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper。所以：IUserDao 和 IUserMapper是一样的。
* 第二个：在idea中创建目录的时候，它和包是不一样的。包在创建时：com.itheima.dao它是三级结构；目录在创建时：com.itheima.dao是一级目录。
* 第三个：mybatis的映射配置文件位置必须和dao接口的包结构相同
* 第四个：映射配置文件的mapper标签namespace属性的取值必须是dao（mapper）接口的全限定类名
* 第五个：映射配置文件的操作配置（select or ...），id属性的取值必须是dao（mapper）接口的方法名
* 当我们遵从了第三，四，五点之后，我们在开发中就无须再写dao的实现类。
