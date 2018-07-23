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

1. Stream基本操作

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
- `isPresent()` : 判断是否包含值  - `orElse(T t)` : 如果调用对象包含值，返回该值，否则返回t 
- `orElseGet(Supplier s)` :如果调用对象包含值，返回该值，否则返回s 获取的值 
- `map(Function f)`: 如果有值对其处理，并返回处理后的Optional，否则返回Optional.empty() 
- `flatMap(Function mapper)`:与map 类似，要求返回值必须是Optional

