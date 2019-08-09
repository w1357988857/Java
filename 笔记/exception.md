```
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at com.train.day07.ExceptionDemo1.test3(ExceptionDemo1.java:25)
	at com.train.day07.ExceptionDemo1.test2(ExceptionDemo1.java:18)
	at com.train.day07.ExceptionDemo1.test1(ExceptionDemo1.java:12)
	at com.train.day07.ExceptionDemo1.main(ExceptionDemo1.java:6)
```

错误信息为方法的调用栈信息

main 调用 test1 ->调用test2-》调用test3 在test3中发生异常，再将来检查错误时，从上向下找到自己所写代码第一次出现的位置，点击对应的类，指向的哪一行就是错误抛出的位置，之后根据此行代码，去分析错误。



异常：

- 运行时异常 RuntimeException，是因为程序员的问题，写代码缺少必要的判断与逻辑，遇到这样的问题，必须要解决，解决的方式，不是try-catch，而是通过修改代码，避免这样的异常
  - ArithmeticException
  - NullPointerException 
  - ......
- 检查型异常 CheckedException ，写代码的过程，如果需要捕获，程序会提示，否则编译不通过，无法运行
  - File 
  - IO

运行时异常，就是我们所俗称的bug，程序运行的时候抛出的

ArithmeticException 算术异常    运行时异常

NullPointerException 空指针异常

ArraysOutOfBoundsException 数组下表越界  





