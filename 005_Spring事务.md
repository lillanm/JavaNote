# Spring事务

1.实现接口PlatformTransactionManager，在里面创建DataSourceTransactionManager对象，将DataSource对象串进去

```java
 //配置事务管理器，mybatis使用的是jdbc事务
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
```

2.在SpringConfig里增加注解@EnableTransactionManagement，开启注解事务驱动

3.在需要开启事物的Service层接口或者具体的方法上增加注解@Transactional

## Spring事务角色

*事务角色：

​	*事务管理员：发起事务方，在Spring中通常指代业务层开启事物的方法

​	*事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法



```java
//propagation设置事务属性：传播行为设置为当前操作需要新事务
@Transactional(propagation = Propagation.REQUIRES_NEW)
//让当前事务和其他事务不绑定到一起
```

