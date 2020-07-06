# 调试问题

第一种场景
普通业务调试，在自己本机的上的模块直接使用不同的服务即可
![alt](https://qi-1257043449.cos.ap-guangzhou.myqcloud.com/rongji/rongji01.png)


第二种场景   

基础模块调试问题，由于基础模块负责相对应的client模块代码，所以业务模块在请求基础模块的时候会自动向线上地址发起请求，实现的效果就是当本地的业务模块发起请求的时候，向本地的基础模块请求

![alt](https://qi-1257043449.cos.ap-guangzhou.myqcloud.com/rongji/rongji02.png)

1.  第一种解决方案    
   使用url注解，在相对应的client上加上url注解
    ```java
    @FeignClient(name = EurekaClientServerNameConstant.EUREKA_CLIENT_SERVER_NAME, contextId = "umsOrgClient", url = "${feign.url}")
    ```
    然后client的配置文件里中添加，如果是正式环境不需要添加任何值
    ```yaml
    feign:
      url:
    ```
    然后在调用方定义userclinet.url即可。
    ```yaml
    feign:
      url: http://192.168.211.101:9095/
    ```
2.  第二种解决方案   
    feign和euerka同时使用的时候，feign本身的ribbon会去eureka获取服务列表，可以通过自己设置服务列表来重定向feign的地址，同时必须禁止ribbon从eureka获取服务列表。
    ```yaml
    stores:
      ribbon:
        listOfServers: http://192.168.211.101:9095/
    ribbon:
      eureka:
        enabled: false
    ```
    这种解决方案有个缺点，上面这种完全基于ribbon的方法，使用ribbon就只能手动发送template请求。
    ```java
     loadBalancerClient.execute("stores", instance, new LoadBalancerRequest<Object>() {
            @Override
            public Object apply(ServiceInstance instance) throws Exception {
                return null;
            }
        });
    ```