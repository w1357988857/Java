## instanceof

它的作用是测试它左边的对象是否是它右边的类的实例，或者右边的类的子类实例，返回 boolean 的数据类型。

```
 public static void test(){
        String a = "";
        Object obj = new Object();
        System.out.println(a instanceof  String);// true
        System.out.println(a instanceof  Object);// true
        System.out.println(obj instanceof  String);// false
    }
```



## 内部类

我们把一个类放在另一个类的内部定义，这个定义在其他类内部的类就被称为内部类，有的也叫嵌套类，包含内部类的类也被称为外部类有的也叫宿住类。

内部类提供了更好的封装，内部类成员可以直接访问外部类的私有数据，因为内部类被当成其他外部类成员。

```
public class InnerClassDemo {
    // 外部类私有成员
    private int a = 10;
    class Inner{
        void test(){
            // 内部类中可直接访问
            System.out.println(a);
        }
    }
}
```

### 非静态内部类

当在非静态内部类的方法内访问某个变量时，系统优先在该方法内查找是否存在该名字的局部变量，如果存在该名字的局部变量，就使用该变量，如果不存在，则到该方法所在的内部类中查找是否存在该名字的属性，如果存在则使用该属性。

总之，第一步先找局部变量，第二步，内部类的属性，第三步。外部类的属性。

```
public class InnerClassDemo {
    // 外部类私有成员
    private int a = 10;

    public static void main(String[] args) {
        new InnerClassDemo().new Inner().test();
    }

    class Inner{
        int a = 20;
        void test(){
            int a = 30;
            // 内部类中可直接访问
            System.out.println(a);
        }
    }
}
```

### 静态内部类

如果用static修饰一个内部类，称为静态内部类。

静态内部类可以包含静态成员，也可以包含非静态成员。所以静态内部类不能访问外部类的实例成员，只能访问外部类的类成员。

静态内部类的对象寄存在外部类里，非静态内部类的对象寄存在外部类实例里

```
public class InnerClassDemo {
    // 外部类私有成员
    private int a = 10;

    public static void main(String[] args) {
        // 非静态内部类的对象寄存在外部类实例里
        new InnerClassDemo().new Inner().test();
        // 静态内部类的对象寄存在外部类里
        new InnerClassDemo.InnerStatic().test();
    }

    class Inner{
        int a = 20;
        void test(){
            int a = 30;
            // 内部类中可直接访问
            System.out.println(a);
        }
    }
    static class InnerStatic{
        static int c = 15;
        int b = 16;
        void test(){
            // a为外部类实例成员，访问报错
//            System.out.println(a);
        }
    }
}
```

### 局部内部类

如果把一个内部类放在方法里定义，这就是局部内部类，仅仅在这个方法里有效。

局部内部类不能在外部类以外的地方使用，那么局部内部类也不能使用访部控制符和static修饰

```
    void testInner(){
        // 将访问修饰符会报错，如private
        class Inner{

        }
        Inner inner = new Inner();
    }
```

### 匿名内部类

匿名内部类适合创建那种只需要一次使用的类，定义匿名内部类的语法格式如下：

new 父类构造器（抽象类/接口）

{

​      //匿名内部类的 类体部分

}

匿名内部类不能是抽象类，匿名内部类不能定义构造器

```
 IProduct product = new IProduct(){
        @Override
        public String getProduct() {
            return null;
        }
    };
```

## 枚举类

J2SE1.5新增了一个enum关键字，用以定义枚举类。正如前面看到，枚举类是一种特殊的类，它一样可以有自己的方法和属性，可以实现一个或者多个接口，也可以定义自己的构造器。

枚举类也是一种类，只是它是一种**比较特殊的类**，因此它一样可以使用属性和方法

枚举类可以实现一个或多个接口，使用enum定义的枚举类默认继承了java.lang.Enum类，而不是继承Object类(间接继承)。其中java.lang.Enum类实现了java.lang.Serializable和java.lang.Comparable两个接口。

枚举类的构造器只能使用private访问控制符，如果省略了其构造器的访问控制符，则默认使用private修饰；如果强制指定访问控制符，则只能指定private修饰符。

枚举类的所有实例必须在枚举类中**显式列出**，否则这个枚举类将永远都不能产生实例。列出这些实例时系统会自动添加public static final修饰，无需程序员显式添加。

```
public enum Animal {
    PIG,DOG,MONKEY;
    public int age;

    private Animal(){
        
    }
}
```

所有枚举类都提供了一个values方法，该方法可以很方便地遍历所有的枚举值。 

```
 public static void main(String[] args) {
        Animal[] animals = Animal.values();
        for (Animal anim:animals) {
            System.out.println(anim);
            System.out.println(anim.age);
        }
    }
```

枚举类通常应该设计成不可变类，也就说它的属性值不应该允许改变，这样会更安全，而且代码更加简洁。为此，我们应该将枚举类的属性都使用private final修饰。

一旦为枚举类显式定义了带参数的构造器，则列出枚举值时也必须对应地传入参数。  

```
public enum Animal {
    PIG(10),DOG(20),MONKEY(30);
    private final int age; // 推荐

    private Animal(int age){
      this.age = age;
    }
}
```

枚举类也可以实现一个或多个接口。与普通类实现一个或多个接口完全一样，枚举类实现一个或多个接口时，也需要实现该接口所包含的方法。 

如果需要每个枚举值在调用同一个方法时呈现出不同的行为方式，则可以让每个枚举值分别来实现该方法，每个枚举值提供不同的实现方式，从而让不同枚举值调用同一个方法时具有不同的行为方式

```
public interface EnumInterface {
    void play();
}
```

```
public enum Animal implements EnumInterface{
    PIG(10){
        @Override
        public void play() {
            System.out.println("拱白菜");
        }
    },DOG(20){
        @Override
        public void play() {
            System.out.println("啃骨头");
        }
    },MONKEY(30);
    private final int age;

    private Animal(int age){
      this.age = age;
    }

    @Override
    public void play() {
        System.out.println("耍");
    }
}
```

可以在枚举类里定义一个抽象方法，然后把这个抽象方法交给各枚举值去实现即可。

```
 PIG(10){
        @Override
        public void play() {
            System.out.println("拱白菜");
        }
        // 类中包含抽象方法，要为每个实例提供相应的实现
        @Override
        public void yelling() {
            
        }
    }
```

```
// 枚举类内部定义
    public abstract void yelling(); 
```

Object

Object类是所有类、数组、枚举类的父类，也就是说，Java允许把所有任何类型的对象赋给Object类型的变量。当定义一个类时没有使用extends关键字为它显式指定父类，则该类默认继承Object父类。

打印对象和toString方法：toString方法是系统将会输出该对象的“自我描述”信息，用以告诉外界对象具有的状态信息。Object 类提供的toString方法总是返回该对象实现类的类名 + @ +hashCode值。

==和equals比较运算符：==要求两个引用变量指向同一个对象才会返回true。equals方法则允许用户提供自定义的相等规则。

Object类提供的equals方法判断两个对象相等的标准与==完全相同。因此开发者通常需要重写equals方法。



JVM

GC

