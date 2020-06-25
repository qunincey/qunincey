# 调试问题

第一种场景
普通业务调试，在自己本机的上的模块直接使用不同的服务即可
![alt](https://qi-1257043449.cos.ap-guangzhou.myqcloud.com/rongji/rongji01.png)


第二种场景   

基础模块调试问题，由于基础模块负责相对应的client模块代码，所以业务模块在请求基础模块的时候会自动向线上地址发起请求，实现的效果就是当本地的业务模块发起请求的时候，向本地的基础模块请求

![alt](https://qi-1257043449.cos.ap-guangzhou.myqcloud.com/rongji/rongji02.png)

1.  第一种解决方案    
   使用url注解，本地起一个基础服务，在相对应的client上加上url注解
    ```java
    @FeignClient(name = EurekaClientServerNameConstant.EUREKA_CLIENT_SERVER_NAME, contextId = "umsOrgClient", url = "${userclinet.url}")
    ```

    然后在调用方定义userclinet.url即可。
    ```yam
    userclinet:
    url: http://192.168.211.101:9095/
    ```
2.  第二种解决方案   
    serverlist