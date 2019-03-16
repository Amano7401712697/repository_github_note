## 二.开发第一个应用程序

### 1.使用 Initializr  初始化一个Springboot的应用程序

### 2.Springboot 的项目结构

####1)Springboot 的基本项目结构:

![1551272868417](C:\Users\amano\AppData\Roaming\Typora\typora-user-images\1551272868417.png)

####2)Springboot项目中基本的文件

```java
XXXXApplication.java：应用程序的启动引导类（bootstrap class），也是主要
的Spring配置类。
application.properties：用于配置应用程序和Spring Boot的属性。
XXXXApplicationTests.java：一个基本的集成测试类。
```

### 3.Springboot的项目结构

####① 启动Springboot

​	**XXXXApplication.java** 中代码最初如下:

![1551273092166](C:\Users\amano\AppData\Roaming\Typora\typora-user-images\1551273092166.png)

​	**@SpringBootApplication**开启了Spring的组件扫描和Spring Boot的自动配置功能。实际
上， **@SpringBootApplication**将三个有用的注解组合在了一起。

- Spring的**@Configuration**：标明该类使用Spring基于Java的配置。虽然本书不会写太多
  配置，但我们会更倾向于使用基于Java而不是XML的配置。
- Spring的**@ComponentScan**：启用组件扫描，这样你写的Web控制器类和其他组件才能被
  自动发现并注册为Spring应用程序上下文里的Bean。本章稍后会写一个简单的Spring MVC
  控制器，使用@Controller进行注解，这样组件扫描才能找到它。
- Spring Boot 的 **@EnableAutoConfiguration** ： 这 个 不 起 眼 的 小 注 解 也 可 以 称 为
  @Abracadabra①，就是这一行配置开启了Spring Boot自动配置的魔力，让你不用再写成
  篇的配置了。 

#### ② 测试Spring Boot应用程序

​	Initializr 还 提 供 了 一 个 测 试 类 的 骨 架 **ReadingListApplicationTests.java** ， 可 以 基 于 它 为 你 的 应 用 程 序 编 写 测 试 。其代码最初如下: 

![1551273419991](C:\Users\amano\AppData\Roaming\Typora\typora-user-images\1551273419991.png)

