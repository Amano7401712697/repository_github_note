# WebService 攻略

## WebService 简介

## WebService相关概念

## WebService的hello world

1.  创建一个Java项目或者Web项目

![1543476626615](C:\Users\weif\AppData\Roaming\Typora\typora-user-images\1543476626615.png)

2.  编写一个用于定义服务的接口,以==@javax.jws.WebService==注解修饰,表明该接口是一个Webservice接口

```java
package com.wfdev.ws.service.inter;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public interface IWebServie {
    @WebMethod
    String sayHello(String name);

    @WebMethod
    String save(String name, String passwd);

    @WebMethod
    String delete(String name);

    @WebMethod
    String update(String name, String passwd);

    @WebMethod
    String get(String name);
}
```

3.  创建WebServiec接口的实现类,以==@javax.jws.WebService==注解修饰,完成业务逻辑.

```java
package com.wfdev.ws.service.impl;

import com.wfdev.ws.service.inter.IWebServie;

import javax.jws.WebService;
import java.util.Map;

@WebService
public class CWebServiceImpl implements IWebServie {
    public static Map<String, String> user;

    public static void setUser(Map<String, String> user) {
        CWebServiceImpl.user = user;
    }

    public static Map<String, String> getUser() {
        return user;
    }

    @Override
    public String sayHello(String name) {
        System.out.println(name + " has request this server!");
        return "hello " + name;
    }

    @Override
    public String save(String name, String passwd) {
        if ("".equals(name) && name != null & "".equals(passwd) && passwd != null) {
            user.put(name, passwd);
            return name + " save successed";
        } else {
            return "save failed";
        }

    }

    @Override
    public String delete(String name) {
        if ("".equals(name) && name != null) {
            user.remove(name);
            return "delete successed";
        } else {
            return "delete failed";
        }
    }

    @Override
    public String update(String name, String passwd) {
        if ("".equals(name) && name != null & "".equals(passwd) && passwd != null) {
            user.put(name, passwd);
            return name + " update successed";
        } else {
            return "save failed";
        }
    }

    @Override
    public String get(String name) {
        if ("".equals(name) && name != null) {
            return "username :" + name + "passwd :" + user.get(name);
        } else {
            return "no user as " + name;
        }
    }
}
```

3.  发布WebService

-   如果是Java项目则可以通过定义一个==PublishWS==类,通过main方法发布WebService服务

```java
package com.wfdev.wf.test;

import com.wfdev.ws.service.impl.CWebServiceImpl;

import javax.xml.ws.Endpoint;

public interface PublishServer {
    public static void main(String[] args) {
        //发布WebService的地址
        String address = "http://192.168.42.72:9998/WS_Server/Webservice";
        //发布WebService服务,传入地址以及服务实现类
        Endpoint.publish(address , new CWebServiceImpl());
        System.out.println("Server is running");
    }
}
```

-    如果是Web项目,可以通过监听器发布WebService服务

```java
package me.gacl.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import javax.xml.ws.Endpoint;
import me.gacl.ws.WebServiceImpl;

/**
 * @author gacl
 * 用于发布WebService的监听器
 */
//使用Servlet3.0提供的@WebListener注解将实现了ServletContextListener接口的WebServicePublishListener类标注为一个Listener
@WebListener
public class WebServicePublishListener implements ServletContextListener {

    @Override
    public void contextDestroyed(ServletContextEvent sce) {

    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //WebService的发布地址
        String address = "http://192.168.1.100:8080/WS_Server/WebService";
        //发布WebService，WebServiceImpl类是WebServie接口的具体实现类
        Endpoint.publish(address , new WebServiceImpl());
        System.out.println("使用WebServicePublishListener发布webservice成功!");
    }
}
```



-   或者通过Serverlet发布WebService服务

```java
package me.gacl.web.controller;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.xml.ws.Endpoint;
import me.gacl.ws.WebServiceImpl;

/**
 * @author gacl
 * 用于发布WebService的Servlet
 */
//使用Servlet3.0提供的@WebServlet注解将继承HttpServlet类的普通Java类标注为一个Servlet
//将value属性设置为空字符串，这样WebServicePublishServlet就不提供对外访问的路径
//loadOnStartup属性设置WebServicePublishServlet的初始化时机
@WebServlet(value="",loadOnStartup=0)
public class WebServicePublishServlet extends HttpServlet {

    /* (non-Javadoc)
     * @see javax.servlet.GenericServlet#init()
     * 在WebServicePublishServlet初始化时发布WebService
     */
    public void init() throws ServletException {
        //WebService的发布地址
        String address = "http://192.168.1.100:8888/WebService";
        //发布WebService，WebServiceImpl类是WebServie接口的具体实现类
        Endpoint.publish(address , new WebServiceImpl());
        System.out.println("使用WebServicePublishServlet发布webservice成功!");
    }
}
```

## 调用WebService服务

1.  通过==JDK==中的==wsimport==获取客户端所需要的代码

    通过命令行输入

```cmd
wsimport -s (需要调用服务的项目的源码路径) -keep (WebService服务发布的地址)
```

2.  获取相关代码

![1543477786246](C:\Users\weif\AppData\Roaming\Typora\typora-user-images\1543477786246.png)

3.  编写测试类

```java
package com.wfdev.ws.test;

import com.wfdev.ws.service.impl.CWebServiceImpl;
import com.wfdev.ws.service.impl.CWebServiceImplService;

public class TestWS {
    public static void main(String[] args) {
        //WebService实例工厂,用于获取Service实例
        CWebServiceImplService factory = new CWebServiceImplService();
        //Service实例
        CWebServiceImpl ws = factory.getCWebServiceImplPort();
        String result = ws.sayHello("卫峰");
        System.out.println(result);
        result = ws.save("卫峰","123456");
        System.out.println(result);
        result = ws.update("卫峰","abcdef");
        System.out.println(result);
        result = ws.get("卫峰");
        System.out.println(result);
        result = ws.delete("卫峰");
        System.out.println(result);
        result = ws.get("卫峰");
        System.out.println(result);
    }
}

```



