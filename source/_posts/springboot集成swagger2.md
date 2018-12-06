---
title: SpringBoot集成swagger2
date: 2018-12-6 16:52:00
category: springboot
tags: [springboot, swagger2]
---
       随着互联网时代的快速发展，越来越多的项目使用了前后端分离开发模式，这种方式下API接口文档就变得至关重要了，而swagger就是帮助我们更快更好编写API接口的一种框架。那么如何把它与SpringBoot集成起来使用呢？
<!-- more -->

### 添加依赖
在 pom.xml 文件中添加swagger2的依赖
``` java
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

### 创建Java配置
* 添加配置类
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.xxx"))
                .paths(PathSelectors.regex("/api/.*"))
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Restful APIs")
                .description("")
                .contact(new Contact("xxx", "xxx", "xxx"))
                .version("1.0")
                .build();
    }

}
```
* 添加静态文件映射配置
```java
@Configuration
public class WebConfig extends WebMvcConfigurationSupport {

  

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
}
```

### 编写API接口
添加@ApiOperation注解来给API增加说明，添加@ApiImplicitParams、@ApiImplicitParam注解来给参数增加说明
```java
    @GetMapping("/test/{id}")
    @ApiOperation(value = "测试", notes = "接口测试")
    @ApiImplicitParam(name = "id", value = "测试ID", required = true, dataType = "Integer")
    public RestResult test(@PathVariable Integer id) {
        return RestResult.builder().result(true).data(id).build();
    }
```
### API文档访问
点击浏览器链接  http://localhost:8090/swagger-ui.html 