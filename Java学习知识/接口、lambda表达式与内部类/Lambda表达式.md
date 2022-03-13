**参考：[Lambda表达式超详细总结_](https://blog.csdn.net/huangjhai/article/details/107110182?ops_request_misc=%7B%22request%5Fid%22%3A%22164455914416780274112855%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=164455914416780274112855&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-107110182.first_rank_v2_pc_rank_v29&utm_term=lambda表达式&spm=1018.2226.3001.4187)和 卷一P243-255**

Lambda表达式是一个可传递的代码块，可以在以后执行一次或多次。

在Java中，lambda表达式就是闭包。

闭包：代码块以及自由变量值的一个术语（自由变量指的是代码块以外的变量）

```java
class Worker implements ActionListener{
    public void actionPerformed(ActionEvent event){
        //do some work
    }
}
```

想要反复执行这个代码时，可以构造Worker类的一个实例。

## Lambda表达式的语法

```java
(String first, String second)
	-> first.length() - second.length()
```

lambda表达式就是一个代码块，以及必须传入代码的变量规范。

## 表达式的两个部分

Lambda表达式在Java语言中引入了一个操作符**“->”**，该操作符被称为Lambda操作符或箭头操作符。它将Lambda分为两个部分：

* **左侧：指定了Lambda表达式需要的所有参数**
* **右侧：制定了Lambda体，即Lambda表达式要执行的功能。**
  **像这样：**

```java
(parameters) -> expression
或
(parameters) ->{ statements; }
```

参考《卷一》P251内容，lambda表达式有三个部分：

1. **一个代码块**：  即  “->”  后面的代码		**{**
     **     				 **System.out.println(text);**
          				 **Toolkit.getDefaultToolkit().beep();**
      				 }**
2. **参数**：	         即（）小括号中的参数       **event**
3. **自由变量的值**：指非参数而且不在代码中定义的变量，是表达式外的变量。    **text**

示例：

```java
public static void repeatMessage(String text , int delay){
    ActionListener listener = event ->{
        System.out.println(text);
        Toolkit.getDefaultToolkit().beep();
    };
    new Timer(delay, listener).start();
}

//使用下面的代码进行调用
repeatMessage("Hello", 1000);	//prints Hello every 1000 milliseconds
```

## Lambda表达式的重要特征:

1. 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
2. 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
3. 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
4. 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。

## 特征例子：

(1)语法格式一：**无参，无返回值**，Lambda体只需一条语句。如下：

```java
  @Test
  public void test01(){
    Runnable runnable = ()-> System.out.println("Runnable 运行");
    runnable.run();//结果：Runnable 运行
  }
```

(2)语法格式二：Lambda需要**一个参数，无返回值**。如下：

```java
  @Test
  public void test02(){
    Consumer<String> consumer=(x)-> System.out.println(x);
    consumer.accept("Hello Consumer");//结果：Hello Consumer
  }
```

​	也可以写成**省略参数的小括号**形式：

```java
  public void test02(){
    Consumer<String> consumer=x-> System.out.println(x);
    consumer.accept("Hello Consumer");//结果：Hello Consumer
  }
```

（3）语法格式三：Lambda需要**两个参数**，并且Lambda体中有**多条语句**。

```java
  @Test
  public void test04(){
    Comparator<Integer> com=(x, y)->{
      System.out.println("函数式接口");
      return Integer.compare(x,y);
    };
    System.out.println(com.compare(2,4));//结果：-1
  }
```

（4）语法格式四：有**两个以上参数**，**有返回值**，若Lambda体中**只有一条语句**，**return和大括号都可以省略不写**

```java
  @Test
  public void test05(){
    Comparator<Integer> com=(x, y)-> Integer.compare(x,y);
    System.out.println(com.compare(4,2));//结果：1
  }
```

（5）Lambda表达式的**参数列表的数据类型可以省略不写**，因为**JVM可以通过上下文推断出数据类型，即“类型推断”**

```java
  @Test
  public void test06(){
    Comparator<Integer> com=(Integer x, Integer y)-> Integer.compare(x,y);
    System.out.println(com.compare(4,2));//结果：1
  }
```

类型推断：在执行**javac**编译程序时，JVM根据程序的上下文推断出了参数的类型。Lambda表达式依赖于上下文环境。

语法背诵口诀：**左右遇一括号省，左侧推断类型省，能省则省。**

## 函数式接口

​	**定义：**对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。这种接口称为函数式接口（functional interface）。我们可以在任意函数式接口上使用**@FunctionalInterface注解**，这样做可以用于检测它是否是一个函数式接口，同时javadoc也会包含一条声明，说明这个接口是一个函数式接口。

```java
public class FunctionalInterfaceTest {
    public static void main(String[] args) {

        //3、将Lambda表达式实现的接口作为参数传递
        String value = toLowerStr((str)->
           str = "abcdefg", "ABC");

        System.out.println(value);  //abc

        //这个写法是引用了parseInt方法作为接口的实现方法
        int num = getNumber(Integer::parseInt, "1234");
        System.out.println(num);

    }

    //2、创建一个接受函数式接口作为参数的方法
    public static String toLowerStr(MyFunctionalF<String> mf, String origin){
        return mf.getValue(origin);
    }

    //弄一个字符串装int的一个方法，一样使用lambda
    public static int getNumber(MyFunctionalF<Integer> toNum, String origin){
        return toNum.getValue(origin);
    }
}
//1、先创建一个函数式接口
@FunctionalInterface
interface MyFunctionalF<T> {
    public T getValue(String origin);
}
```

**总结：**

​		一个方法接受一个函数式接口作为参数，则可以使用lambda表达式来创建这个接口，lambda表达式中的代码即为函数式接口中的抽象方法的具体实现的代码。

## Java内置函数式接口：

有关这些内置函数式接口的理解代码在[代码位置](E:\Project01\JavaCoreTechnologyVolume1\src\Chapter6\Study_Lambda\Built_inFunctionalInterface\Test.java)中

四大核心函数式接口的介绍，如图所示：

![](C:\Users\清悠樱落\Documents\Typora文件\Java学习知识\接口、lambda表达式与内部类\函数式接口.png)

**其他接口的定义**

![](C:\Users\清悠樱落\Documents\Typora文件\Java学习知识\接口、lambda表达式与内部类\其他函数式接口.png)

## 方法引用

​		当要传递给Lambda体的操作，已经**有实现的方法**了，就可以使用方法引用！（实现抽象方法的参数列表，必须与方法引用的参数列表一致，方法的返回值也必须一致，即**方法的签名一致**）。方法引用可以理解为**方法引用是Lambda表达式的另外一种表现形式**。
方法引用的语法：**使用操作符“::”将对象或类和方法名分隔开**。
**方法引用的使用情况**共分为以下三种：

- **对象::实例方法名**
- **类::静态方法名**
- **类::实例方法名**

使用示例：

1、对象::实例方法名

```java
  /**
   *PrintStream中的println方法定义 
   *     public void println(String x) {
   *         synchronized (this) {
   *             print(x);
   *             newLine();
   *         }
   *     }
   */
  //对象::实例方法名
  @Test
  public void test1(){
    PrintStream out = System.out;
    Consumer<String> consumer=out::println;
    consumer.accept("hello");
  }

```

2、类::静态方法名

```java
    /**
     * Integer类中的静态方法compare的定义：
     *     public static int compare(int x, int y) {
     *         return (x < y) ? -1 : ((x == y) ? 0 : 1);
     *     }
     */
  @Test
  public void test2(){
    Comparator<Integer> comparable=(x,y)->Integer.compare(x,y);
    //使用方法引用实现相同效果
    Comparator<Integer> integerComparable=Integer::compare;
    System.out.println(integerComparable.compare(4,2));//结果：1
    System.out.println(comparable.compare(4,2));//结果：1
  }

```

3、类::实例方法名

```java
  @Test
  public void test3(){
    BiPredicate<String,String> bp=(x,y)->x.equals(y);
    //使用方法引用实现相同效果
    BiPredicate<String,String> bp2=String::equals;
    System.out.println(bp.test("1","2"));//结果：false
    System.out.println(bp2.test("1","2"));//结果：false
  }

```

注意：方法引用中，可以使用this参数和super参数，

自我总结：

​		方法引用：一个方法中以接口作为参数，接口的抽象方法可以引用lambda表达式提供的方法；即转换为函数式接口的实例。

## 方法引用与lambda表达式的细微区别

​		考虑一个方法引用，如separator：：equals。如果separator为null，构造separator：：equals时就会立即抛出一个NullPointException异常。如果lambda表达式x -> separator。equals（x）只在调用时才会抛出NullPointException。

## 构造器引用

格式：类名：：new；

注意：构造器参数列表要与接口中抽象方法的参数列表一致。

示例：

```java
//Employee类
public class Employee {
    private int id;
    private String name;
    private int age;

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    
    public Employee() {

    }
    
    public Employee(int id) {
        this.id = id;
    }

    public Employee(int id, int age) {
        this.id = id;
        this.age = age;
    }

    public Employee(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
}
```

```java
//程序入口，main的位置
public class Test01 {
    public static void main(String[] args) {

        ///传统lambda表达式
        Function<Integer, Employee> lambdaSupplier = (id)-> new Employee(id);
        Employee employee = lambdaSupplier.apply(100);
        System.out.println(employee);

        //引用无参构造器
        Supplier<Employee> supplier=Employee::new;
        System.out.println(supplier.get());

        //引用有参构造器
        Function<Integer,Employee> function=Employee::new;  //引用了一个参数的构造器
        System.out.println(function.apply(21));

        BiFunction<Integer,Integer,Employee> biFunction=Employee::new;  //引用了两个参数的构造器
        System.out.println(biFunction.apply(8,24));
        
        /*
        输出结果：
            Employee{id=100, name='null', age=0}
            Employee{id=0, name='null', age=0}
            Employee{id=21, name='null', age=0}
            Employee{id=8, name='null', age=24}
         */
    }

}
```

总结：当你使用构造器使用时，要与函数式接口中的方法签名是一致的；引用好构造器后，即可使用接口提供的方法来构造一个对象。

## 数组引用

数组引用与构造器引用基本相同

示例：

```java
//Employee类与构造器引用中的相同
public class Test02 {
    public static void main(String[] args) {

        //使用传统的lambda表达式实现数组引用
        Function<Integer, int[]> newInteger = (len)->new int[len];
        int[] arrays = newInteger.apply(5);
        System.out.println("使用lambda表达式时，数组的长度：" + arrays.length);
        //使用lambda表达式时，数组的长度：5

        //使用数组引用
        newInteger = int[]::new;
        arrays = newInteger.apply(10);
        System.out.println("使用数组引用时，数组的长度是：" + arrays.length);
        //使用数组引用时，数组的长度是：10

        //使用类似的方法来引用Employee的构造器
        Function<Integer, Employee[]> employeeFunction = Employee[]::new;
        Employee[] employees = employeeFunction.apply(20);
        System.out.println("使用数组引用来构造的Employees数组长度为："+employees.length);
        //使用数组引用来构造的Employees数组长度为：20

    }
}
```

总结：由于本次示例中的构造器都是使用着一个形参的构造器，如果有不同个数的形参，请使用签名一致的接口来保存起数组引用。（参考**Java内置函数式接口**）

