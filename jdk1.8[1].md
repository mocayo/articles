#### JDK1.8特性[1]

---

##### 接口默认方法与静态方法

```java
public class Main {

    interface Calculate {
        // abstract method
        double add(int a, int b);

        // default method
        default double multiple(int a, int b) {
            return a * b;
        }
		
        // static method
        static double minus(int a, int b) {
            return a - b;
        }

    }

    static class Cal implements Calculate {
        @Override
        public double add(int a, int b) {
            return a + b;
        }
    }

    public static void main(String[] args){
        Calculate c = new Cal();
        System.out.println(c.add(5, 2));
        System.out.println(c.multiple(5, 2));
        System.out.println(Calculate.minus(5, 2));
    }
}
```

> **说明：**接口`Calculate`中`multiple(int a, int b)`方法有默认实现，子类实现接口时可以不用重写。`minus(int a, int b)`是静态方法，通过`Calculate.minus`调用。
>
> **运行结果：**
>
> ```java
> 7.0
> 10.0
> 3.0
> ```

----

##### Lambda表达式

1. Lambda表达式实例🌰

```java
import java.util.*;

public class Main {

	public static void main(String[] args){
		List<String> pets = Arrays.asList("cat", "bird", "dog", "miao");
		// 倒序
		Collections.sort(pets, new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				return o2.compareTo(o1);
			}
		});
		System.out.println(Arrays.toString(pets.toArray()));

		// 正序
		Collections.sort(pets, (String o1, String o2) -> o1.compareTo(o2));
		System.out.println(Arrays.toString(pets.toArray()));
	}
}
```

> **说明：**类似于JavaScript中的箭头表达式和python中的lambda表达式。在本例中通过Lambda表达式实现对pets列表排序，与以前的方法对比，代码更精简，可读性更好。（方便多了！）
>
> **运行结果：**
>
> ```java
> [miao, dog, cat, bird]
> [bird, cat, dog, miao]
> ```

2. Lambda表达式语法

- **基本语法：`左侧 -> 右侧 `**

  > **`左侧 `：**Lambda表达式参数列表
  >
  > **`  -> `  ：**箭头操作符/Lambda操作符
  >
  > **`右侧 `：**Lambda方法体

* **语法格式1：无参数，无返回值**

  > ```java
  > Runnable r = () -> System.out.println("Hello world!");
  > r.run()
  > ```

* **语法格式2：有1个参数，无返回值**

  > ```java
  > (x) -> System.out.println(x);
  > x -> System.out.println(x);
  > ```

* **语法格式3：多于2个参数，有返回值，Lambda方法体中多条语句**

  > ```java
  > Collections.sort(pets, (String o1, String o2) -> o1.compareTo(o2));
  > ```

* **语法格式4：Lambda体中只有一条语句时可省略return和大括号,参数列表数据类型可省略，JVM自动类型推断**

  > ```java
  > Comparator<String> c = (String o1, String o2) -> o1.compareTo(o2);
  > Comparator<String> c = (o1, o2) -> o1.compareTo(o2);
  > ```

3. 函数式接口

   Lambda在Java类型系统中的实现为：每个Lambda表达式都对应一个类型，通常是接口类型。函数式接口是指仅包含一个抽象方法的接口，每一个该类型对应的Lambda表达式都会匹配到这个抽象方法。(可以添加@FunctionalInterface注解，编译器如果发现该注解标注的接口多余1个抽象方法会报错。)

   > Java8中四大核心函数式接口
   >
   > ```java
   > // 消费型接口
   > interface Consumer<T> {
   >     void accept(T t);
   > }
   > // 供给型接口
   > interface Supplier<T> {
   >     T get();
   > }
   > // 函数型接口
   > interface Function<T, R> {
   >     R apply(T t);
   > }
   > // 断言型接口
   > interface Predicate<T> {
   >     boolean test(T t);
   > }
   > ```
   >
   >   

   **举个例子🌰**

   > ```java
   > public class Main {
   > 
   >     public static void main(String[] args){
   >         Consumer consumer = x -> System.out.println(x);
   >         consumer.accept("consumer");
   > 
   >         Supplier supplier = () -> System.nanoTime();
   >         consumer.accept(supplier.get());
   > 
   >         Function<String, Integer> function = (x) -> Integer.parseInt(x);
   >         consumer.accept(function.apply("100"));
   > 
   >         Predicate<String> predicate = (x) -> x.isEmpty();
   >         consumer.accept(predicate.test("tea"));
   >     }
   > }
   > ```
   >
   > **说明：**接口定义中的T、R为范型。定义consumer为控制台输出方法，supplier为获取系统当前纳秒数方法，function为将字符串转为整数方法，predicate为判断字符串x是否为空的断言方法。
   >
   > **运行结果：**
   >
   > ```java
   > consumer
   > 79065357161348
   > 100
   > false
   > ```

   **关于上述例子的另一种写法**

   > ```java
   > public class Main {
   > 
   >     public static void main(String[] args){
   >         // Consumer consumer = x -> System.out.println(x);
   >         Consumer consumer = System.out::println;
   >         consumer.accept("consumer");
   > 
   >         // Supplier supplier = () -> System.nanoTime();
   >         Supplier supplier = System::nanoTime;
   >         consumer.accept(supplier.get());
   > 
   >         // Function<String, Integer> function = (x) -> Integer.parseInt(x);
   >         Function<String, Integer> function = Integer::parseInt;
   >         consumer.accept(function.apply("100"));
   > 
   >         // Predicate<String> predicate = (x) -> x.isEmpty();
   >         Predicate<String> predicate = String::isEmpty;
   >         consumer.accept(predicate.test("tea"));
   >     }
   > }
   > ```
   >
   > **说明：**Java8中允许**`::`**来传递方法或构造函数引用，但需要保证方法参数类型、返回值类型兼容。

4. Lambda作用域

   在Lambda表达式中访问外部作用域的方式与匿名对象很类似，可以直接访问标记了final的外部局部变量，或实例的字段以及静态变量。

----

#### 反馈与建议

- 欢迎指正
- 邮箱：qihaiqh@qq.com