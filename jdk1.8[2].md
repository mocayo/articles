#### JDK1.8特性[2]

----

##### Stream接口

` java.util.Stream` 表示能应用在一组元素上一次执行的操作序列。

Stream 操作分为中间操作或者最终操作两种，最终操作返回一特定类型的计算结果，中间操作返回Stream本身，这样就可以将多个操作依次串起来（链式）。

Stream 的创建需要指定数据源，比如 `java.util.Collection`的子类，List或者Set， Map**不支持**。

Stream的操作可以串行执行或者并行执行。

1. 举个例子🌰

> ```java
> public class Main {
> 
>     public static void main(String[] args){
>         List<String> pets = Arrays.asList("cat", "bird", "dog", "miao", "pig");
>         pets.stream()
>                 .filter(s -> s.length() == 3)
>                 .sorted((s1, s2) -> s2.compareTo(s1))
>                 .forEach(System.out::println);
>     }
> }
> ```
>
> **说明：**上述代码段中定义了pets列表，通过`stream()`方法获取pets对应的`Stream<String>`对象，经过过滤操作、排序操作，最终打印`Stream<String>`对象，中间操作不会改变原来的pets列表。
>
> **运行结果：**
>
> ```java
> pig
> dog
> cat
> ----
> cat
> bird
> dog
> miao
> pig
> ```

2. Stream基本操作

- allMatch – 检查是否匹配所有元素 
- anyMatch – 检查是否至少匹配一个元素  
- noneMatch – 检查是否没有匹配所有元素 
- findFirst – 返回第一个元素 
- count – 返回流中元素的总个数  
- max – 返回流中最大值 
- min – 返回流中最小值
- map / reduce
- collect – 将流转换成其他格式，接受一个Collector接口的实现，用于给Stream中元素做汇总的操作

------

##### Optional类

`Optional<T> 类(java.util.Optional)` 是一个容器类，代表一个值存在或不存在，原来用null 表示一个值不存在，现在Optional 可以更好的表达这个概念，并且可以避免空指针异常。

1. 常用方法

- `Optional.of(T t)` : 创建一个Optional 实例  
- `Optional.empty()` : 创建一个空的Optional 实例  
- `Optional.ofNullable(T t)`:若t 不为null,创建Optional 实例,否则创建空实例  
- `isPresent()` : 判断是否包含值 
- `orElse(T t)` : 如果调用对象包含值，返回该值，否则返回t 
- `orElseGet(Supplier s)` :如果调用对象包含值，返回该值，否则返回s 获取的值 
- `map(Function f)`: 如果有值对其处理，并返回处理后的Optional，否则返回Optional.empty() 
- `flatMap(Function mapper)`:与map 类似，要求返回值必须是Optional

2. 举个例子🌰

* Optional.of()或者Optional.ofNullable()：创建Optional对象，差别在于of不允许参数是null，而ofNullable则无限制

  ```java
  // 参数不能是null
  Optional<Integer> optional1 = Optional.of(1);
   
  // 参数可以是null
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  // 参数可以是非null
  Optional<Integer> optional3 = Optional.ofNullable(2);
  ```

* Optional.empty()：所有null包装成的Optional对象

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(null);
  Optional<Integer> optional2 = Optional.ofNullable(null);
  System.out.println(optional1 == optional2);// true
  System.out.println(optional1 == Optional.<Integer>empty());// true
   
  Object o1 = Optional.<Integer>empty();
  Object o2 = Optional.<String>empty();
  System.out.println(o1 == o2);// true
  ```

* Optional.isPresent()：判断值是否存在

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  // isPresent判断值是否存在
  System.out.println(optional1.isPresent() == true);
  System.out.println(optional2.isPresent() == false);
  ```

* Optional.ifPresent(Consumer consumer)：如果option对象保存的值不是null，则调用consumer对象，否则不调用

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  // 如果不是null,调用Consumer
  optional1.ifPresent(new Consumer<Integer>() {
  	@Override
  	public void accept(Integer t) {
  		System.out.println("value is " + t);
  	}
  });
   
  // null,不调用Consumer
  optional2.ifPresent(new Consumer<Integer>() {
  	@Override
  	public void accept(Integer t) {
  		System.out.println("value is " + t);
  	}
  });
  ```

* Optional.orElse(value)：如果optional对象保存的值不是null，则返回原来的值，否则返回value

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  // orElse
  System.out.println(optional1.orElse(1000) == 1);// true
  System.out.println(optional2.orElse(1000) == 1000);// true
  ```

  

* Optional.orElseGet(Supplier supplier)：功能与orElse一样，只不过orElseGet参数是一个对象

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  System.out.println(optional1.orElseGet(() -> {
  	return 1000;
  }) == 1);//true
   
  System.out.println(optional2.orElseGet(() -> {
  	return 1000;
  }) == 1000);//true
  ```

  

* Optional.orElseThrow()：值不存在则抛出异常，存在则什么不做

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  optional1.orElseThrow(()->{throw new IllegalStateException();});
   
  try
  {
  	// 抛出异常
  	optional2.orElseThrow(()->{throw new IllegalStateException();});
  }
  catch(IllegalStateException e )
  {
  	e.printStackTrace();
  }
  ```

  

* Optional.filter(Predicate)：判断Optional对象中保存的值是否满足Predicate，并返回新的Optional

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  Optional<Integer> filter1 = optional1.filter((a) -> a == null);
  Optional<Integer> filter2 = optional1.filter((a) -> a == 1);
  Optional<Integer> filter3 = optional2.filter((a) -> a == null);
  System.out.println(filter1.isPresent());// false
  System.out.println(filter2.isPresent());// true
  System.out.println(filter2.get().intValue() == 1);// true
  System.out.println(filter3.isPresent());// false
  ```

* Optional.map(Function)：对Optional中保存的值进行函数运算，并返回新的Optional(可以是任何类型)

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
  Optional<Integer> optional2 = Optional.ofNullable(null);
   
  Optional<String> str1Optional = optional1.map((a) -> "key" + a);
  Optional<String> str2Optional = optional2.map((a) -> "key" + a);
   
  System.out.println(str1Optional.get());// key1
  System.out.println(str2Optional.isPresent());// false
  ```

  

* Optional.flatMap()：功能与map()相似，区别在于mapping函数的返回值不同。map方法的mapping函数返回值可以是任何类型T，而flatMap方法的mapping函数必须是Optional

  ```java
  Optional<Integer> optional1 = Optional.ofNullable(1);
   
  Optional<Optional<String>> str1Optional = optional1.map((a) -> {
  	return Optional.<String>of("key" + a);
  });
   
  Optional<String> str2Optional = optional1.flatMap((a) -> {
  	return Optional.<String>of("key" + a);
  });
   
  System.out.println(str1Optional.get().get());// key1
  System.out.println(str2Optional.get());// key1
  ```

----

##### 其他

1. **HashMap**中链长度大于8时采取红黑树的结构存储
2. JDK1.8没有永久区，取而代之的是**MetaSpace元空间**，用的是物理内存
3. **Nashorn, JavaScript 引擎** − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。
4. **Date Time API** − 加强对日期与时间的处理。 

----

#### 反馈与建议

- 欢迎指正
- 邮箱：qihaiqh@qq.com

