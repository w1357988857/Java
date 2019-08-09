INSTANCEOF：

```java
/**
 * instanceof：判断前一个是否是后一个类型
 */
public class InstanceOfDemo {
    public static void main(String[] args) {
        String a = "红尘来呀来，去呀去";
        /**
         *  instanceof - 》boolean :true/flase
         */
        System.out.println(a instanceof String);//true
        System.out.println(a instanceof Object);//true
        Object obj = new Object();
        System.out.println(obj instanceof String);//false
    }
}

```

THROW：

```java
public class ThrowsDemo {
    public static void main(String[] args) {
        try {
            //异常不处理，编译不通过
            test();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static void test() throws FileNotFoundException {
        throwsExcep();
    }

    /**
     *
     * @throws FileNotFoundException ：检查型异常，编译不通过
     */
    public static void throwsExcep() throws FileNotFoundException {
        //当前方法中不知道该怎样处理该异常，就将其抛出去
        try {
            InputStream is = new FileInputStream("Aa");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
            e.getMessage();//获取的信息即是要传的参数
            //对异常的初步处理

            //需要在另一处对异常做进一步的处理
            throw e;
        }
    }

    public static void test2(){
        //运行时异常只会在运行的时候才会抛出，在编译期间感知不到
        throwRunExe();
    }

    /**
     * 异常与业务分离
     * @throws RuntimeException
     *
     * throw ：异常实例
     *         在异常初步处理后，还可以在另一处对异常做进一步的处理。
     * throws ：异常类
     */
    public static void throwRunExe() throws RuntimeException{
        //运行时异常
        int i = 10/0;
        throw new RuntimeException();
    }
}

```

FINALLY：

```java
public class FinallyDemo {
    public static void main(String[] args) {
        test();
    }

    public static void test(){
        InputStream is = null;
        try {
            //输入流，检查型异常必须要处理，否则编译不通过
            //流对象比较耗资源，使用完毕后必须要关闭
            is = new FileInputStream("aa");
            /*
            try-catch-finally中包含try-catch-finally属于嵌套异常，
            每个块中都可以包含try-catch-finally
             */

            /*try {
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }*/

            /*
            在此时发生异常，也会进入finally执行
             */
            int i = 10/0;

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }finally {
            /*
            finally不管有没有发生异常，
                都会执行，执行在try-catch中
             */

            try {
                /*
                必须判断流对象是否为空，否则会报空指针异常
                 */
                if(is != null){
                    is.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /*
    finalize（）：GC的不确定性；不能保证一定会执行
        GC（垃圾回收装置）执行，才会执行finalize（）
     */
    @Override
    //finalize
    //属性    构造器     方法
    protected void finalize() throws Throwable {
        System.out.println(this.toString());
        System.out.println("Finalize");
    }
}
```

内部类

```java
/**
 * 一个类中所包含：
 *     属性   方法      构造器     内部类
 */

/**
 * InnerClassDemo：外部类
 *      在一个文件中，只有一个public修饰的类，并且类名要和文件名相同
 */
public class InnerClassDemo {
    //实例成员：实例。
    private String name;
    private int age = 10;
    //类成员：类名。
    private static int b = 22;

    //构造器
    public InnerClassDemo() {}

    void test(){}

    /**
     * Info；内部类
     */
    class Info{
        private int age = 20;
        int info(){
            int age = 30;//就近原则
            return age;
        }
    }

    /**
     * 静态内部类
     */
    static class InfoStatic{
        int c = 20;
        static int d = 20;
        void demo(){
            InnerClassDemo icd = new InnerClassDemo();
            System.out.println("a = " + icd.age);
            System.out.println(b);
        }
    }
}

/**
 * Tt：外部类
 *     相当于在外部定义了一个类Tt
 */
class Tt{

}

```

```java
public class Main {
    public static void main(String[] args) {
        InnerClassDemo icd = new InnerClassDemo();
        //内部类创建实例
        InnerClassDemo.Info info = icd.new Info();
        info.info();
        //静态内部类创建实例
        InnerClassDemo.InfoStatic infoStatic = new                	 	InnerClassDemo.InfoStatic();
        infoStatic.demo();
    }
}

```

匿名内部类：

```java
/**
 * 接口
 */
public interface AnonymousInterface {
    void test();
}

/**
 * 抽象
 */
public abstract class BaseAnonymous {
    private String name;
    public abstract void anonymous();

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```



```java
/**
 * 匿名内部类:
 *      不能是抽象类，
 */
public class AnonymousMain {
    public static void main(String[] args) {
        testBase();
        testBase2();
    }

    /**
     * 抽象匿名内部类
     */
    public static void testBase(){
        /**
         * 匿名内部类：
         *      适合使用一次的类
         * 隐含的含义是；抽象类没有实例，new BaseAnonymous()
         *               会创建一个匿名的内部类，这个内部类继承 BaseAnonymous
         *               ba 指向匿名内部类的实例
         *   方法内部的局部内部类
         */
        BaseAnonymous ba = new BaseAnonymous(){

            @Override
            public void anonymous() {
                setName("ABC");
                System.out.println("AnonymousInnerClass");
            }
        };
        ba.anonymous();
        ba.getName();
    }

    /**
     * 接口匿名内部类
     */
    public static void testBase2(){
        //匿名内部类 - 》实现了AnonymousInter接口
        AnonymousInterface ai = new AnonymousInterface() {
            @Override
            public void test() {
                System.out.println("匿名内部类实现了AnonymousInterface");
            }
        };
        ai.test();
    }

    public static void test(){
        class FunInner{

        }
    }

}
```

枚举类（穷举）

J2SE1.5新增的

​	所有实例必须在枚举类中显示列出

```java
/**
 * final修饰的类是不可变类
 */
public enum GenderEnum {
    /**
     * 枚举类：
     *      实例不是由程序员手动new出来，而是创建类时就指定
     *          系统会自动添加public static final 修饰
     *       可以实现一个或多个接口，默认继承java.lang.Enum类
     */
    MAN,FEMALE,UNKOWN;
}
```



```java
public class MainEnum {
    public static void main(String[] args) {
        System.out.println(GenderEnum.MAN);
        //实例数组
        GenderEnum[] ges = GenderEnum.values();
        for(GenderEnum ge : ges){
            System.out.println(ge);
        }
    }
}
```

