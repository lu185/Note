# Spring-Boot





## `pom.xml`配置

```xml
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
   
    
    <!-- ######################### 二选一 ######################## -->
    <!-- MyBatis 启动器 -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.3.2</version>
    </dependency>
    
    <!-- MyBatis-Plus -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.3.2</version>
    </dependency>
    
    <!-- ######################################################## -->
	
    <!-- 分页插件 -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
        <version>1.2.5</version>
    </dependency>
    
    <!-- Spring-Boot 单元测试启动器 -->
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



## 启动`Spring-Boot`应用

```java
@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```



## `Spring-Boot`整合`Spring-JDBC`

### 配置`datasource`

在 `application.properties` 中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.
```

### 测试

```java
@SpringBootTest
public class ApplicationTest {

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

### 配置`datasource`

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

查询所有和分页查询

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

### 启动时扫描`Mapper`接口所在包

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
    JdbcTemplate template;

    @Autowired
    UserMapper mapper;

    @Test
    public void test(){
        System.out.println(mapper.findAll());
    }

}
```

