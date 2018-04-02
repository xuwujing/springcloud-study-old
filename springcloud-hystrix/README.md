## springcloud-hystrix

### 熔断测试

依次启动springcloud-eureka、springcloud-producer、springcloud-consumer-hystrix，正常测试一遍没问题之后，停止springcloud-producer，在发送请求，会使用熔断机制，调用写的移除处理信息。
