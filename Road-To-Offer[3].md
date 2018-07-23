#### 计算机基础

> C++中**不能**重载`?:`、`.`、`::`、`sizeof`、`.*`
>
> **只能**用成员函数重载`=`、`()`、`[]`、`->`、`new`、`delete`

> 二叉树叶子结点在**前序**、**中序**、**后序**遍历**相对位置**不变

> 微指令周期=CPU时钟周期

> **补码**和**移码**零的表示唯一

> 任意二进制数可以表示为$N = S \times 2^P$
>
> **S**为尾数，**P**为阶码（小数点位置），**2**表示二进制底数

> 取不大于散列长度的**素数**作为模，减少冲突

#### Linux

> `fork` 全部复制父进程空间，
>
> `vfork` 共享内存
>
> `clone` 有选择的复制，其余共享

> 程序设置`setid`，普通用户会临时变成root权限，有效用户为root，实际用户仍为user

> `cron`表达式：**分 时 日 月 周 命令**

> 查看负载`top`、`uptime`

#### Java

> `byte`、`short`、`char` 进行计算时会提升为`int`

> Spring中没有提供日志系统，需借助AOP和log4j

> **泛型**在编译阶段会被擦除，不影响执行速度

> 截止JDK1.8的锁**读写锁、自旋锁、乐观锁**

> 一个字节的整数（-128～127）将被缓存至`IntegerCache` ，`new`创建的整数除外
>
> ```java
> Integer x1 = Integer.valueOf(17);
> Integer x2 = 17;
> Integer x3 = new Integer(17);
> System.out.println(x1 == x2);	// true
> System.out.println(x1 == x3);	// false
> 
> 
> ```

