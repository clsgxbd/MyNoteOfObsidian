#CompletableFuture #FutureTask

CSDN:
http://t.csdn.cn/71EQm
http://t.csdn.cn/LO6qm

JDK5 -> FutureTask
JDK8 -> CompletableFuture

总结 : 
	executor参数指定线程池
	带Async的都是异步(新线程)
	

#### thenApply，thenAccept，thenRun，thenCompose的区别
| 特点 | thenApply | thenAccept | thenRun | thenCompos |
| - | - | - | - | - |
| 入参 | 有 | 有 | 无 | 有 |
| 返回值 | 有 | 无 | 无 | 有 |
| 新的CompletableFuture | 无 | 无 | 无 | 有 |


executor参数指定使用的线程池，没有指定会使用ForkJoinPool.commonPool() 作为它的线程池执行异步代码。
#线程池 #ThreadPool


### 一. 异步执行任务静态方法
```java
// 有返回值
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier);
// 有返回值 指定线程池
public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier,Executor executor);
// 无返回值
public static CompletableFuture<Void> runAsync(Runnable runnable);
// 无返回值 指定线程池
public static CompletableFuture<Void> runAsync(Runnable runnable, Executor executor);
```


### 二. 回调方法
####  1.  whenComplete
```java
public CompletableFuture<T> whenComplete(BiConsumer<? super T,? super Throwable> action)
public CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action)
public CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action, Executor executor)
```
#### exceptionally2. handle
```java
// 当原始的CompletableFuture抛出异常的时候，会进入exceptionally方法，用来处理异常的情况
public CompletableFuture<T> exceptionally(Function<Throwable,? extends T> fn)
```
####  3. handle
```java
public <U> CompletionStage<U> handle(BiFunction<? super T, Throwable, ? extends U> fn);
public <U> CompletionStage<U> handleAsync(BiFunction<? super T, Throwable, ? extends U> fn);
public <U> CompletionStage<U> handleAsync(BiFunction<? super T, Throwable, ? extends U> fn,Executor executor);
```
##### handle与whenComplete的区别
- 都是对结果进行处理,handle有返回值,whenComplete没有返回值
- 上一个任务异常时whenComplete可判断异常但不处理异常（仍然会抛异常）,而handle可实现exceptionally的功能：对异常进行"美化"（不再抛异常）



### 三. 连续回调的回调函数

#### 1. thenApply 转换结果
```java
public <U> CompletableFuture<U> thenApply(Function<? super T,? extends U> fn);
public <U> CompletableFuture<U> thenApplyAsync(Function<? super T,? extends U> fn);
public <U> CompletableFuture<U> thenApplyAsync(Function<? super T,? extends U> fn, Executor executor);
```
#### 2. thenAccept 消费结果
```java
public CompletionStage<Void> thenAccept(Consumer<? super T> action);
public CompletionStage<Void> thenAcceptAsync(Consumer<? super T> action);
public CompletionStage<Void> thenAcceptAsync(Consumer<? super T> action,Executor executor);
```
#### 3. thenRun 任务完成后触发的回调
```java
public CompletionStage<Void> thenRun(Runnable action);
public CompletionStage<Void> thenRunAsync(Runnable action);
public CompletionStage<Void> thenRunAsync(Runnable action,Executor executor);
```
#### 4. thenCompose
```java
public <U> CompletableFuture<U> thenCompose(Function<? super T, ? extends CompletionStage<U>> fn);
public <U> CompletableFuture<U> thenComposeAsync(Function<? super T, ? extends CompletionStage<U>> fn) ;
public <U> CompletableFuture<U> thenComposeAsync(Function<? super T, ? extends CompletionStage<U>> fn, Executor executor) ;
```
##### thenApply()和thenCompose（）的区别：
- henApply（）转换的是泛型中的类型，是同一个CompletableFuture，相当于将CompletableFuture 转换成CompletableFuture；
- thenCompose（）用来连接两个CompletableFuture，是生成一个新的CompletableFuture。


####  5.thenRun
跟 thenAccept 方法不一样的是，不关心任务的处理结果。只要上面的任务执行完成，就开始执行 thenAccept 。
```java
public CompletionStage<Void> thenRun(Runnable action);
public CompletionStage<Void> thenRunAsync(Runnable action);
public CompletionStage<Void> thenRunAsync(Runnable action,Executor executor);
```

####  6.thenCombine 合并任务
thenCombine 会把两个 CompletionStage 的任务都执行完成后，把两个任务的结果一块交给 thenCombine 来处理（类似thenApply转化操作）
```java
public <U,V> CompletionStage<V> thenCombine(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn);
public <U,V> CompletionStage<V> thenCombineAsync(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn);
public <U,V> CompletionStage<V> thenCombineAsync(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn,Executor executor);
```

####  7. thenAcceptBoth
当两个CompletionStage都执行完成后，接收两个任务的处理结果，并消费处理，无返回结果（类似thenAccept）。
```java
public <U> CompletionStage<Void> thenAcceptBoth(CompletionStage<? extends U> other,BiConsumer<? super T, ? super U> action);
public <U> CompletionStage<Void> thenAcceptBothAsync(CompletionStage<? extends U> other,BiConsumer<? super T, ? super U> action);
public <U> CompletionStage<Void> thenAcceptBothAsync(CompletionStage<? extends U> other,BiConsumer<? super T, ? super U> action,     Executor executor);
```

####  8. applyToEither 方法
两个CompletionStage，谁执行返回的结果快，就用哪个CompletionStage的结果进行下一步的转化操作（类似thenApply）。
```java
public <U> CompletionStage<U> applyToEither(CompletionStage<? extends T> other,Function<? super T, U> fn);
public <U> CompletionStage<U> applyToEitherAsync(CompletionStage<? extends T> other,Function<? super T, U> fn);
public <U> CompletionStage<U> applyToEitherAsync(CompletionStage<? extends T> other,Function<? super T, U> fn,Executor executor);
```

####  9. acceptEither 方法
两个CompletionStage，谁执行返回的结果快，就用哪个CompletionStage的结果进行下一步的消耗操作（类似thenAccept），无返回值。
```java
public CompletionStage<Void> acceptEither(CompletionStage<? extends T> other,Consumer<? super T> action);
public CompletionStage<Void> acceptEitherAsync(CompletionStage<? extends T> other,Consumer<? super T> action);
public CompletionStage<Void> acceptEitherAsync(CompletionStage<? extends T> other,Consumer<? super T> action,Executor executor);
```

####  10. runAfterEither 方法
两个CompletionStage，任何一个完成了都会执行下一步的操作（类似thenRun ）
```java
public CompletionStage<Void> runAfterEither(CompletionStage<?> other,Runnable action);
public CompletionStage<Void> runAfterEitherAsync(CompletionStage<?> other,Runnable action);
public CompletionStage<Void> runAfterEitherAsync(CompletionStage<?> other,Runnable action,Executor executor);
```
####  11. runAfterBoth

两个CompletionStage，都完成了计算才会执行下一步的操作（类似thenRun ）
```java
public CompletionStage<Void> runAfterBoth(CompletionStage<?> other,Runnable action);
public CompletionStage<Void> runAfterBothAsync(CompletionStage<?> other,Runnable action);
public CompletionStage<Void> runAfterBothAsync(CompletionStage<?> other,Runnable action,Executor executor);
```
####  12. thenCompose 方法
thenCompose 方法允许你对两个 CompletionStage 进行流水线操作，第一个操作完成时，将其结果作为参数传递给第二个操作。
```java
public <U> CompletableFuture<U> thenCompose(Function<? super T, ? extends CompletionStage<U>> fn);
public <U> CompletableFuture<U> thenComposeAsync(Function<? super T, ? extends CompletionStage<U>> fn) ;
public <U> CompletableFuture<U> thenComposeAsync(Function<? super T, ? extends CompletionStage<U>> fn, Executor executor) ;
```


###  四. 组合方法
```java
// 等待所有任务完成，构造后CompletableFuture完成
public static CompletableFuture<Void> allOf(CompletableFuture<?>... cfs).join();
// 只要有一个任务完成，构造后CompletableFuture就完成
public static CompletableFuture<Object> anyOf(CompletableFuture<?>... cfs).join();
```



原文: 
http://t.csdn.cn/71EQm
http://t.csdn.cn/LO6qm