#Openfeign #feign

#### 依赖
```pom

<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
    <!-- <version>2.3.6.RELEASE</version> -->
</dependency>  
  
<!-- 服务调用feign -->  
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-starter-openfeign</artifactId>
	<!-- <version>2.2.5.RELEASE</version> -->
    <scope>provided</scope>  
</dependency>
```

#### FeignClient远程调用接口
```java
  
@FeignClient(value ="service-product", fallback = ProductDegradeFeignClient.class)  
public interface ProductFeignClient {  
  
    /**  
     * 远程调用具体接口
     * @return  
     */  
    @GetMapping("/#调用地址(别忘记拼接上RequestMapping里的)/......")  
    public 返回值 方法名(参数);  
  
}
```

#### impl 降级方法->返回兜底数据 
```java
/**  
 * 服务降级
 */  
@Component  
public class ProductDegradeFeignClient implements ProductFeignClient {
	/**  
     * 返回兜底数据
     * @return  
     */  
}
```

#### 调用方
一. 引入依赖
二. 开启扫描远程调用接口  
```java
@EnableFeignClients(basePackages = "com.atguigu.gmall")
```
三. 注入远程调用对象
```
	@Autowired
	private ProductFeignClient productFeignClient;
```
四. 调用方法

注意: 前台页面远程调用后台时没有添加header数据, 需要配置拦截器: 
```java
  
import feign.RequestInterceptor;  
import feign.RequestTemplate;  
import org.springframework.stereotype.Component;  
import org.springframework.web.context.request.RequestContextHolder;  
import org.springframework.web.context.request.ServletRequestAttributes;  
import javax.servlet.http.HttpServletRequest;  
  
@Component  
public class FeignInterceptor implements RequestInterceptor {  
    public void apply(RequestTemplate requestTemplate){  
            //  微服务远程调用使用feign ，feign 传递数据的时候，没有添加header 数据。  
            ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();  
            HttpServletRequest request = attributes.getRequest();  
            //  添加header 数据  
            requestTemplate.header("userTempId", request.getHeader("userTempId"));  
            requestTemplate.header("userId", request.getHeader("userId"));  
    }  
}
```