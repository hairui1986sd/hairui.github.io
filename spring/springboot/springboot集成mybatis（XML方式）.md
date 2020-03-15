# springboot集成mybatis（XML方式）





### 添加主要依赖

    <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>2.1.2</version>
        </dependency>
    
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <scope>runtime</scope>
        </dependency>

### application.yml文件配置

- config-locations：配置mybatis-config.xml路径，mybatis-config.xml中配置MyBatis基础属性
- mapper-locations：配置Mapper对应的XML文件路径
- mybatis.type-aliases-package：配置项目中实体类包路径
- spring.datasource.*：数据源配置

Spring Boot启动时数据源会自动注入到SqlSessionFactory中，使用SqlSessionFactory构建SqlSessionFactory，再自动注入到Mapper中，最后直接使用Mapper即可。

由于我使用的是比较新的Mysql连接驱动，所以配置文件可能和之前有一点不同。注意：我们使用的 mysql-connector-java 8+ ，JDBC 连接到mysql-connector-java 6+以上的需要指定时区 `serverTimezone=GMT%2B8`。

```
spring:
  datasource:
    driverClassName: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/lift?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2b8
    username: jerico
    password: 12345678

mybatis:
  config-locations: classpath:mybatis-config.xml
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.jerico.spring.mybatis.entity
```

### 启动类

在启动类中添加对Mapper包扫描@MapperScan，Spring Boot启动的时候会自动加载包路径下的Mapper。或者直接在Mapper类上面添加注解@Mapper，但是在每个Mapper添加注解会很麻烦。

```java
@SpringBootApplication
@MapperScan("com.jerico.spring.mybatis.dao")
public class MybatisApplication {

  public static void main(String[] args) {
    SpringApplication.run(MybatisApplication.class, args);
  }
}
```

### 配置mybatis-config.xml

MyBatis公共属性mybatis-config.xml，主要配置常用的typeAliases，设置类型别名为Java类型设置一个短的名字。它只和XML配置有关，存在的意义仅在于用来减少类完全限定名的冗余。这样在使用Mapper.xml时候，需要引入可以直接这样写：

```xml
resultType="Integer" 
parameterType="Long"
```

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <!--配置驼峰属性自动映射，例如实体中属性为 userSex,数据库属性为 user_sex，MyBatis 默认是不能自动转换的。
我们可以在application.yml中配置 mybatis.configuration.map-underscore-to-camel-case=true或在该文件中
进行如下配置，实现自动映射。如果不进行此配置，通常我们要自定义以下结果集映射： -->
  <settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
  </settings>

  <typeAliases>
    <typeAlias alias="Integer" type="java.lang.Integer"/>
    <typeAlias alias="Long" type="java.lang.Long"/>
    <typeAlias alias="HashMap" type="java.util.HashMap"/>
    <typeAlias alias="LinkedHashMap" type="java.util.LinkedHashMap"/>
    <typeAlias alias="ArrayList" type="java.util.ArrayList"/>
    <typeAlias alias="LinkedList" type="java.util.LinkedList"/>
  </typeAliases>
</configuration>
```

以上配置也可以直接配置在application.yml文件中，那么就无需在application.xml中配置classpath:/config/mybatis-config.xml。

### 添加User的映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.jerico.spring.mybatis.dao.UserDao">
  <sql id="base_column">
    user_id,
    user_name,
    age,
    phone,
    email,
    money,
    create_time,
    created_by,
    last_updated_by,
    last_update_time
  </sql>

  <select id="getUser">
    select
    <include refid="base_column"/>
    from common_user
    where user_id = #{id}
  </select>

  <select id="listUsers">
    select
    <include refid="base_column"/>
    from common_user
  </select>

  <insert id="insertUser">
    insert into common_user
    <include refid="base_column"/>
    values (#{id}, #{userName}, #{age},#{phone},#{email},#{money},#{createdBy},#{lastUpdatedBy})
  </insert>

  <update id="updateUser">
    update common_user
    set user_name       = #{userName},
        age             = #{age},
        phone           = #{phone},
        email           = #{email},
        money           = #{money},
        last_updated_by = #{lastUpdatedBy}
    where user_id = #{userId}
  </update>

  <delete id="deleteUser">
    delete
    from common_user
    where user_id = #{userId}
  </delete>

</mapper>
```

命名空间指明对应Dao接口的地址：<mapper namespace="com.jerico.spring.mybatis.dao.UserDao">

<sql>标签用于写可以复用的XML；

### 编写DAO层接口

```java
package com.jerico.spring.mybatis.dao;

import com.jerico.spring.mybatis.entity.UserEntity;
import java.util.List;

/**
 * @className: UserDao
 * @description:
 * @author: jerico
 * @date: 2020-03-13 00:29
 * @version:
 */
public interface UserDAO {

  /**
   * 根据id查询用户信息
   *
   * @param id 用户id
   * @return 用户信息
   */
  UserEntity getUser(int id);

  /**
   * 查询所有用户
   *
   * @return 所有用户列表
   */
  List<UserEntity> listUsers();

  /**
   * 新增用户
   *
   * @param userEntity 用户信息
   * @return 成功的条数
   */
  int insertUser(UserEntity userEntity);

  /**
   * 全量更新用户
   *
   * @param userEntity 用户信息
   * @return 成功的条数
   */
  int updateUser(UserEntity userEntity);

  /**
   * 根据用户id删除用户
   *
   * @param id 用户id
   * @return 成功的条数
   */
  int deleteUser(int id);
}
```

这里的方法名需要和xml中id属性一致。

### 编写Service和controller代码







### mybatis插入时主键生成问题

```xml
<insert id="insertUser">
  insert into common_user (user_id,
                           user_name,
                           age,
                           phone,
                           email,
                           money,
                           create_time,
                           created_by,
                           last_updated_by,
                           last_update_time)
  values (#{userId}, #{userName},
          #{age}, #{phone}, #{email}, #{money}, now(), #{createdBy}, #{lastUpdatedBy}, now())
</insert>
```

首先，如果你的数据库支持自动生成主键的字段（比如 MySQL 和 SQL Server），上面的写法，数据库生成的主键user_id无法赋给userEntity对象。如果需要回传id的话，那么你可以设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置为目标属性就 OK 了。例如，如果上面的 common_user表已经在 user_id 列上使用了自动生成，那么语句可以修改为：

```xml
<insert id="insertUser" useGeneratedKeys="true" keyProperty="userId">
  insert into common_user (user_id,
                           user_name,
                           age,
                           phone,
                           email,
                           money,
                           create_time,
                           created_by,
                           last_updated_by,
                           last_update_time)
  values (#{userId}, #{userName},
          #{age}, #{phone}, #{email}, #{money}, now(), #{createdBy}, #{lastUpdatedBy}, now())
</insert>
```

对于不支持自动生成主键列的数据库和可能不支持自动生成主键的 JDBC 驱动，MyBatis 有另外一种方法来生成主键。这里有一个简单（也很傻）的示例，它可以生成一个随机 ID（不建议实际使用，这里只是为了展示 MyBatis 处理问题的灵活性和宽容度）：

```
<insert id="insertAuthor">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    select CAST(RANDOM()*1000000 as INTEGER) a from SYSIBM.SYSDUMMY1
  </selectKey>
  insert into Author
    (id, username, password, email,bio, favourite_section)
  values
    (#{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection,jdbcType=VARCHAR})
</insert>
```



### 集成分页插件Pagehelper

如何使用： https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/HowToUse.md

##### 在application.yml文件中配置分页相关信息（参数具体含义参考以上链接）

```
#pagehelper分页插件配置
pagehelper:
  helperDialect: mysql
  reasonable: true
  supportMethodsArguments: true
  params: count=countSql
```

##### 在service层写分页代码

写法一：

```java
@Override
public PageInfo<UserEntity> listUsers(int page, int size) {
  // 将参数传给这个方法就可以实现物理分页了，非常简单
  PageHelper.startPage(page, size);
  List<UserEntity> userList = userDAO.listUsers();
  PageInfo<UserEntity> pageInfo = new PageInfo<>(userList);
  return pageInfo;
}
```

写法二（通过实践发现，分页效果有效，但是返回中没有分页相关的信息）：

```java
//方式二（实现了分页却没有分页信息返回） start
// 将参数传给这个方法就可以实现物理分页了，非常简单
PageHelper.startPage(page, size);
Page<UserEntity> pageResult = userDAO.listUsers();
return pageResult;
```

**分页原理：PageHelper.startPage会拦截下一个sql，也就是List<UserEntity> userList = userDAO.listUsers()的SQL。并且根据当前数据库的语法，把这个SQL改造成一个高性能的分页SQL，同时还会查询该表的总行数，具体可以看SQL日志。**
**PageHelper.startPage和List<UserEntity> userList = userDAO.listUsers();最好紧跟在一起，中间不要有别的逻辑，否则可能出BUG。**
**Page page：相当于一个list集合，userDAO.listUsers()方法查询完成后，会给page对象的相关参数赋值。**

### 日志级别

在application.yml文件中配置sql的日志级别

```xml
格式： logging.level.你的包名.mybatis接口包=debug
示例： logging:
  		level:
            com.jerico.spring.mybatis:
                dao: debug
```

### 参考

[「Java」 - SpringBoot & MyBatis xml版](https://zhuanlan.zhihu.com/p/65654819)

[SpringBoot系列-整合Mybatis（XML配置方式）](https://juejin.im/post/5dcb7691f265da4d3f44c5cf)

[基于 SpringBoot2.0+优雅整合 SpringBoot+Mybatis](https://segmentfault.com/a/1190000017211657)