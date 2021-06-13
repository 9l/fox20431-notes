# 如何创建一个SpringMVC项目

## 1

配置`/main/webapp/WEB-INF/web.xml`

目的:拦截并分发所有请求,简化`web.xml`的代码,便于快速开发

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">

    <servlet>
        <servlet-name>spring-mvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:application-context.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring-mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- You can add other if you need. -->
</web-app>
```

## 2

创建`/main/resource/application-context.xml`

目的:通过`web.xml`配置信息可知,该文件作为Sping MVC的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven/>

    <!-- 使用这个注释可以请求静态资源 -->
    <mvc:default-servlet-handler/>

    <!-- controller类需要注册成bean -->
    <bean class="xxx"/>

</beans>
```

## 3

用IDEA检查项目结构(File->Project Structure),设置正确的`web.xml`,将生成的`war`包部署到tomcat即可完成所有操作.

## 其他摘录



- Dispatcher Servlet ——核心Servlet前置控制器，配置在web.xml文件中的。拦截匹配的请求，Servlet拦截匹配规则要自己定义，把拦截下来的请求，依据相应的规则分发到目标Controller来处理
- Controllers ——具体的业务控制器，处理具体请求的业务并响应
- View Resolvers ——视图解析器，用于将响应的逻辑视图解析为真正的视图View对象
- Views, Models ——Views的主要作用是用于处理响应视图，然后返回给客户端，Models主要用于传递控制方法处理数据到响应视图页面
- ModelAndView ——Model 和 View 的复合体
- Model and Session Attributes ——对模型属性和会话属性的处理

链接：https://juejin.im/post/6844903950609547277


### 流程

1. 用户发送请求至前端控制器DispatcherServlet；
2. DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handle；
3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有)一并返回给DispatcherServlet；
4. DispatcherServlet 调用 HandlerAdapter处理器适配器；
5. HandlerAdapter 经过适配调用 具体处理器(Handler，也叫后端控制器)；
6. Handler执行完成返回ModelAndView；
7. HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；
8. DispatcherServlet将ModelAndView传给ViewResolver视图解析器进行解析；
9. ViewResolver解析后返回具体View；
10. DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）
11. DispatcherServlet响应用户。

![sfasdf](./asset/procedure-flow-chart.png)