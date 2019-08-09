# 单元测试

单元测试是编写测试代码，应该准确、快速地保证程序基本模块的正确性。强调建立测试数据的一段代码，可以先测试，然后再应用。这个方法就好比“测试一点，编码一点，测试一点，编码一点……”，增加了程序员的产量和程序的稳定性，可以减少程序员的压力和花费在排错上的时间。

在IDEA中**安装**单元测试插件：

```
File->Settings->Plugins(左侧菜单)->JUnitGenerator 2.0(输入框中)->Restart
```

**配置插件**

Settings->Other Settings->JUnit Generator(点击左侧)–>Properties–>修改Output Path为${SOURCEPATH}/test/java/${PACKAGE}/${FILENAME} –>**修改Default Template为JUnit4** –>**点击JUnit4页签 –>将package test.$entry.packageName;修改成package $entry.packageName; –>点击OK[确定]保存并退出设置**

**创建并配置测试目录**

创建一个和src同级别的文件夹叫test(逻辑代码放src里，测试代码放test里，项目-右击-New->Directory->test)。接着在IntelliJ IDEA里还要把这个test文件夹要设置成测试文件的根目录，test上右击Mark Directory As - Test Sources Root。

**创建测试类**

进入需要进行单元测试的类，通过Cmd + Shift + T创建测试类，勾选需要测试的方法，点击确认。**Create New Test**

```
public class MathTest {
    public int  testAdd(int x, int y){
        System.out.println(x + y);
        return x + y;
    }

    public int  testSub(int x, int y){
        System.out.println(x - y);
        return x - y;
    }
}
```

![](img/10.png)

![](img/11.png)

**若显示如上library not found的问题，请参照如下问题解决**

**问题**

 java: 程序包org.junit不存在

File -> Project Struct... -> Libraies -> 点击绿色的加号 -> Java -> 找到 IDEA 安装路径下的 Lib 中的junit-4.12 -> 确定完就行了

![](img/09.png)

如上一次添加如下类库：

![](img/12.png)

```
public class MathTestTest {
    @Before
    public void setUp() throws Exception {
        System.out.println(11);
    }
    @After
    public void tearDown() throws Exception {
    }
    @Test
    public void testAdd() {
        assertEquals(30,new MathTest().testAdd(10,20));
    }
    @Test
    public void testSub() {
        new MathTest().testSub(10,20);
    }
}
```

**语法**

- @BeforeClass 全局只会执行一次，而且是第一个运行
- @Before 在测试方法运行之前运行
- @Test 测试方法
- @After 在测试方法运行之后允许
- @AfterClass 全局只会执行一次，而且是最后一个运行

```
public class MathTestTest {

    @Before
    public void setUp() throws Exception {
        System.out.println("setUp");
    }
    @After
    public void tearDown() throws Exception {
        System.out.println("tearDown");
    }
    @Test
    public void testAdd() {
        System.out.println("add");
    }
    @Test
    public void testSub() {
        System.out.println("sub");
    }

    @BeforeClass
    public static void beforeOne(){
        System.out.println("beforeOne");
    }

    @AfterClass
    public static void afterOne(){
        System.out.println("afterOne");
    }
}
```

输出

```
beforeOne
setUp
add
tearDown
setUp
sub
tearDown
afterOne
```



