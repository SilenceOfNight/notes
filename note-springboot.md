# Springboot

## 使用.yml文件作为配置文件，可以简化配置。
例子：
```
server:
  port: 8082
  context-path: /demo2
```
【注意】：
1. 使用冒号（:）作为分割符
2. 冒号之后，必须跟空白符。
3. 注意缩进

在配置文件中自定义属性
在application.yml中自定义属性customizedAttribute，如下所示：
```
server:
  port: 8080
  context-path: /

customizedAttribute: customizedAttributeValue
```
在HelloWorldContainer中引用自定义属性customizedAttribute
```
package com.cyz.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Created by Z on 2017/4/15.
 */
@RestController
public class HelloWorldController {
    @Value("${customizedAttribute}") String customizedAttribute;

    @RequestMapping(value="/hello", method = RequestMethod.GET)
    public String sayHello(){
        return customizedAttribute;
    }
}

```
甚至：
```
server:
  port: 8080
  context-path: /

customizedAttribute: customizedAttributeValue
customizedAttribute2: customizedAttributeValue2
customizedAttribute3: "customizedAttribute: ${customizedAttribute}, customizedAttribute2: ${customizedAttribute}"

customized:
  attribute: value

```
```
package com.cyz.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Created by Z on 2017/4/15.
 */
@RestController
public class HelloWorldController {
    @Value("${customizedAttribute3}") String customizedAttribute;

    @RequestMapping(value="/hello", method = RequestMethod.GET)
    public String sayHello(){
        return customizedAttribute;
    }
}

```

引入：
``` pom.xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

``` application.yml
server:
  port: 8080
  context-path: /

customizedAttribute: customizedAttributeValue
customizedAttribute2: customizedAttributeValue2
customizedAttribute3: "customizedAttribute: ${customizedAttribute}, customizedAttribute2: ${customizedAttribute}"

customized:
  attribute: value
  attribute2: value2

```

``` CustomizedProperties.java
package com.cyz.constant;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/**
 * Created by Z on 2017/4/15.
 */
@Component
@ConfigurationProperties(prefix = "customized")
public class CustomizedProperties {
    private String attribute;

    private String attribute2;

    public String getAttribute() {
        return attribute;
    }

    public void setAttribute(String attribute) {
        this.attribute = attribute;
    }

    public String getAttribute2() {
        return attribute2;
    }

    public void setAttribute2(String attribute2) {
        this.attribute2 = attribute2;
    }

    @Override
    public String toString() {
        return "CustomizedProperties{" +
                "attribute='" + attribute + '\'' +
                ", attribute2='" + attribute2 + '\'' +
                '}';
    }
}

```

``` HelloWorldController.java
package com.cyz.controller;

import com.cyz.constant.CustomizedProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Created by Z on 2017/4/15.
 */
@RestController
public class HelloWorldController {
    @Value("${customizedAttribute3}") private String customizedAttribute;

    @Autowired
    private CustomizedProperties customizedProperties;

    @RequestMapping(value="/hello", method = RequestMethod.GET)
    public String sayHello(){
        return customizedProperties.toString();
    }
}

```

## 开发环境（dev）与生产环境（prod）配置分离
``` application.yml
spring:
  profiles:
    active: prod
```

``` application-dev.yml
server:
  port: 8080
  context-path: /

customizedAttribute: customizedAttributeValue

customized:
  attribute: value1
  attribute2: value2

```

``` application-prod.yml
server:
  port: 8081
  context-path: /spring-demo

customizedAttribute: customizedAttributeValue

customized:
  attribute: value1
  attribute2: value2
```


## Controller
|@Controller     | 处理http请求|
|@RestController | Spring4之后新加的注解。组合注解，相当于@Controller + @ResponseBody|
|@RequestMapping | 配置url映射

### Controller
``` pom.xml
<!-- SpringBoot的默认模板 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

``` HelloController.java
package com.cyz.controller;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Created by Z on 2017/4/15.
 */
@Controller
public class HelloController {

    @RequestMapping(value="/hi", method = RequestMethod.GET)
    public String sayHello() {
        System.out.println("hi");
        return "hello";
    }
}

```

``` resource/templates/hello.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Springboot-Demo</title>
</head>
<body>
    <h1 class="title">Hello, this is a demo of SpingBoot.</h1>
</body>
</html>
```

【问题】
spring.thymeleaf.mode的默认值是HTML5，其实是一个很严格的检查。比如<meta charset="UTF-8" />，如果少最后的标签封闭符号/，就会报错而转到错误页。
【解决方案】
spring.thymeleaf.mode修改成：LEGACYHTML5。需要注意的是，LEGACYHTML5需要搭配一个额外的库NekoHTML才可用。
``` application.yml
spring:
  profiles:
    active: dev
  thymeleaf:
    mode: LEGACYHTML5
```
``` pom.xml
<dependency>
  <groupId>net.sourceforge.nekohtml</groupId>
  <artifactId>nekohtml</artifactId>
</dependency>
```

引入thymeleaf以后，可以将spring-boot-starter-web删掉。因为thymeleaf已经包含了它。
``` pom.xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### RequestMapping
多路径请求同一个Controller
```
package com.cyz.controller;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Created by Z on 2017/4/15.
 */
@Controller
public class HelloController {

    @RequestMapping(value={"/hi", "sayHi"}, method = RequestMethod.GET)
    public String sayHello() {
        System.out.println("hi");
        return "hello";
    }
}

```

PS: @RequestMapping 不指定method时，默认任何类型的请求都会响应。存在安全风险，故应该指定method。


|@PathVariable |获取url中的参数 |
|@RequestParam |获取请求中的参数|

组合注解
|@GetMapping   |@RequestMapping(method = RequestMethod.GET)   |
|@PostMapping  |@RequestMapping(method = RequestMethod.POST)  |
|@PutMapping   |@RequestMapping(method = RequestMethod.PUT)   |
|......        |......                                        |



Spring-Data-JPA（对Hibernate的整合）
JPA(Java Persistence API)定义了一系列对象持久化的标准，目前实现这一规范的产品有Hibernate、TopLink等。

导入相应的依赖
``` pom.xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- MySQL 的驱动包，其他数据库需要使用另外的依赖 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
</dependency>
```

### JpaRepository