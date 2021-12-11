@Async注解
Spring Boot的主程序中配置@EnableAsync
@Async所修饰的函数不要定义为static类型，这样异步调用不会生效

如需获得返回值用Future
@Async
public Future<String> doTaskOne() throws Exception {
    System.out.println("开始做任务一");
    long start = System.currentTimeMillis();
    Thread.sleep(random.nextInt(10000));
    long end = System.currentTimeMillis();
    System.out.println("完成任务一，耗时：" + (end - start) + "毫秒");
    return new AsyncResult<>("任务一完成");
}
  
  
public void test() throws Exception {
	long start = System.currentTimeMillis();
	Future<String> task1 = task.doTaskOne();
	Future<String> task2 = task.doTaskTwo();
	Future<String> task3 = task.doTaskThree();
	while(true) {
		if(task1.isDone() && task2.isDone() && task3.isDone()) {
			// 三个任务都调用完成，退出循环等待
			break;
		}
		Thread.sleep(1000);
	}
	long end = System.currentTimeMillis();
	System.out.println("任务全部完成，总耗时：" + (end - start) + "毫秒");
}
