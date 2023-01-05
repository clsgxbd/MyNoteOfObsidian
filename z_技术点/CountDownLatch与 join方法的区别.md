面试题
#CountDownLatch与join方法的区别
​		调用一个子线程的 **join()方法**后，该线程会一直**被阻塞直到子线程运行完毕**。而 CountDownLatch 则使用计数器允许子线程**运行完毕或者运行中**时候递减计数，也就是 CountDownLatch 可以在子线程运行任何时候让 await 方法返回而不一定必须等到线程结束；另外使用线程池来管理线程时候一般都是直接添加 Runnable 到线程池这时候就没有办法在调用线程的 join 方法了，countDownLatch 相比 Join 方法让我们对线程同步有更灵活的控制。