# Spring

## IOC（控制反转）
控制反转，控制权的转移，应用程序本身不负责依赖对象的创建和维护，而是由外部容器负责。DI（Dependency Injection，依赖注入。由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中）是其一种实现方式。
目的：创建对象并组装对象之间的关系

### Bean容器初始化
依赖：
  -org.springframework.beans
  -org.springframework.context
相关重要的类：
  -BeanFactory提供配置结构和基础功能，加载并初始化Bean
  -ApplicationContext保存了Bean对象并在Srping中被广泛使用

1. ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("classpath*:spring-*.xml");
2. FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext("D:\Buffere\application.xml");
3. Web应用中依赖Servlet或者Listener
   1)
   <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listener>
   2)
   <servlet>
    <servlet-name>context</servlet>
    <servlet-class>org.springframework.web.context.ContextLoaderServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
   </servlet>