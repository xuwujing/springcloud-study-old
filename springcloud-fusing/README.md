## springcloud-fusing

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
