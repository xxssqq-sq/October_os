# Hibernate

## 一、idea中hibernate搭建

### 1、新建maven空项目

### 2、修改pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.doubecharli</groupId>
    <artifactId>hibernate</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <!--hibernate-->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.1.0.Final</version>
        </dependency>

        <!--mysql driver-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.40</version>
        </dependency>

    </dependencies>

</project>
```

## 二、hello

使用Hibernate一共有四步，分别是：

- 创建Hibernate配置文件
- 创建持久化类
- 创建映射文件
- 通过Hibernate API访问数据库

### 1、创建Hibernate配置文件

配置文件主要是配置数据库的一些信息

默认名为hibernate.cfg.xml，放在resources文件夹下。

```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>

        <!-- 指定连接数据库所用的驱动 -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <!-- 指定连接数据库的url，hibernate连接的数据库名 -->
        <property name="connection.url">jdbc:mysql://localhost:3306/hibernate</property>
        <!-- 指定连接数据库的用户名 -->
        <property name="hibernate.connection.username">root</property>
        <!-- 数据库的登陆密码 -->
        <property name="hibernate.connection.password">199498xy</property>
        <!-- 指定连接数据库的编码 -->
        <property name="connection.characterEncoding">utf8</property>
        <!-- 指定数据库方言 -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <!-- 显示Hibernate持久化操作所生成的SQL -->
        <property name="show_sql">true</property>
        <!-- 将SQL脚本进行格式化后再输出 -->
        <property name="format_sql">true</property>
        <!-- 指定自动生成数据表的策略 -->
        <property name="hbm2ddl.auto">update</property>

        <!-- 罗列所有的映射文件 -->
        <mapping resource="mapping/User.hbm.xml"/>
    </session-factory>
</hibernate-configuration>

```

