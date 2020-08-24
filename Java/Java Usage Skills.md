# Java Usage Skills

### 1. URLDecoder

> when get prams from URL like below, may exists encoding issue. We choose base64 encode/decode to ensure all chars can transport, however '**=**' is used as a `Fill bit` will be transformed as '**%3D**' in URL.
>
> then we can combine URLDecoder with base64 encode/decode together to solve the issue

```URL
localhost:9528/#/register?token=bkU0Y1ozWXdzTW5DRUtKYVB5UnFHZ255SHJvZGtHOHRBdUFEWXY2dkUreWhGSHMrSFdsMlQ0bjlGY0plejljRlZoMmxxVzZsMjIwdEU1cGpBOHkyZThGZW9HaHVKY2YrYVRMWHVYYVFHOFdTK1c4TkJNa0szNGJZTGt3YmRwZnk1cVBaRFo0bzRObzlCVzRTeFQ1Vks3UjJGYXBBbXl0ZkVqQ005cWcvcXVxR1RvTUY0YWlkVVpTNTc2V0l4SlRxUkdhUGtqcDFDbkFpTHpiKy9TYUM3Mzc4cURvUnJmd2U%3D

```

```groovy
	final Base64.Decoder decoder = Base64.getDecoder()
	final Base64.Encoder encoder = Base64.getEncoder()

	@Autowired
	Gson gson

gson.fromJson(EncryptUtil.decryptPsw(new String(decoder.decode(URLDecoder.decode(registerToken,"UTF-8")),"UTF-8")),Object)
```

### 2. Thread timeout

- [ExecutorService等待线程执行并设置超时时间的三种方式](https://blog.csdn.net/m0_38001814/article/details/102730198)

- [线程池ExecutorService的使用及其正确关闭方法](https://www.cnblogs.com/zhncnblogs/p/10894271.html)

```java
class Test {
	static void test(int timeoutInSec){
		ExecutorService executorService = Executors.newFixedThreadPool(1)
		Future<Object> future
		future = executorService.submit(new Callable<Object>() {
			@Override
			Object call() throws Exception {
//				while (true){
//					println('Thread start')
//					Thread.sleep(1)
//				}
//				1/0
				println('Thread done')
				return 'ff'
			}
		} )

		executorService.shutdown()
		executorService.awaitTermination(timeoutInSec,TimeUnit.SECONDS)
		future.cancel(true)


//		executorService.shutdown()

//		executorService.shutdownNow()
		println("i'm done")
		Object o
		try{
			o = future.get()
		}catch(Exception e){
			println('exception....')
			e.printStackTrace()

			println('----------------')
		}

		println(o)
	}

	public static void main(String[] args) {
		test(3)
	}
}
```

强行停止

```java
		Thread executeDaemonThread = new Thread(new Runnable() {
			@Override
			void run() {

				try{
                    ....
				}
				catch(Exception e){
					...
				}
				finally{
					//redirect print info to Console
					System.setOut(System.out)
				}
			}
		})


		executeDaemonThread.setDaemon(true)
		executeDaemonThread.start()

		TimeUnit.SECONDS.timedJoin(executeDaemonThread,10)
		if(executeDaemonThread.isAlive()){
			executeDaemonThread.stop()  //todo: find another more safety stop function
		}
```

