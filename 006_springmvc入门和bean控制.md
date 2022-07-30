# springMVC入门

## springMVC入门

**1.在pom文件中引入springMVC需要的maven坐标**

```java
<!--1.导入springmvc与servlet-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
```

**2.使用注解模式开发，简化servlet的写法**

```java
//2.定义controller
//2.1使用@Controller定义bean
@Controller
public class UserController {
    //2.2设置当前操作的访问路径
    @RequestMapping("/save")
    //2.3设置当前操作的返回值类型
    @ResponseBody
    public String save(){
        System.out.println("user save...");
        return "{'module':'springmvc'}";
    }
}
```

**3.定义一个类，继承AbstractDispatcherServletInitializer，重写方法，加载springMVC容器配置**

```java
//4.定义一个servlet容器启动的配置类，在里面加载spring的配置
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    //加载springMVC容器配置
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }
    //设置哪些请求归属springMVC处理
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    //加载spring容器配置
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}

```

## bean加载控制

作用：防止springMVC的bean被spring加载

```java
@Configuration
//不加载springMVC的bean的两种方法：
//1.分开加载
//@ComponentScan({"com.lillanm.service","com.lillanm.dao"})
//2.排除
@ComponentScan(value = "com.lillanm",
    excludeFilters = @ComponentScan.Filter(
            type = FilterType.ANNOTATION,
            classes = Controller.class
    ))
public class SpringConfig {
}
```

以后就使用第一种方法，分开来加载，dao层使用Mybatis，不需要加载，但为了防止以后使用其他技术，这里写上也无妨

## servlet容器配置类的简化写法

继承AbstractAnnotationConfigDispatcherServletInitializer(最长的这个)，重写方法

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

这样只需要将spring和springMVC的配置类分开加载就行