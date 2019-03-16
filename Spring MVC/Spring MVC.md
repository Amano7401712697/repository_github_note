# Spring MVC

## Spring MVC概述

- Spring 为展现层提供的基于 MVC 设计理念的优秀的 Web 框架
- Spring MVC 通过一套 MVC 注解，让 POJO 成为处理请 求的控制器，而无须实现任何接口
- 支持 REST 风格的 URL 请求
- 采用了松散耦合可插拔组件结构，比其他 MVC 框架更具 扩展性和灵活性



## Spring MVC的hello world

### 环境搭建

1. 导入jar包,或通过Maven管理依赖

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.0.9.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.0.9.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.0.9.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.0.9.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.0.9.RELEASE</version>
		</dependency>
</dependencies>
<!-- tomcat插件 -->
<build>
		<plugins>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
				<configuration>
<!-- 项目访问路径 本例：localhost:9090, 如果配置的aa，则访问路径为localhost:9090/aa -->
					<path>/springmvc</path> 
					<port>8080</port>
					<uriEncoding>UTF-8</uriEncoding>
				</configuration>
			</plugin>
		</plugins>
	</build>
```

2. 配置web.xml

```xml
<servlet>
    <!-- 中央拦截器DispatcherServlet -->
    <servlet-name>springDispatcherServlet</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <!--DispatcherServlet的初始化参数,配置加载springmvc配置文件的位置-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:/spring-mvc.xml</param-value>
    </init-param>
    <!--启动时加载-->
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

3. 配置Spring mvc配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">
	
	<mvc:annotation-driven></mvc:annotation-driven>
	<!--配置自动扫描的包名-->
	<context:component-scan base-package="com.wfdev.springmvc"/>
	<!--配置视图解析器,将controller返回值映射为物理视图-->
	<bean name="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
</beans>

```

3. 编写Controller

```java
package com.wfdev.springmvc.controllers;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
//
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public String hello() {
		return "success";
	}
}
```

4. 编写视图
5. 测试

### 要点

1. 使用 @RequestMapping 注解来映射请求的URL

2. 返回值会通过视图解析器解析为实际的物理视图, 对于 InternalResourceViewResolver 视图解析器, 会做如下的解析:通过 prefix + returnVal + 后缀 这样的方式得到实际的物理视图, 然会做转发操作

## Spring MVC注解

###@RequestMapping 注解

1. Spring MVC 使用 @RequestMapping 注解为控制器指定可 以处理哪些 URL 请求.

2. **@RequestMapping** 除了修饰方法, 还可来修饰类 
   - 类定义处: 提供初步的请求映射信息。相对于 WEB 应用的根目录

   - 方法处: 提供进一步的细分映射信息。 相对于类定义处的 URL。若类定义处未标注 @RequestMapping，则方法处标记的 URL,相对于 WEB 应用的根目录

3. @RequestMapping 除了可以使用请求 URL 映射请求外， 还可以使用请求方法、请求参数及请求头映射请求
   - value : 请求URL
   - method : 请求方法
   - params : 请求参数
   - deads : 请求头

### @PathVariable 注解

​	带占位符的 URL 是 Spring3.0 新增的功能,通过 **@PathVariable** 可以将 URL 中占位符参数绑定到控制器处理方法的入参中：URL 中的 **{xxx}** 占位符可以通过 @PathVariable("xxx") 绑定到操作方法的入参中.

```java
@RequestMapping("/hello/{username}")
public String testPathVariable(@PathVariable String username) {
    System.out.println(username);
    return "success";
}
```

### @RequestParam 注解

​	在处理方法入参处使用 @RequestParam 可以把请求参 数传递给请求方法

| 属性名       |                                                              |
| ------------ | ------------------------------------------------------------ |
| value        | 请求参数名                                                   |
| required     | 是否必须。默认为 true, 表示请求参数中必须包含对应 的参数，若不存在，将抛出异常 |
| defaultValue | 请求参数默认值                                               |

### @CookieValue 注解

​	**@CookieValue** 可让处理方法入参绑定某个 Cookie 值

### @RequestHeader 注解

​	请求头包含了若干个属性，服务器可据此获知客户端的信 息，通过 **@RequestHeader** 即可将请求头中的属性值绑 定到处理方法的入参中



## REST

###REST简介

​	REST：即 Representational State Transfer。（资源）表现层状态转化。是目前 最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便， 所以正得到越来越多网站的采用

-  资源（Resources）：网络上的一个实体，或者说是网络上的一个具体信息。它 可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。 可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的 URI 。要 获取这个资源，访问它的URI就可以，因此 URI 即为每一个资源的独一无二的识 别符。
- 表现层（Representation）：把资源具体呈现出来的形式，叫做它的表现层 （Representation）。
- 状态转化（State Transfer）：每发出一个请求，就代表了客户端和服务器的一 次交互过程。HTTP协议，是一个无状态协议，即所有的状态都保存在服务器 端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生“ 状态转化”（State Transfer）。而这种转化是建立在表现层之上的，所以就是 “ 表现层状态转化”。具体说，就是 HTTP 协议里面，四个表示操作方式的动 词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET 用来获 取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。

### Spring MVC如何发送REST请求

1. 在Web.xml中注册过滤器**org.springframework.web.filter.HiddenHttpMethodFilter**

   HiddenHttpMethodFilter：浏览器 form 表单只支持 GET 与 POST 请求，而DELETE、PUT 等 method 并不支 持，Spring3.0 添加了一个过滤器，可以将这些请求转换 为标准的 http 方法，使得支持 GET、POST、PUT 与 DELETE 请求。

   ```xml
   <filter>
   		<filter-name>HiddenHttpMethodFilter</filter-name>
   		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
   	</filter>
   	<filter-mapping>
   		<filter-name>HiddenHttpMethodFilter</filter-name>
   		<url-pattern>/*</url-pattern>
   	</filter-mapping>
   ```

   

2. 在页面表中中添加隐藏域,name为**_method**, value为需要转化的请求类型

   ```xml
   <!-- 测试put delete 请求 -->
   	<form action="/hello/1">
   		<input type="hidden" name="_method" value="DELETE">
   		<input type="submit" value="submit">
   	</form>
   ```

## Spring MVC接收参数

### 使用 POJO 对象绑定请求参数值

​	Spring MVC 会按请求参数名和 POJO 属性名进行自动匹 配，自动为该对象填充属性值。支持级联属性。

```xml
<!-- 测试pojo传参 -->
<form action="hello/testPojo">
    <input type="text" name="username"/>
    <input type="password" name="password"/>
    <input type="submit" value="submit"/>
</form>
```

```java
@RequestMapping("/hello/testPojo")
public String testPojo(User user) {
    System.out.println(user);
    return "success";
}
```

### 使用 Servlet API 作为入参

- HttpServletRequest
- HttpServletResponse
- HttpSession
- java.security.Principal
- Locale 
- InputStream
- OutputStream
- Reader
- Writer

