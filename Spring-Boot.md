# `Spring-Boot`

## `Spring-Boot`基础配置

### `pom.xml`配置

```html
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
	<!-- Spring-Boot Web 启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <!-- Spring-Boot 单元测试启动器 -->
	<dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### `application.properties`配置

```properties
# 配置 tomcat 端口
server.port=185

# 配置 上下文
server.servlet.context-path=/SpringBootApplicaiton
```

### 启动`Spring-Boot应用`

```java
@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```







## `Spring-Boot`整合`Spring-JDBC`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- Spring-JDBC 启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

### 配置数据源

在 `application.properties` 中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.
```

### 测试

基于`Spring-Boot-Test`进行测试

```java
@SpringBootTest
public class SpringJDBCTest {

    @Autowired
    JdbcTemplate template;
    
    static class User{
        private Integer userId;
        private String userName;
        
        // Add Getter and Setter ..
        
        // Override toString ..
    }
    
    static class UserMapper implements RowMapper<User>{
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            User user = new User();
            user.setUserId(rs.getInt("userId"));
            user.setUserName(rs.getString("userName"));
            return user;
        }
    }
    
    @Test
    public void findAll(){
        String SQL = "select * from user";
        List<User> data = template.query( SQL, new UserMapper() );
		System.out.println( data );
    }
   
    @Test
    public void add(){
        String SQL = "insert into user(userName) value(?)"；
        template.update(SQL,"root");
        this.findAll();
    }
}
```







## `Spring-Boot`整合`MyBatis`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- MyBatis 启动器 -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>

<!-- MyBatis 分页插件 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.5</version>
</dependency>
```

### 配置数据源

在 `application.properties` 中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.
```

### 创建实体类

```java
package com.entitys;

public class User{
    private Integer userId;
    private String userName;
    
    // Add Getter and Setter ..
        
    // Override toString ..
}
```

### 创建`Mapper`接口

```java
package com.mappers;

public interface UserMapper {
    @Select("select * from `user`")
    public List<User> findAll();
}
```

### 创建控制器

查询所有 和 分页查询

```java
package com.contollers;

@RestController
public class UserManage {

    @Autowired
    UserMapper userMapper;

    // 查询所有
    @RequestMapping("findAll")
    public List<User> findAll(){
        return userMapper.findAll();
    }
    
    // 分页查询
    @RequestMapping("findByPage")
    public List<User> findByPage(Integer page,Integer limit){
        
        // 设置 页数 和 显示的条数
        PageHelper.startPage(page, limit);
        
        List<User> data = userMapper.findAll();
        
        // PageInfo 是 PageHelper 的内置的分页信息类
        PageInfo info= new PageInfo(data);
        return data; 
    }
}
```

### 启动时扫描`Mapper`接口

```java
@SpringBootApplication
@MapperScan("com.mappers")
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

### 基于 `Spring-Boot-Test` 测试

```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class ApplicationTest {
    @Autowired
    UserMapper mapper;

    @Test
    public void test(){
        System.out.println(mapper.findAll());
    }
}
```







## `Spring-Boot`整合`MyBatis-Plus`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- MyBatis-Plus 启动器 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
</dependency>
```

### 配置数据源

在`application.properties`中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.

# 启用驼峰式命名法
mybatis-plus.configuration.map-underscore-to-camel-case=false
```

### 创建实体类

```java
package com.entitys;

public class User{
    private Integer userId;
    private String userName;
    
    // Add Getter and Setter ..
        
    // Override toString ..
}
```

### 创建`Mapper`接口

让`Mapper`接口继承于`BaseMapper`接口

```java
package com.mappers;

@Mapper
public interface UserMapper extends BaseMapper<User>{
    
}
```

### 创建控制器

```java
package com.controllers;

@RestController
public class UserInfoManage{
    
    @Autowired
    UserMapper userMapper;

    @RequestMapping("findAll")
    public List<User> findAll(){
        return userInfoMapper.selectByMap(null);
    }
}
```

### 启动时扫描`Mapper`接口

```java
@SpringBootApplication
@MapperScan("com.mappers")
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```







## `Spring-Boot`整合`JSP`

### `pom.xml`配置

```xml
<!-- jstl标签 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
<!-- jasper -->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
```

### 配置视图解析器

```properties
spring.mvc.view.prefix=/WEB-INF/JSP/
spring.mvc.view.suffix=.jsp
```

### 创建`webapp`

在`main`文件夹下创建`webapp`

```
-- main
    +- webapp
        +- WEB-INF
            += JSP
                += index.jsp
            += web.xml
```

### 编辑`index.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>hello ${name}</h1>
</body>
</html>
```

### 创建控制器

```java
package com.contollers;

@Controller
public class SayHello {

    @RequestMapping("/hello/{name}")
    public String hello(ModelAndView mav, @PathVariable String name){
        mav.addObject("name",name);
        return "index";
    }
}
```

### 启动

```java
package com;

@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```





# `MyBatis-Plus`

## 代码生成器

```java
package com.utils;

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import java.util.Scanner;

public class MyBatisPlusGenerator {

    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("Five");
        gc.setOpen(false);
        mpg.setGlobalConfig(gc);

        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/test?useUnicode=true&useSSL=false&characterEncoding=utf8");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("1=23.");
        mpg.setDataSource(dsc);

        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.test");
        mpg.setPackageInfo(pc);

        TemplateConfig templateConfig = new TemplateConfig();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);

        strategy.setSuperEntityColumns("id");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.execute();
    }
}

```





