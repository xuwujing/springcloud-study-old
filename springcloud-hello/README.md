## springcloud-hello
### hello 测试
导入这三个项目，然后依次启动这三个项目:
springcloud-eureka、springcloud-producer、springcloud-consumer
都能成功启动之后，在浏览器输入：
` http://localhost:8000`(也就是在springcloud-eureka配置服务中心的地址)


先输入：
> http://localhost:9001/hello?name=pancm 

查询是否能够成功启动,
然后再输入:
> http://localhost:9002/hello/pancm 

查看返回是否和9001服务返回的数据一致。
若一致，则说明客户端已经成功的通过feign调用了远程服务hello，并且将结果返回到了浏览器。


### 负载均衡测试
成功启动springcloud-eureka、springcloud-producer、springcloud-consumer 之后，更改springcloud-producer的**application.properties**配置，将端口改为9003，然后再到web层更改返回数据。然后再启动一个springcloud-producer主程序。在`http://localhost:8000` 可以看到多出了一个地址，
最后再到浏览器输入:
> http://localhost:9002/hello/pancm 

重复进行访问，可以看到输出的结果交叉打印

    pancm,Hello World
    pancm,Hello World!!!
	pancm,Hello World
    pancm,Hello World!!!




### 熔断测试
只需在服务消费者`springcloud-consumer`
**application.properties**配置文件添加:

    feign.hystrix.enabled=true

然后修改HelloRemote



	@FeignClient(name= "springcloud-producer",fallback = HelloRemoteHystrix.class)
	public interface HelloRemote {
	    @RequestMapping(value = "/hello")
	    public String hello(@RequestParam(value = "name") String name);
	}



新增异常处理类:


	@Component
	public class HelloRemoteHystrix implements HelloRemote{
	
	    @Override
	    public String hello(@RequestParam(value = "name") String name) {
	        return name+", 请求服务生产者失败!";
	    }
	}



然后依次启动springcloud-eureka、springcloud-producer、springcloud-consumer，正常测试一遍没问题之后，停止springcloud-producer，在发送请求，会使用熔断机制，调用刚刚书写的类。
