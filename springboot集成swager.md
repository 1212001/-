## springboot集成swager

### 1、添加依赖

```
<!-- 4、集成swagger -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

### 2、配置swagger配置文件

```java
package com.ghostcloud.config.swagger;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;


/**
 * @description: Swagger配置类
 * @author: anson
 * @Date: 2019/9/3 22:52
 * @version: 1.0
 */

@Configuration
@EnableSwagger2
public class SwaggerConfig
{
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                //swagger要扫描的包路径
                .apis(RequestHandlerSelectors.basePackage("com.ghostcloud.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("springboot Swagger 测试")
                .version("1.0")
                .build();
    }
}
```

### 3、controller接口，注解

```java
package com.ghostcloud.controller;

import com.ghostcloud.entity.UserTableEntity;
import com.ghostcloud.service.UserTableService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
@Api(value = "用户接口")	//接口描述
@Slf4j
@RestController
@RequestMapping("/user")
public class HelloController {
    @Autowired
    private UserTableService userTableService;
    @ApiOperation(value = "获取用户", notes = "查询用户信息")
    @GetMapping("/getUser")
    public List<UserTableEntity> getUser(UserTableEntity userTableEntity){
        log.info("asd");
        log.info("我是slf4j的日志");
        List<UserTableEntity> userTable = userTableService.getUserTable(userTableEntity);
        return userTable;
    }
}

```

### 4、访问localhost:8080/swagger-ui.html查询接口