##### Java

> Java **赋值运算**有返回值，赋什么就返回什么

> null地址为`0x0000`，用户不能使用此地址，不能赋值给基本类型变量

> 变量不能被`default`修饰

> `final`声明的方法不能重写，父类中的`private`方法不能重写
>
> ```java
> class Foo {
>     private void method1() {}
>     final void method2() {}
>     final private method3() {}
> }
> class Child extends Foo {
>     void method1() {} // yes
>     void method2() {} // no
>     void method3() {} // yes
> }
> ```

> Java中接口可多继承
>
> ```java
> interface A {
>     int x = 1; // avoid variable with the same name
> }
> 
> interface B {
>     int x = 2; // avoid variable with the same name
> }
> 
> interface C extends A, B {
>     
> }
> ```
>
> 如上面代码所示，若接口A和接口B中有相同的方法，则在接口C中只保留**一个**声明。
>
> 但要注意不要出现同名变量，否则编译器认为` java: reference to xxx is ambiguous`

##### Linux

> DHCP服务器的配置文件为`/etc/dhcpd.conf`，默认不存在，需要手工创建

> `pwrite`对文件随机写，是系统调用

##### 内存分配区域

> | 类别               | 存储区        |
> | ------------------ | ------------- |
> | `new/malloc`       | 堆区          |
> | 函数局部变量和参数 | 栈区          |
> | 全局和静态         | 全局区/静态区 |
>

##### 算法

> **最短路径算法**
>
> | 算法         | 时间复杂度 | 描述                   |
> | ------------ | ---------- | ---------------------- |
> | Dijkstra     | O(n^2)     | 单源最短路径，权值非负 |
> | Floyd        | O(n^3)     | 每对节点间的最短路径   |
> | Bellman-Ford | O(VE)      | 单源最短路径，包含负权 |

> **生成树的数量等于回路边数**

##### DBMS事务特性

> | 特性                      | 机制       |
> | ------------------------- | ---------- |
> | 原子性（**A**tomicity）   | 完整性管理 |
> | 一致性（**C**onsistency） | 并发控制   |
> | 隔离性（**I**solation）   | 安全控制   |
> | 持久性（**D**urability）  | 恢复管理   |