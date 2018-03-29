## springcloud-hello
导入这三个项目，然后依次启动这三个项目:
springcloud-eureka、springcloud-producer、springcloud-consumer
都能成功启动之后，在浏览器输入：http://localhost:8000/
(也就是在springcloud-eureka配置服务中心的地址)


先输入：http://localhost:9001/hello?name=pancm 
查询是否能够成功启动。
然后再输入:http://localhost:9002/hello/pancm
查看返回是否和9001服务返回的数据一致。
若一致，则说明客户端已经成功的通过feign调用了远程服务hello，并且将结果返回到了浏览器。



