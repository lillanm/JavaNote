# springmvc请求与响应

## 访问路径

该注解用来设置访问路径，用在具体的方法上，也可以在类上加该注解，访问时两者相加即可

```java

@Controller
@RequestMapping("/test")//访问路径先输入这一段
public class UserController {
    //普通参数：请求参数与形参名称对应即可完成参数传递
    @RequestMapping("/commonParam")//后输入这一段具体的方法路径
    @ResponseBody
    public String commonParam(String name ,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'common param'}";
    }
}
```

请求体使用@ResponseBody注解表示

```java
@Override//在Servlet容器里增加这段代码解决乱码问题
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
```



## 返回数据：

在@ResponseBody注解的方法下直接return即可

传递参数：

get、post请求的参数键名即为参数名，直接在具体的方法上写即可接收

@RequestParam:给参数格外的键名

@RequestBody：用来传递JSON字符串

**Date类型参数：需要自己设定格式，默认格式为yyyy/MM/dd，可以使用@DateTimeFormat注解自己配置格式**

```java
package com.lillanm.controller;

import com.lillanm.pojo.User;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.Arrays;
import java.util.Date;
import java.util.List;

@Controller
public class UserController {
    //普通参数：请求参数与形参名称对应即可完成参数传递
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name ,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'common param'}";
    }

    //普通参数：请求参数名与形参名不同时，使用@RequestParam注解关联请求参数名称与形参名称之间的关系
    @RequestMapping("/commonParamDifferentName")
    @ResponseBody
    public String commonParamDifferentName(@RequestParam("name") String userName , int age){
        System.out.println("普通参数传递 userName ==> "+userName);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'common param different name'}";
    }

    //POJO参数：请求参数与形参对象中的属性对应即可完成参数传递
    @RequestMapping("/pojoParam")
    @ResponseBody
    public String pojoParam(User user){
        System.out.println("pojo参数传递 user ==> "+user);
        return "{'module':'pojo param'}";
    }

    //嵌套POJO参数：嵌套属性按照层次结构设定名称即可完成参数传递
    @RequestMapping("/pojoContainPojoParam")
    @ResponseBody
    public String pojoContainPojoParam(User user){
        System.out.println("pojo嵌套pojo参数传递 user ==> "+user);
        return "{'module':'pojo contain pojo param'}";
    }

    //数组参数：同名请求参数可以直接映射到对应名称的形参数组对象中
    @RequestMapping("/arrayParam")
    @ResponseBody
    public String arrayParam(String[] likes){
        System.out.println("数组参数传递 likes ==> "+ Arrays.toString(likes));
        return "{'module':'array param'}";
    }

    //集合参数：同名请求参数可以使用@RequestParam注解映射到对应名称的集合对象中作为数据
    @RequestMapping("/listParam")
    @ResponseBody
    public String listParam(@RequestParam List<String> likes){
        System.out.println("集合参数传递 likes ==> "+ likes);
        return "{'module':'list param'}";
    }


    //集合参数：json格式
    //1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
    //2.使用@RequestBody注解将外部传递的json数组数据映射到形参的集合对象中作为数据
    @RequestMapping("/listParamForJson")
    @ResponseBody
    public String listParamForJson(@RequestBody List<String> likes){
        System.out.println("list common(json)参数传递 list ==> "+likes);
        return "{'module':'list common for json param'}";
    }


    //POJO参数：json格式
    //1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
    //2.使用@RequestBody注解将外部传递的json数据映射到形参的实体类对象中，要求属性名称一一对应
    @RequestMapping("/pojoParamForJson")
    @ResponseBody
    public String pojoParamForJson(@RequestBody User user){
        System.out.println("pojo(json)参数传递 user ==> "+user);
        return "{'module':'pojo for json param'}";
    }

    //集合参数：json格式
    //1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
    //2.使用@RequestBody注解将外部传递的json数组数据映射到形参的保存实体类对象的集合对象中，要求属性名称一一对应
    @RequestMapping("/listPojoParamForJson")
    @ResponseBody
    public String listPojoParamForJson(@RequestBody List<User> list){
        System.out.println("list pojo(json)参数传递 list ==> "+list);
        return "{'module':'list pojo for json param'}";
    }

    //日期参数
    //使用@DateTimeFormat注解设置日期类型数据格式，默认格式yyyy/MM/dd
    @RequestMapping("/dataParam")
    @ResponseBody
    public String dataParam(Date date,
                            @DateTimeFormat(pattern="yyyy-MM-dd") Date date1,
                            @DateTimeFormat(pattern="yyyy/MM/dd HH:mm:ss") Date date2){
        System.out.println("参数传递 date ==> "+date);
        System.out.println("参数传递 date1(yyyy-MM-dd) ==> "+date1);
        System.out
            .println("参数传递 date2(yyyy/MM/dd HH:mm:ss) ==> "+date2);
        return "{'module':'data param'}";
    }

}

```

## 响应

若没加注解@ResponseBody，就是跳转到对应页面，若加了，就是返回数据

响应的数据格式由该方法的返回值确定

```java
@Controller
public class UserController {

    //响应页面/跳转页面
    //返回值为String类型，设置返回值为页面名称，即可实现页面跳转
    @RequestMapping("/toJumpPage")
    public String toJumpPage(){
        System.out.println("跳转页面");
        return "page.jsp";
    }

    //响应文本数据
    //返回值为String类型，设置返回值为任意字符串信息，即可实现返回指定字符串信息，需要依赖@ResponseBody注解
    @RequestMapping("/toText")
    @ResponseBody
    public String toText(){
        System.out.println("返回纯文本数据");
        return "response text";
    }

    //响应POJO对象
    //返回值为实体类对象，设置返回值为实体类类型，即可实现返回对应对象的json数据，需要依赖@ResponseBody注解和@EnableWebMvc注解
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    public User toJsonPOJO(){
        System.out.println("返回json对象数据");
        User user = new User();
        user.setName("lill");
        user.setAge(15);
        return user;
    }

    //响应POJO集合对象
    //返回值为集合对象，设置返回值为集合类型，即可实现返回对应集合的json数组数据，需要依赖@ResponseBody注解和@EnableWebMvc注解
    @RequestMapping("/toJsonList")
    @ResponseBody
    public List<User> toJsonList(){
        System.out.println("返回json集合数据");
        User user1 = new User();
        user1.setName("lillanm");
        user1.setAge(15);

        User user2 = new User();
        user2.setName("lillanm2");
        user2.setAge(12);

        List<User> userList = new ArrayList<User>();
        userList.add(user1);
        userList.add(user2);

        return userList;
    }
}

```

## REST风格

访问资源时使用行为动作区分对资源进行了何种操作，简化了地址栏，如需要传递参数，则直接在地址栏加上数字列

查询全部信息：GET（查询）



