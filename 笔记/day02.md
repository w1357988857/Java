# Day02

## switch-case-break

Switch-case-break

1、使用break语句，防止case穿透
 2、 default可以省略，但不推荐省略
 3、 switch语句中控制表达式的类型只能是byte、short、char、int、String（JDK7新增）和枚举

```
public static void main(String[] args) {
        // 从键盘接收输入，一串字符串
        // 如果输入的是篮球，输出，爱好是篮球
        // 如果输入的是足球，输出，爱好是足球
        // 如果输入的是网球，输出，爱好是网球
        // 如果输入的不是上述信息，输出输入不合法
        // default:如果输入的信息都没有出现在case中，则为default
        // break:跳出switch语句

        Scanner scanner = new Scanner(System.in);
        // 接收键盘输入的字符串  ctrl + q
        String hobby = scanner.nextLine();
        //  开关 选择  byte、short、char、int、String（JDK7新增）和枚举
        switch (hobby){

            case "篮球":
            case "乒乓球":
                System.out.println("爱好是篮球或乒乓球");
                break;  // 如果没有break,case语句穿透
            // 可以出现在switch的任何位置，执行不会受到影响
            default:
                System.out.println("输入不合法");
                break;

            case "足球":
                System.out.println("爱好是足球");
                break;
            case "网球":
                System.out.println("爱好是网球");
                break;

        }
    }
```

## 引用类型

处理基本数据类型，都是引用类型

引用类型，使用==比较两个值是否相等，比较的是存储地址是否相等

只要是new（新建）出来的东西，一定会在堆中（heap）重新开辟空间来存储其数据

```
String word2 = new String("Everyone,there is a world, quiet and lonely there live something!");

```

```
// 后面的字符串是一个常量，String是不可变的，常量存储在常量去，可以共用
String word = "Everyone,there is a world, quiet and lonely there live something!";
String word1 = "Everyone,there is a world, quiet and lonely there live something!";
```

**实例代码**

```
public static void main(String[] args) {
        String word = "Everyone,there is a world, quiet and lonely there live something!";
        String word1 = "Everyone,there is a world, quiet and lonely there live something!";
        String word2 = new String("Everyone,there is a world, quiet and lonely there live something!");
        System.out.println(word);
        System.out.println(word2);
        System.out.println(word == word1);
        System.out.println(word == word2);
        // 不管存储在什么方法，都可以轻松判断值是否相同，这个方法就是equals方法
        // 比较两个引用类型的变量值是否相同，使用equals方法，后面涉及到对象时，
        // 会涉及到重写equals方法
        System.out.println(word.equals(word2));
//        int a = 10;
//        int b = 20;
//        System.out.println(a == b);

        // 字符串分割
        // 字符串截取
        // 查找
    }
```

String的equals方法实现

```
word.equals(word2)
word2 - 传递给 》  anObject

public boolean equals(Object anObject) {
        // == 比较的是地址，如果地址相同，那么值一定相同
        if (this == anObject) {
            return true;
        }
        // instanceof判断anObject是否是String类型的
        if (anObject instanceof String) {
            // 将anObject从Object类型转换到String类型
            // 赋值给anotherString
            String anotherString = (String)anObject;
            // word =转换成字符数组=> value,得到word的长度
            int n = value.length;
            // 判断两个比较的字符串长度是否相等
            if (n == anotherString.value.length) {
                char v1[] = value;// {'E','v','e','r','y'}
                // {'E','v','e','r','y'}
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
```



https://www.cnblogs.com/zhangzongxing01/p/5559126.html

String   字符串

类 / 对象/实例

## do-while

与while的区别

do-while至少会执行循环一次，先循环，再判断

while 先判断  再循环

```
public static void main(String[] args) {
        int i = 10;
        // 先判断后执行
        while(i < 10){
            System.out.println("while" + i);
        }
        // 先执行 后判断
        do{
            System.out.println("do-while " + i);
        } while (i < 10);

    }
```

注意：**不管是什么循环，一定要给一个出口条件，否则产生死循环，对程序造成是毁灭性的打击。**

## 嵌套for循环

```
 public static void main(String[] args) {
        /*int i = 0;
        for( ; i < 10; i ++) {
        }
        System.out.println(i);*/

        ///for(;;){}  // 死循环
        // 99乘法表
        for(int j = 1; j <= 9; j++) {
            for(int i = 1; i <= j; i++){
                System.out.print(i + " * " + j + " = " +  (i * j) + "\t");
            }
            System.out.println();
        }

    }
```

## return-break-continue

### return

返回 - 结束方法

```
 public static void main(String[] args) {
         test();
        test1();
        String content = test2();
        System.out.println(content);
    }

    public  static void test(){
        String word = "天下风云出我辈，一入江湖岁月催。皇图霸业谈笑中，不胜人生一场醉。";
        System.out.println(word);
        // 结束方法，其后不能再写代码
        return;
    }
    // void 无返回值
    public  static void test1(){
        String word = "天下风云出我辈，一入江湖岁月催。皇图霸业谈笑中，不胜人生一场醉。";
        System.out.println(word);
        // 结束方法，其后不能再写代码
        if(word.equals("天下风云出我辈，一入江湖岁月催。皇图霸业谈笑中，不胜人生一场醉。")) {
            // 结束方法
            return;
        }
        System.out.println("return之后代码");
    }
    // String 方法的返回值必须是一个String类型的内容
    // 必须要有return返回
    public  static String test2(){
        String word = "天下风云出我辈，一入江湖岁月催。皇图霸业谈笑中，不胜人生一场醉。";
        System.out.println(word);
        // 结束方法，其后不能再写代码
        if(word.equals("天下风云出我辈，一入江湖岁月催。皇图霸业谈笑中，不胜人生一场醉。")) {
            // 结束方法
            return "abc"; // 返回到方法调用的地方
        }
        System.out.println("return之后代码");
        return "aaa";// 返回到方法调用的地方
    }
```

### break

```
// break
    public static void main(String[] args) {
        int x = 10;
        switch (x){
            case 1:
                break;  // 跳出switch语句，防止case语句穿透
            default:
                break;
        }
        //循环里面的break
        for (int i = 0; i < 10; i ++) {
            if (i == 5){
                break;// 跳出所在的循环
            }
            System.out.println(i);
        }
        System.out.println("break 跳出来执行此处");

        for(int i = 0; i < 3; i ++){
            for(int j = 0; j < 4; j ++){
                if(j == 2){
                    break;
                }
                System.out.println(j);
            }
        }
        
        boolean isRunning = true;
        while(isRunning) {
//            break;
            isRunning = false;
        }
    }
```

### continue

```
for (int i = 0; i < 10; i++) {
            if(i == 5) {
                continue; // 跳过本次循环，其后的循环代码不在执行
            }
            System.out.println(i);

        }
```

