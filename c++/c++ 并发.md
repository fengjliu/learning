# Concurrency Patterns

https://www.modernescpp.com/index.php/concurrency-patterns

# Self thinking


计算机领域中的并发是单个系统里同时执行多个任务； 有两种情景： 任务切换/真正的硬件并发/两者的混合
实现方式： 多进程并发和多线程并发
多进程并发： 通信方式有信号， 套接字， 文件 和管道， 需要操作系统启动和管理多个进程， 但是优点更容易编写安全的代码以及可以把任务部署到多个机器上
多线程并发：  需要做线程同步， 确保所有线程访问的共享数据的一致性， 同时上下文切换带来资源消耗。 通信方式比如共享内存
为啥使用并发： 分离关注点和提高性能
定义线程对象并初始化三种推荐方式: 
```
    1. void do_some_work();
        std::thread my_thread(do_some_work);
	2.  class background_task
		{ p
		ublic:
		void operator()() const
		{
		do_something();
		do_something_else();
		}
		};
		background_task f;
		std::thread my_thread(f); 
	3.lanmda
	    std::thread my_thread([]{
		do_something();
		do_something_else();
		});
```
 为了避免条件竞争。 两种手段：互斥量加锁；原子操作
