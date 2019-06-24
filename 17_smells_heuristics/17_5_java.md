## 17.5 Java

### J1: 通过使用通配符避免过长的导入清单（Avoid Long Import Lists by Using Wildcards）

如果使用了来自同一包的两个或多个类，用通配符导入：`import package.*`。

> 仅仅是为了减少代码行？

### J2: 不要继承常量（Don’t Inherit Constants）

不要在子类中访问父类的常量。因为不方便找到常量究竟定义在哪个类里。

> 通过IDEA可以方便地找到呀。。。


### J3: 常量 vs. 枚举（Constants versus Enums）

推荐用枚举。

