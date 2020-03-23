### Eureka Server 搭建 ###

#### 配置pom ####
```pom
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

#### 开启 Eureka Server ####
```java
@EnableEurekaServer
public class EurekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }

}
```

#### 配置application.yml ####
```yaml
spring:
  application:
    # 服务名称
    name: eureka
server:
  port: 1111
eureka:
  client:
    # 自己不注册到注册中心
    register-with-eureka: false
    # 不从注册中心获取注册信息
    fetch-registry: false
```

### Eureka Server 集群部署 ###
#### 增加application.yml配置文件 ####
> 1. 主要通过eureka.client.service-url.defaultZone 使得Eureka服务之间互相注册
> 2. 并开启register-with-eureka和fetch-registry

#### application-a.yml ####
```pom
spring:
  application:
    # 服务名称
    name: eureka
server:
  port: 1111
eureka:
  instance:
    hostname: eureka-a
  client:
    # 自己不注册到注册中心
    register-with-eureka: false
    # 不从注册中心获取注册信息
    fetch-registry: false
    service-url:
      defaultZone: http://localhost:1112/eureka
```

#### application-b.yml ####
```pom
spring:
  application:
    # 服务名称
    name: eureka
server:
  port: 1112
eureka:
  instance:
    hostname: eureka-b
  client:
    # 自己不注册到注册中心
    register-with-eureka: false
    # 不从注册中心获取注册信息
    fetch-registry: false
    service-url:
      defaultZone: http://localhost:1111/eureka
```

#### 执行打包后分别启动两个Eureka服务 ####
```
java -jar eureka-0.0.1-SNAPSHOT.jar --spring.profiles.active=a
java -jar eureka-0.0.1-SNAPSHOT.jar --spring.profiles.active=b
```