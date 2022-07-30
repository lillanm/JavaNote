# Spring中@Component注解的使用

@component是Spring中的一个注解，用来实现bean的注入

使用注解可以代替配置文件中的内容，在容器内初始化bean

**web开发提供三个@Component注解的衍生注解，功能一样，仅仅名字不同，用来区分不同层的架构**

@Controller("")：表现层业务bean

@Service("")：业务层bean

@Repository("")：数据层bean

括号里写名字，以后可以用来获取bean

------

@Scope标签控制bean的设计模式：@Scope("singleton")：单例设计模式，

@Scope("prototype")：每次创建对象的时候new一个新的对象



------

**使用注解对bean的初始摧毁进行控制**

@PostConstruct：在这里定义函数执行init方法

@PreDestroy：在这里定义函数执行destroy方法

------

@Autowired：自动根据类型注入

```java
@Component//这是一个需要被管理的bean
public class BookServiceImpl implements BookService {
    @Autowired //自动装配   
    private BookDao bookDao;
    @Override
    public void save() {
        System.out.println("service");
    }
}
```

**如果有两个实现类实现同一个接口，使用@Qualifier注解加名称指定实现类**

要想自动执行init和destroy函数，需要在主函数中配置ShutdownHook，见下代码

```java
public class AppForAnnotation {
    public static void main(String[] args) {

        AnnotationConfigApplicationContext ctx =new AnnotationConfigApplicationContext(SpringConfig.class);
        ctx.registerShutdownHook();//有这行代码才有前面注解下init和destroy函数的执行
        //获取IOC容器里面的bean
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        System.out.println(bookDao);
        BookService bookService = ctx.getBean(BookService.class);
        System.out.println(bookService);
    }
}
```

