
#### JDK1.7特性
----
- **`switch`语句支持字符串变量**
``` java
public String eatWhat (String day) {
  switch (day):
    case "today": 
      return "meat";
      break;
    case "tomorrow": 
      return "fisk";
      break;
    case "yesterday": 
      return "chicken";
      break;
    default: 
      return "steak";
}
```
>`switch`语句比较表达式中的`String`对象，并关联`case`标签，相当于`if-then-else`。

- **泛型自动推断类型**
``` java
ArrayList<String> al1 = new ArrayList<String>();    // Old
ArrayList<String> al2 = new ArrayList<>();          // New
```
>上述两行代码等价

- **整数表达式**
> - 表示二进制前缀`0b`
``` java
byte b1 = 0b1001;
byte b2 = 0x9;
```
> - 连数符 `-`，用下划线连接整数提高可读性，只用于数字之间。
``` java
int a1 = 5_1;
int a2 = 52_;
```

- **单个`catch`中捕获多个异常**

```java
// Before Java7 
try {
  // ...
} catch (FileNotFoundException ex1) {
  // do something
} catch (IOException ex2) {
  // do something
} finally {
  // ...
}
// Java7
try {
  // ...
} catch (FileNotFoundException | IOException ex) {
  throw new MyException(ex.getMessage());
} finally {
  // ...
}
```

- **新增`try-with-resource`语句**

> `try-with-resource`声明一个或多个资源，并确保最后每个资源都被关闭。
> 类似于Python中的`with`语句块。

``` java
// Before Java7
public static void main(String[] args) {
    FileInputStream inputStream = null;
    try {
        inputStream = new FileInputStream(new File("test"));
        System.out.println(inputStream.read());
    } catch (IOException e) {
        throw new RuntimeException(e.getMessage(), e);
    } finally {
        if (inputStream != null) {
            try {
                inputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e.getMessage(), e);
            }
        }
    }
}
// Java7
public static void main(String[] args) {
    try (FileInputStream inputStream = new FileInputStream(new File("test"))) {
        System.out.println(inputStream.read());
    } catch (IOException e) {
        throw new RuntimeException(e.getMessage(), e);
    }
}
```

#### 反馈与建议
---
- 欢迎指正
- 邮箱：<qihaiqh@qq.com>
