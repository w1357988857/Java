## 

Java 8 新增了接口的默认方法。简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。我们只需在方法名前面加个default关键字即可实现默认方法。了解决接口的修改与现有的实现不兼容的问题。

```
public interface InterfaceDemo {

    default void info(){
        System.out.println("info");
    }
    static void test(){
        System.out.println("test");
    }
}
```

```
public class InterfaceDemoImpl implements InterfaceDemo{
    @Override
    public void info() {
//        System.out.println("info");
        // 调用接口的默认实现
        InterfaceDemo.super.info();
        // 调用接口的默认静态方法实现
        InterfaceDemo.test();
    }

    public static void main(String[] args) {
        InterfaceDemoImpl interfaceDemo = new InterfaceDemoImpl();
        interfaceDemo.info();
    }
}
```

## Lambda表达式

Lambda 表达式，也可称为**闭包**，它是推动 Java 8 发布的最重要新特性。

Lambda 允许把**函数作为一个方法的参数**（函数作为参数传递进方法中）。

使用Lambda 表达式可以使代码变的更加简洁紧凑。

```
(parameters) -> expression
(parameters) ->{statements; }
```

以下是lambda表达式的**重要特征**:

- 可选类型声明：不需要声明参数类型，编译器可以**统一识别**参数值。
- 可选的参数圆括号：**一个参数无需定义圆括号**，但多个参数需要定义圆括号。
- 可选的大括号：如果主体包含了**一个语句**，就**不需要使用大括号**。
- 可选的返回关键字：如果主体**只有一个表达式返回值**则编译器会**自动返回值**，大括号需要指定明表达式返回了一个数值

```
public class LambdaDemo {

    public static void main(String[] args) {
        // 类型声明
        MathOperation addition = (int a, int b) -> a + b;
        // 不用类型声明,自动识别
        MathOperation subtraction = (a, b) -> a - b;
        // 大括号中的返回语句
        MathOperation multiplication = (int a, int b) -> {
            return a * b;
        };
        // 没有大括号及返回语句
        MathOperation division = (int a, int b) -> a / b;
        // 直接使用接口变量调用
        System.out.println(addition.operation(10,15));
    }

    interface MathOperation {
        int operation(int a, int b);
    }
}
```

Lambda 表达式主要用来定义行内执行的方法类型接口，例如，一个简单方法接口。在上面例子中，我们使用各种类型的Lambda表达式来定义MathOperation接口的方法。

Lambda 表达式免去了使用匿名方法的麻烦，并且给予Java简单但是强大的**函数化的编程能力**。

**注意:**lambda 表达式只能引用标记了 final 的外层局部变量，这就是说不能在 lambda 内部修改定义在域外的局部变量，否则会编译错误。

lambda 表达式的局部变量可以不用声明为 final，但是必须不可被后面的代码修改（即隐性的具有final 的语义）

```
public static void main(String[] args) {
        String info = "test";
       
        // info = "dasf";// 修改会导致lambda表达式报错
        // 大括号中的返回语句
        MathOperation multiplication = (int a, int b) -> {
            // Variable used in lambda expression should be final or effectively final
             // 不能在 lambda 内部修改定义在域外的局部变量，否则会编译错误。
            // info = "dasf"; // 报错
            System.out.println(info);
            return a * b;
        };
       
    }
```

在Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量

```
        // int a = 10; // 报错
        // 类型声明
        MathOperation addition = (int a, int b) -> a + b;
```

## 方法引用

方法引用使用一对冒号 **::** 

```
public static void main(String[] args) {
        List<String> names = new ArrayList();
        names.add("郭德纲");
        names.add("于谦");
        // 遍历输出
        names.forEach(System.out::println);
    }
```

System.out::println 方法作为静态方法来引用

