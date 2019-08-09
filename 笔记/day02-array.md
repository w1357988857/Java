# Array

### 异常

```
 static String[] sArr;

    public static void main(String[] args) {

        int[] iArr = new int[4];
        // 数组下标从0开始，最大值  长度-1   3
        System.out.println(iArr.length);
        System.out.println(iArr.length-1);
        // 数组通过下标访问
        System.out.println(iArr[1]);
        // ArrayIndexOutOfBoundsException (异常): 4（越界的位置）
        // at ......ArrException.main(ArrException.java:12)  在哪个类.哪个方法 哪一行代码
        // 没讲异常怎么处理之前，一旦发生异常，程序就会终止
        // System.out.println(iArr[4]);
        
        // lang.NullPointerException   运行时异常，俗称的BUG
        System.out.println(sArr.length);
    }
```

### 带有随机数

```
public static void main(String[] args) {
        // 整型数组，长度为5，为给他们依次赋值[0,100],之间的随机数
        int[] iArr = new int[5];
        for (int i = 0; i < iArr.length; i++) {
            // [0.0,1.0)->[0.0,101) double
            int score = (int)(Math.random() * 101);
            iArr[i] = score;
        }
        // [0.0,1.0)-> [0.0,80] + 20 ->[20.0,100]
        //  Math.random() * 81 + 20
        
        System.out.println("===========");
        // 数组中元素的类型 变量接收遍历得到的元素:数组
        for (int x:iArr) {
            System.out.println(x);
        }
    }
```

### 练习

```
public static void main(String[] args) {
        String[] sArr = {"公孙离","孙悟空","达摩","黄忠","妲己"};
        String[] sArr1 = new String[3];
        // 随机产生sArr的下标，将对应下标的值赋值给sArr1(产生3次赋值3次)
        // ，输出sArr1
        for (int i = 0; i <sArr1.length; i++) {
            int index = (int)(Math.random() * sArr.length);
            sArr1[i] = sArr[index];
        }
        for (int i = 0; i < sArr1.length; i++) {
            System.out.println(sArr1[i]);
        }
        
        for (String hero:sArr1 ) {
            System.out.println(hero);
        }
    }
```

## 求最大值

```
public static void main(String[] args) {
        // 随机产生一个5个元素的整型数组，求该数组中的最大值  [30-120]
        int[] iArr = new int[5];
        for (int i = 0; i < iArr.length; i++) {
            int data =(int) (Math.random() * 91) + 30;
            iArr[i] = data;
        }

        for (int x:iArr) {
            System.out.print(x + " ");
        }
        System.out.println();

        // 假设第一个是最大值，那这个值跟数组中其他的元素进行比较
        // 如果大于他，将这个大的值覆盖原最大值
        int maxValue = iArr[0];
        for (int i = 1; i < iArr.length; i++) {
            if (maxValue < iArr[i]){
                maxValue = iArr[i];
            }
        }
        System.out.println(maxValue);
    }
```

## 数组排序算法

### 二分查找

在一个有序数组中查找某个元素是否存在

数组 arr：最大小标  最小下标

（lowIndex + uperIndex）/ 2 -> middleIndex

arr[middleIndex] 与待查找元素进行比较，如果大于，说明在middleIndex的左边

​                                                                              如果小于，说明在middleIndex的右边

重新调整lowIndex 或 uperIndex -》middleIndex 在进行比较

如果找到了，条件是什么

如果没找到，条件又是什么

```
35 40 48 86 95 
```

```
static int[] iArr = {12,33,38,49,50,66};

    public static void main(String[] args) {
        binarySearch();
    }

    public static void binarySearch(){
        Scanner scanner = new Scanner(System.in);
        int value = scanner.nextInt();

        int lowIndex = 0;
        int upperIndex = iArr.length - 1;
        int middleIndex = 0;
        while(lowIndex <= upperIndex){ // 出口条件控制
            middleIndex = (lowIndex + upperIndex) / 2;
            if(iArr[middleIndex] == value){
                System.out.println(middleIndex);
                break;
            } else if(iArr[middleIndex] > value){
                upperIndex = middleIndex-1;
            } else {
                lowIndex = middleIndex + 1;
            }
        }

        if(lowIndex > upperIndex) {
            // 怎样判断没有找到
            System.out.println("没有找到");
        }
    }
```

### 冒泡排序

从小到大排序，两两比较，如果前面的大于后面，就交换他们的位置，经过多轮比较之间，实现从小到大

```
 48  95  35 86 40 
 48  35  86 40 95
 35  48  40 86 95
 35  40  48 86 95
 35  40  48 86 95
```

```
static int[] iArr = {48,95,35,86,40};
    public static void main(String[] args) {
    	//length：属性，不用加括号
        for (int i = 0 ; i < iArr.length-1; i ++) {
            for (int j = 0; j < iArr.length -  i - 1; j++) {
                if(iArr[j] > iArr[j+1]){
                    //  交换两个元素的位置，需要引入第三个变量
                    int temp = iArr[j];
                    iArr[j] = iArr[j+1];
                    iArr[j+1] = temp;
                }
            }
        }

        for (int x:iArr) {
            System.out.println(x);
        }

    }
```

改良的冒泡排序-》快速排序

### 选择排序

```
48,95,35,86,40->35,95,48,86,40
35,95,48,86,40->35,48,95,86,40->35 40 95 86 48
35 40 95 86 48->35 40 86 95 48 ->35 40 48 86 95 
35 40 48 86 95 ->
```

```
static int[] iArr = {48,95,35,86,40};
    
    public static void main(String[] args) {
        for (int i = 0; i < iArr.length-1; i++) {
            for (int j = i; j < iArr.length; j++) {
                if(iArr[i] > iArr[j]){
                    int temp = iArr[i];
                    iArr[i] = iArr[j];
                    iArr[j] = temp;
                }
            }
        }
        for (int x:iArr) {
            System.out.println(x);
        }
    }
```

## Arrays

```
static int[] iArr = {32,34,12,2,11,22,45};

    public static void main(String[] args) {
        // ascending升序   desc降序
        Arrays.sort(iArr);
        //foreach语句
        for (int x:iArr) {
            System.out.println(x);
        }

        // 返回大于等于0,表示查找到了数据，否则没有找到
        int index = Arrays.binarySearch(iArr, 32);
        System.out.println(index);
    }
```

