#Swagger #knife4j

## 项目集成 Swagger

#### Swagger 依赖:
```pom

<!--swagger-->  
<dependency>  
    <groupId>io.springfox</groupId>  
    <artifactId>springfox-swagger2</artifactId>  
    <!-- <version>2.7.0</version> -->
</dependency>  
<dependency>  
    <groupId>io.springfox</groupId>  
    <artifactId>springfox-swagger-ui</artifactId>
    <!-- <version>2.7.0</version> -->
</dependency>
```

#### Swagger 配置类
  ```java
  
import com.google.common.base.Predicates;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import springfox.documentation.builders.ApiInfoBuilder;  
import springfox.documentation.builders.ParameterBuilder;  
import springfox.documentation.builders.PathSelectors;  
import springfox.documentation.schema.ModelRef;  
import springfox.documentation.service.ApiInfo;  
import springfox.documentation.service.Contact;  
import springfox.documentation.service.Parameter;  
import springfox.documentation.spi.DocumentationType;  
import springfox.documentation.spring.web.plugins.Docket;  
import springfox.documentation.swagger2.annotations.EnableSwagger2;  
  
import java.util.ArrayList;  
import java.util.List;  
  
/**  
 * Swagger2配置信息  
 */  
@Configuration  
@EnableSwagger2  
public class Swagger2Config {  

	// 前台用系统
    @Bean  
    public Docket webApiConfig(){  
  
        //添加head参数start  
        List<Parameter> pars = new ArrayList<>();  
        ParameterBuilder tokenPar = new ParameterBuilder();  
        tokenPar.name("userId")  
                .description("用户ID")  
                .defaultValue("1")  
                .modelRef(new ModelRef("string"))  
                .parameterType("header")  
                .required(false)  
                .build();  
        pars.add(tokenPar.build());  
  
        ParameterBuilder tmpPar = new ParameterBuilder();  
                tmpPar.name("userTempId")  
                .description("临时用户ID")  
                .defaultValue("1")  
                .modelRef(new ModelRef("string"))  
                .parameterType("header")  
                .required(false)  
                .build();  
        pars.add(tmpPar.build());  
        //添加head参数end  
  
        return new Docket(DocumentationType.SWAGGER_2)  
                .groupName("webApi")  
                .apiInfo(webApiInfo())  
                .select()  
                //过滤掉admin路径下的所有页面  
                .paths(Predicates.and(PathSelectors.regex("/api/.*")))  
                //过滤掉所有error或error.*页面  
                //.paths(Predicates.not(PathSelectors.regex("/error.*")))  
                .build()  
                .globalOperationParameters(pars);  
  
    }  

	// 后台管理系统
    @Bean  
    public Docket adminApiConfig(){  
  
        return new Docket(DocumentationType.SWAGGER_2)  
                .groupName("adminApi")  
                .apiInfo(adminApiInfo())  
                .select()  
                //只显示admin路径下的页面  
                .paths(Predicates.and(PathSelectors.regex("/admin/.*")))  
                .build();  
  
    }  
  
    private ApiInfo webApiInfo(){  
  
        return new ApiInfoBuilder()  
                .title("网站-API文档")  
                .description("本文档描述了网站微服务接口定义")  
                .version("1.0")  
                .contact(new Contact("#联系人#", "http://#主页#.com", "#邮箱#@qq.com"))    
                .build();  
    }  
  
    private ApiInfo adminApiInfo(){  
  
        return new ApiInfoBuilder()  
                .title("后台管理系统-API文档")  
                .description("本文档描述了后台管理系统微服务接口定义")  
                .version("1.0")  
                .contact(new Contact("#联系人#", "http://#主页#.com", "#邮箱#@qq.com"))    
                .build();  
    }  
  
  
}
```










## 项目集成 knife4j

#### knife4j 依赖 
```pom

<!--knife4j 增强版wagger -->
<dependency>  
    <groupId>com.github.xiaoymin</groupId>  
    <artifactId>knife4j-spring-boot-starter</artifactId>  
    <!-- <version>2.0.8</version> -->
</dependency>
```

#### knife4j 配置类
```java
  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import springfox.documentation.builders.ApiInfoBuilder;  
import springfox.documentation.builders.ParameterBuilder;  
import springfox.documentation.builders.PathSelectors;  
import springfox.documentation.builders.RequestHandlerSelectors;  
import springfox.documentation.schema.ModelRef;  
import springfox.documentation.service.ApiInfo;  
import springfox.documentation.service.Contact;  
import springfox.documentation.service.Parameter;  
import springfox.documentation.spi.DocumentationType;  
import springfox.documentation.spring.web.plugins.Docket;  
import springfox.documentation.swagger2.annotations.EnableSwagger2WebMvc;  
  
import java.util.ArrayList;  
import java.util.List;  
  
/**  
 * knife4j配置信息  
 */  
@Configuration  
@EnableSwagger2WebMvc  
public class Knife4jConfig {  
  
    @Bean  
    public Docket adminApiConfig(){  
        List<Parameter> pars = new ArrayList<>();  
        ParameterBuilder tokenPar = new ParameterBuilder();  
        tokenPar.name("token")  
                .description("用户token")  
                .defaultValue("")  
                .modelRef(new ModelRef("string"))  
                .parameterType("header")  
                .required(false)  
                .build();  
        pars.add(tokenPar.build());  
        //添加head参数end  
  
        Docket adminApi = new Docket(DocumentationType.SWAGGER_2)  
                .groupName("adminApi")  
                .apiInfo(adminApiInfo())  
                .select()  
                //只显示admin路径下的页面  
                .apis(RequestHandlerSelectors.basePackage("com.atguigu"))  
                .paths(PathSelectors.regex("/admin/.*"))  
                .build()  
                .globalOperationParameters(pars);  
        return adminApi;  
    }  
  
    private ApiInfo adminApiInfo(){  
        return new ApiInfoBuilder()  
                .title("后台管理系统-API文档")  
                .description("本文档描述了后台管理系统微服务接口定义")  
                .version("1.0")  
                .contact(new Contact("qy", "http://atguigu.com", "493290402@qq.com"))  
                .build();  
    }  
}
```