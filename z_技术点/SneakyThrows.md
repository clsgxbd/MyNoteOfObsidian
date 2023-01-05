#SneakyThrows

学习链接: http://t.csdn.cn/700Bk

@SneakyThrows 注解: 

鬼鬼祟祟的抛出
是将编译时异常封装成运行时异常骗过编译器, 在运行阶段抛出

常用于切面类的环绕通知 #切面 #AOP 
@SneakyThrows
@Around("@annotation(com.xxx....xxxCatch)") // 环绕通知注解