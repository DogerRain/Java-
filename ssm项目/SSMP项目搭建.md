>SSM项目是spring+springMVC+mybatis集合而成的项目，mybatis作为一个ORM框架，相对原生的JDBC来说，操作数据库更简单、更符合Java的思想。但是使用mybatis操作数据库的时候，我们需要写很多xml，而使用mybatis-plus则可以把数据库的表当成一个对象去操作。
>

上一篇搭建了一个SSM项目 [搭建SSM项目](https://mp.weixin.qq.com/s?__biz=MzAxNTc4ODYzOQ==&mid=2247483967&idx=1&sn=ef0ce1392636cdd5761a8f5c3c106de7&chksm=9bfffd0fac887419c2094c100e659893d6a700b8aae2dae67686d3682a8b9f3275887f0313ba&token=522750604&lang=zh_CN#rd)，这篇文章只要讲述基于上一次的SSM项目，把mybatis换成mybatis-plus，让大家看看mybatis-plus的好处。

## 1. 引入mybatis-plus组件

pom引入`mybatis-plus`，尽量使用3.x以上版本，因为2.x版本已经不维护了。

```xml
 <!-- mybatis框架包 -->
<!-- <dependency>
     <groupId>org.mybatis</groupId>
     <artifactId>mybatis</artifactId>
     <version>${mybatis.version}</version>
 </dependency>
 <dependency>
     <groupId>org.mybatis</groupId>
     <artifactId>mybatis-spring</artifactId>
     &lt;!&ndash;<version>1.1.1</version>&ndash;&gt;
     <version>1.3.3</version>
 </dependency>-->
 <!-- mybatis框架包 -->

 <!--mybatis-plus包，就不需要上面两个mybatis包了-->
 <dependency>
     <groupId>com.baomidou</groupId>
     <artifactId>mybatis-plus</artifactId>
     <version>3.3.1</version>
 </dependency>
```

之前SSM项目使用`mybatis-spring`与spring集成，用`mybatis-plus`就不需要了，因为已经集成了`mybatis-spring` 

![mybatis-plus集成mybatis-spring](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200604222540763.png)

## 2. 修改applicationContext.xml

SSM项目的`sqlSessionFactory`一般是这样配置的：

```xml
<!-- mybatis和spring完美整合，不需要mybatis的配置映射文件 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!-- 扫描model包 -->
    <property name="typeAliasesPackage" value="com.ssm.hellocoder.entity"/>
    <!-- 扫描sql配置文件:mapper需要的xml文件-->
    <property name="mapperLocations" value="classpath:mapper/*.xml"/>
</bean>
```
使用`mybatis-spring`需要改成这样：

```xml
<!-- mybatis-plus -->
<bean id="sqlSessionFactory"
      class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!-- 自动扫描mapperxml文件 -->
    <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    <!--取别名 -->
    <property name="typeAliasesPackage" value="com.ssm.hellocoder.entity"/>
</bean>
<bean id="configuration" class="com.baomidou.mybatisplus.core.MybatisConfiguration">
    <!--使用驼峰，默认为true-->
    <property name="mapUnderscoreToCamelCase" value="true"/>
     <!--日志打印SQL语句-->
     <property name="logImpl" value="org.apache.ibatis.logging.log4j.Log4jImpl"/>
</bean>
```

`mybatis-spring`的bean还有很多配置，具体可以参考：[https://mp.baomidou.com/config/](https://mp.baomidou.com/config/)

## 3. 使用tableName实现对象与表映射

```java
@TableName("t_user_hellocoder")
public class TUserHellocoder implements Serializable {
    private static final long serialVersionUID = 417502814533585676L;

    private Integer id;

    private String name;

    private String description;
    private String createTime;

    public String getCreateTime() {
        return createTime;
    }

    public void setCreateTime(String createTime) {
        this.createTime = createTime;
    }


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
```

其中`t_user_hellocoder`是我在数据库的表

```sql
CREATE TABLE `t_user_hellocoder` (
  `id` int(11) NOT NULL,
  `name` varchar(11) DEFAULT NULL,
  `description` varchar(100) DEFAULT NULL,
  `create_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

INSERT INTO `test`.`t_user_hellocoder`(`id`, `name`, `description`, `create_time`) VALUES (1, '我是HaC', '一枚混迹于互联网的程序猿', '2020-06-04 21:47:35');
```

## 4. Dao（Mapper）修改

```java
package com.ssm.hellocoder.dao;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.ssm.hellocoder.entity.TUserHellocoder;
import org.apache.ibatis.annotations.Mapper;

/**
 * (TUserHellocoder)表数据库访问层
 *
 * @author HaC
 * @since 2020-05-29 01:28:28
 */
@Mapper
public interface TUserHellocoderDao extends BaseMapper<TUserHellocoder> {

}
```

继承`BaseMapper`，现在这个dao就是与表`TUserHellocoder`（表）绑定了。

## 5. 操作数据库

```java
package com.ssm.hellocoder.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.ssm.hellocoder.dao.TUserHellocoderDao;
import com.ssm.hellocoder.entity.TUserHellocoder;
import com.ssm.hellocoder.service.TUserHellocoderService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.util.List;

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
        TUserHellocoder tUserHellocoder = new TUserHellocoder();
//        tUserHellocoder = tUserHellocoderDao.selectOne(new QueryWrapper<TUserHellocoder>().lambda().eq(TUserHellocoder::getId, id).isNotNull(TUserHellocoder::getId));
        tUserHellocoder = tUserHellocoderDao.selectOne(new QueryWrapper<TUserHellocoder>().eq("id", id).isNotNull("id"));
//        List<TUserHellocoder> tUserHellocoderList = tUserHellocoderDao.selectList(new QueryWrapper<TUserHellocoder>().eq("id", id).isNotNull("id"));
        return tUserHellocoder;

    }
}
```

selectOne表示返回一个TUserHellocoder对象，`eq("id", id)`表示 `where ’id‘ =id`

```java
tUserHellocoderDao.selectOne(new QueryWrapper<TUserHellocoder>().lambda().eq(TUserHellocoder::getId, id).isNotNull(TUserHellocoder::getId));
```

以上这种属于lambda的写法，`QueryWrapper`后面可以接很多条件：

![QueryWrapper用法](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/15538326-0d4969c75a01f974.png)

更多条件参考：[https://mp.baomidou.com/guide/wrapper.html](https://mp.baomidou.com/guide/wrapper.html)



使用驼峰：

数据库的列名是`create_time`，使用驼峰命名法转成对象的时候，会自动转成`createTime`，不需要人为把sql 多写 `select create_time as createTime`，命名更符合Java的规范。

![image-20200604225617997](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200604225617997.png)

![image-20200604214831875](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200604214831875.png)

## 总结

以上就是使用Java编程的思想以操作对象的形式去操作数据库，省去大量sql的语法，提高了开发效率；但是只能操作单表，如果要写复杂的sql，和mybatis使用方式一样，在xml里面自定义即可。



项目代码见GitHub或者点击原文：[https://github.com/DogerRain/DemoFamily](https://github.com/DogerRain/DemoFamily)



参考：

[搭建SSM项目](https://mp.weixin.qq.com/s?__biz=MzAxNTc4ODYzOQ==&mid=2247483967&idx=1&sn=ef0ce1392636cdd5761a8f5c3c106de7&chksm=9bfffd0fac887419c2094c100e659893d6a700b8aae2dae67686d3682a8b9f3275887f0313ba&token=522750604&lang=zh_CN#rd)

[https://mp.baomidou.com/guide/wrapper.html](https://mp.baomidou.com/guide/wrapper.html)

[https://mp.baomidou.com/config/](https://mp.baomidou.com/config/)