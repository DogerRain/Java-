> SSM（Spring+SpringMVC+MyBatis）框架集由Spring、MyBatis两个开源框架整合而成（SpringMVC是Spring中的部分内容）。常作为数据源较简单的web项目的框架。

下面手把手教你使用IDEA搭建一个SSM项目：

##  新建project

File—》新建project，使用maven作为包管理工具，因为我这里使用了父项目，所以我是新建一个module，都一样的。

![image-20200528233357103](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200528233357103.png)

设置名称：

![image-20200528233426546](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200528233426546.png)

我们打开发现，项目没有java目录，没关系我们可以自己创建，打开文件结构（快捷键 Ctrl+Shift+Alt+S）

![image-20200528234537259](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200528234537259.png)

然后把这个java文件夹设置为source目录：

![image-20200528234722786](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200528234722786.png)



如果`application.xml`表头的xml报错，可以把链接复制过来：

该图片来源见水印

![](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/20190601111820419.png)



![image-20200528235632989](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200528235632989.png)

## 1  引入spring核心组件

修改`pom.xml` ， 注意标签

```xml
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <!-- spring版本号 -->
        <spring.version>4.3.5.RELEASE</spring.version>
        <!-- mybatis版本号 -->
        <mybatis.version>3.4.1</mybatis.version>
    </properties>

    <dependencies>
          <!--jsp依赖-->
          <!--<dependency>-->
              <!--<groupId>javax.servlet.jsp</groupId>-->
              <!--<artifactId>javax.servlet.jsp-api</artifactId>-->
              <!--<version>2.3.1</version>-->
              <!--<scope>provided</scope>-->
          <!--</dependency>-->
          <!--jstl-->
          <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>jstl</artifactId>
              <version>1.2</version>
          </dependency>

        <!-- mybatis框架包 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <!--<version>1.1.1</version>-->
            <version>1.3.3</version>
        </dependency>
        <!-- mybatis框架包 -->


        <!--springMVC-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--spring的核心包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--问配置文件、创建和管理bean 以及进行Inversion of Control-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--为Spring 核心提供了大量扩展。可以找到使用Spring ApplicationContext特性时所需的全部类-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--Spring 对JDBC 数据访问进行封装的所有类 可以使用原生的jdbc connector-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Spring web 包含Web应用开发时，用到Spring框架时所需的核心类，包括自动载入WebApplicationContext特性的类、
         Struts与JSF集成类、文件上传的支持类、Filter类和大量工具辅助类-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!--日志管理工具-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!-- druid连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>

        <!-- 数据库驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.35</version>
        </dependency>
    </dependencies>

    <!--打包声明-->
    <build>
        <finalName>SSM_project</finalName>
        <plugins>
            <plugin>
                <!-- https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-compiler-plugin -->
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>UTF8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## 2 配置web.xml

这里引入了两个文件 `applicationContext.xml` 和 `spring-mvc.xml`

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

  <display-name>mvcDemo</display-name>
  <!--项目的欢迎页，项目运行起来后访问的页面-->
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>

  <!-- 注册ServletContext监听器，创建容器对象，并且将ApplicationContext对象放到Application域中 -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!-- 指定spring核心配置文件 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <!-- 解决乱码的过滤器 -->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>

    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!-- 配置前端控制器 -->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 指定配置文件位置和名称 如果不设置,默认找/WEB-INF/<servlet-name>-servlet.xml -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
    <async-supported>true</async-supported>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>
```





注意：以下配置文件均在`resources` 目录下

## 3 新建`log4j.properties`

配置：

```c
#日志输出级别
log4j.rootLogger=debug,stdout,D,E

#设置stdout的日志输出控制台
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
#输出日志到控制台的方式，默认为System.out
log4j.appender.stdout.Target = System.out
#设置使用灵活布局
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
#灵活定义输出格式
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} -[%p]  method:[%c (%rms)] - %m%n
```

如果不需要日志管理，可以不配置，类似于log4j的日志管理工具还有`logback`,logback可以参考：[https://blog.csdn.net/yudianxiaoxiao/article/details/86664616](https://blog.csdn.net/yudianxiaoxiao/article/details/86664616)

## 4 新建 jdbc.properties

**本地数据库配置：**

```java
jdbc.driver=com.mysql.jdbc.Driver
#数据库地址
jdbc.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8
#用户名
jdbc.username=root
#密码
jdbc.password=root
#最大连接数
c3p0.maxPoolSize=30
#最小连接数
c3p0.minPoolSize=10
#关闭连接后不自动commit
c3p0.autoCommitOnClose=false
#获取连接超时时间
c3p0.checkoutTimeout=10000
#当获取连接失败重试次数
c3p0.acquireRetryAttempts=2
```

初始化一个表：

```sql
DROP TABLE IF EXISTS t_user_hellocoder;
CREATE TABLE `t_user_hellocoder` (
  `id` int(11) NOT NULL,
  `name` VARCHAR(11) DEFAULT NULL,
  `description` VARCHAR(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8  ;

INSERT INTO `test`.`t_user_hellocoder`(`id`, `name`, `description`) VALUES (1, ''我是HaC'', ''一枚混迹于互联网的程序猿'');
```



## 5 配置 `applicati onContext.xml` 和 `spring-mvc.xml`

新建 `spring-mvc.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <!-- 扫描注解，声明com.ssm.hellocoder包下的文件都能被扫描 -->
    <context:component-scan base-package="com.ssm.hellocoder"/>

    <!-- 开启SpringMVC注解模式 -->
    <mvc:annotation-driven/>

    <!-- 静态资源默认servlet配置 -->
    <mvc:default-servlet-handler/>

    <!-- 配置返回视图的路径，以及识别后缀是jsp文件 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <!--即默认是/WEB-INF/jsp/目录的文件-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--默认是返回 名称+.jsp文件-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

`applicationContext.xml`:

我新建的项目`applicationContext.xml`在`WEB-INF`目录下，需要把它移动到`resources`目录下，如果本来就在就不用管，没有就新建。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
                        http://www.springframework.org/schema/tx
                        http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 加载properties文件 -->
    <bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="location" value="classpath:jdbc.properties"/>
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- mybatis和spring完美整合，不需要mybatis的配置映射文件 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 扫描model包 -->
        <property name="typeAliasesPackage" value="com.ssm.hellocoder.entity"/>
        <!-- 扫描sql配置文件:mapper需要的xml文件-->
        <property name="mapperLocations" value="classpath:mapper/TUserHellocoderDao.xml"/>
    </bean>

    <!-- Mapper动态代理开发，扫描dao接口包-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 给出需要扫描Dao接口包 -->
        <property name="basePackage" value="com.ssm.hellocoder.dao"/>
    </bean>

    <!-- 事务管理 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--数据库连接池-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```



## 6 新建controller、service、dao、entity、xml、jsp

先上目录结构，大家不懂的可以仿照一下：

![image-20200529230244510](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200529230244510.png)

`TUserHellocoder.java`

```java
package com.ssm.hellocoder.entity;

import java.io.Serializable;

/**
 * (TUserHellocoder)实体类
 *
 * @author HaC
 * @since 2020-05-29 01:28:27
 */
public class TUserHellocoder implements Serializable {
    private static final long serialVersionUID = 417502814533585676L;
    
    private Integer id;
    
    private String name;
    
    private String description;


    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

}
```

`TUserHellocoderController.java`

```java
package com.ssm.hellocoder.controller;

import com.ssm.hellocoder.service.TUserHellocoderService;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import javax.annotation.Resource;

/**
 * (TUserHellocoder)表控制层
 *
 * @author HaC
 * @since 2020-05-29 00:47:08
 */
@Controller
@RequestMapping("/HelloCoder")
public class TUserHellocoderController {
    /**
     * 服务对象
     */
    @Resource
    private TUserHellocoderService tUserHellocoderService;

    /**
     * 通过主键查询单条数据
     *
     * @param id 主键
     * @return 单条数据
     */
    @GetMapping("/selectUser")
    public ModelAndView selectOne(@RequestParam(value = "id") Integer id) {
        ModelAndView md = new ModelAndView("index");
        //把数据存到ModelAndView，给前端
        md.addObject("user",this.tUserHellocoderService.queryById(id));
        return md;
    }
}
```

 `TUserHellocoderService.java`

```java
package com.ssm.hellocoder.service;

import com.ssm.hellocoder.entity.TUserHellocoder;

/**
 * (TUserHellocoder)表服务接口
 *
 * @author HaC
 * @since 2020-05-29 01:28:29
 */
public interface TUserHellocoderService {

    /**
     * 通过ID查询单条数据
     *
     * @param id 主键
     * @return 实例对象
     */
    TUserHellocoder queryById(Integer id);

}
```

`TUserHellocoderServiceImpl.java`

```java
package com.ssm.hellocoder.service.impl;

import com.ssm.hellocoder.dao.TUserHellocoderDao;
import com.ssm.hellocoder.entity.TUserHellocoder;
import com.ssm.hellocoder.service.TUserHellocoderService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

/**
 * (TUserHellocoder)表服务实现类
 *
 * @author HaC
 * @since 2020-05-29 01:28:29
 */
@Service("tUserHellocoderService")
public class TUserHellocoderServiceImpl implements TUserHellocoderService {
    @Resource
    private TUserHellocoderDao tUserHellocoderDao;

    /**
     * 通过ID查询单条数据
     *
     * @param id 主键
     * @return 实例对象
     */
    @Override
    public TUserHellocoder queryById(Integer id) {
        return this.tUserHellocoderDao.queryById(id);
    }
}
```

`TUserHellocoderDao.java`

```java
package com.ssm.hellocoder.dao;

import com.ssm.hellocoder.entity.TUserHellocoder;

/**
 * (TUserHellocoder)表数据库访问层
 *
 * @author HaC
 * @since 2020-05-29 01:28:28
 */
public interface TUserHellocoderDao {

    /**
     * 通过ID查询单条数据
     *
     * @param id 主键
     * @return 实例对象
     */
    TUserHellocoder queryById(Integer id);

}
```

在`resources`目录下新建一个文件夹`mapper`，新建文件 `TUserHellocoderDao.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ssm.hellocoder.dao.TUserHellocoderDao">

    <resultMap type="com.ssm.hellocoder.entity.TUserHellocoder" id="TUserHellocoderMap">
        <result property="id" column="id" jdbcType="INTEGER"/>
        <result property="name" column="name" jdbcType="VARCHAR"/>
        <result property="description" column="description" jdbcType="VARCHAR"/>
    </resultMap>

    <!--查询单个-->
    <select id="queryById" resultMap="TUserHellocoderMap">
        select
          id, name, description
        from test.t_user_hellocoder
        where id = #{id}
    </select>

</mapper>
```

在`WEB-INF`目录下新建一个文件夹`jsp`，新建文件 `index.jsp`

```jsp
<html>
<body>
<h2>Hello World!</h2>
<p>${user.name}</p>
<p>${user.description}</p>
</body>
</html>
```

以上就是SSM项目的配置了。



## 7 运行

配置服务器运行：

 加上 `-Dfile.encoding=UTF-8` 解决日志乱码![image-20200529010055832](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200529010055832.png)



选择war包：

![image-20200529005459184](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200529005459184.png)

 运行

浏览器输入 `http://localhost:8081/HelloCoder/selectUser?id=1`

![image-20200529231602740](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200529231602740.png)

大功告成！



源码在Github：[https://github.com/DogerRain/DemoFamily](https://github.com/DogerRain/DemoFamily)

