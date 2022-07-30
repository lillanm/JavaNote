# Spring依赖注入

首先在config包下定义一个SpringConfig类用@Configuration注解作为spring容器，

用@ComponentScan("")里面写需要扫描包的范围

**对于实体类的装配：**

使用@Autowired注解

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;


    @Override
    public void save() {
        System.out.println("bookService");
        bookDao.save();
    }
}
```

**对于实体类里面的属性，可以使用@Value()注解配置**、

也可以加载properties资源文件中的内容，先使用PropertySource()注解，里面写加载的资源文件的名称，再在@Value(${})括号内填键名即可使用properties文件

```java
@Repository
@PropertySource("jdbc.properties")
public class BookDaoImpl implements BookDao {
    @Value("${name}")
    private String name;
    @Override
    public void save() {
        System.out.println("bookDao..."+name);
    }
}
```



**第三方bean管理**

1.在config包下定义专门的类存放，使用@Bean注解，即可在其他地方使用该类

```java
@PropertySource("jdbc.properties")//引入外部properties文件
public class JdbcConfig {
    //获取第三方bean对象
    //1.定义一个方法获得要管理的对象
    //2.添加@Bean，表示当前方法的返回值是一个bean
    @Value("${driverClassName}")
    private String driverClassName;
    @Bean
    //对第三方注入需要在函数体里面声明
    public DataSource dataSource(BookDao bookDao) {
        System.out.println(bookDao);
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driverClassName);
        ds.setUrl("jdbc:mysql://localhost");
        ds.setUsername("root");
        ds.setPassword("666");
        return ds;
    }
}
```

在拥有@Configuration注解类的下方，使用@Import({JdbcConfig.class})注解，{}内支持使用数组的形式导入第三方Bean

