## 17.4 一般性问题（General）

### G1: 一个源文件中存在多种语言（Multiple Languages in One Source File）

### G2: 明显的行为未被实现（Obvious Behavior Is Unimplemented）
Following “The Principle of Least Surprise”, any function or class should implement the behaviors that another programmer could reasonably expect.

``` java 
Day day = DayDate.StringToDay(String dayName);
```

Reasonable expectations:
- "Monday" --> Day.MONDAY
- "monday" --> Day.MONDAY
- "Mon" -- Day.MONDAY

### G3: 不正确的边界行为（Incorrect Behavior at the Boundaries） 

Don’t rely on your intuition. 
Look for every boundary condition and write a test for it.

### G4: 忽视安全（Overridden Safeties）
Turning off certain compiler warnings (or all warnings!) may help you get the build to succeed, but at the risk of endless debugging sessions. 

### G5: 重复（Duplication）

DRY: Don't Repeat Yourself. 这是本书最重要的规则之一
- 重复代码代表遗漏了抽象。

### G6: 在错误的抽象层级上的代码（Code at Wrong Level of Abstraction）
``` java
public interface Stack {
  Object pop() throws EmptyException;
  void push(Object o) throws FullException; 
  double percentFull();
}  
```
- percentFull()方法应该放到子接口`BoundedStack`上。

### G7: 基类依赖于派生类（Base Classes Depending on Their Derivatives）
In general, base classes should know nothing about their derivatives.

### G8: 信息过多（TMI, Too Much Information）
- 类或模块中暴露的接口越少越好；
- 类中的方法越少越好；
- 函数知道的变量越少越好；
- 类拥有的实体变量越少越好；

### G9: 死代码（Dead Code）
- 不被调用的方法；
- 不会进入的if/switch条件；
- 从不抛出异常的try-catch；

### G10: 垂直分隔（Vertical Separation）
垂直距离要短，
- 变量和函数应该在靠近被使用的地方定义。
- 本地变量应该在首次被使用的位置上面声明。
- 私有函数应该刚好在其首次被调用的位置下面定义。

### G11: 前后不一致（Inconsistency）
If you do something a certain way, do all similar things in the same way. 
- `HttpServletResponse response;` vs. `HttpServletResponse httpResponse;`
- `loadInterstellarVendorItem()` vs. `getInterstellarVendorItem()`

### G12: 混淆视听（Clutter）
- 没有用到的变量；
- 从不调用的函数；
- 没有信息量的注释；
All these things are clutter and should be removed. 

### G13: 人为耦合（Artificial Coupling）
不互相依赖的东西就不该耦合。 


### G14: 特性依恋 （Feature Envy）
see Martin Fowler’s Refactoring.

The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes.

``` java
public class HourlyPayCalculator {
  public Money calculateWeeklyPay(HourlyEmployee e) {
    int tenthRate = e.getTenthRate().getPennies();
    int tenthsWorked = e.getTenthsWorked();
    int straightTime = Math.min(400, tenthsWorked);
    int overTime = Math.max(0, tenthsWorked - straightTime); 
    int straightPay = straightTime * tenthRate;
    int overtimePay = (int)Math.round(overTime*tenthRate*1.5);
    return new Money(straightPay + overtimePay); 
  }
}

```

> The calculateWeeklyPay method envies the scope of HourlyEmployee. It “wishes” that it could be inside HourlyEmployee.


但事无绝对，下面这个reportHours方法如果移到HourlyEmployee类中，就会违反SRP原则。
``` java
public class HourlyEmployeeReport { 
  private HourlyEmployee employee ;
  public HourlyEmployeeReport(HourlyEmployee e) { 
    this.employee = e;
  }
  
  String reportHours() { 
    return String.format("Name: %s\tHours:%d.%1d\n", 
      employee.getName(), 
      employee.getTenthsWorked()/10, 
      employee.getTenthsWorked()%10);
  } 
}
```

### G15: 选择算子参数（Selector Arguments）

什么是selector arguments:用于选择函数行为的参数。
- boolean
- enum
- ...

``` java
public int calculateWeeklyPay(boolean overtime) { //Selector Argument
  int tenthRate = getTenthRate();
  int tenthsWorked = getTenthsWorked();
  int straightTime = Math.min(400, tenthsWorked);
  int overTime = Math.max(0, tenthsWorked - straightTime); 
  int straightPay = straightTime * tenthRate;
  double overtimeRate = overtime ? 1.5 : 1.0 * tenthRate; 
  int overtimePay = (int)Math.round(overTime*overtimeRate); 
  return straightPay + overtimePay;
}
```

### G16: 晦涩的意图（Obscured Intent）

``` java
public int m_otCalc() { 
  return iThsWkd * iThsRte +
       (int) Math.round(0.5 * iThsRte * 
          Math.max(0, iThsWkd - 400)
       ); 
}
```

这个函数短小紧凑，但究竟是在做什么事情呢？

### G17: 位置错误的权责（Misplaced Responsibility）

Question: PI常量应该放在Math类、Trigonometry类、还是Circle类？
- Following “The Principle of Least Surprise”. Code should be placed where a reader would naturally expect it to be.


### G18: 不恰当的静态方法（Inappropriate Static）

恰当的静态方法：
``` java
Math.max(double a, double b)

// to avoid:
new Math().max(a, b);
a.max(b);
```

不恰当的静态方法：
``` java
HourlyPayCalculator.calculatePay(employee, overtimeRate).
```
原因：有理由希望这个函数是多态的。（OvertimeHourlyPayCalculator, StraightTimeHourlyPayCalculator）

> 可以作为Employee方法的非静态函数。


### G19: 使用解释性变量（Use Explanatory Variables）
让程序可读的有力方法之一就是将计算过程打散，用有意义的变量名存储中间值。

``` java
Matcher match = headerPattern.matcher(line); 
if(match.find()) {
  String key = match.group(1); //中间值
  String value = match.group(2); //中间值 
  headers.put(key.toLowerCase(), value);
}
```


### G20: 函数名称应该表达其行为（Function Names Should Say What They Do）
反例：
``` java
Date newDate = date.add(5);
```

正例：
``` java
Date newDate = date.addDaysTo(5);
Date newDate = date.increaseByDays(5);
```

### G21: 理解算法（Understand the Algorithm）
Before you consider yourself to be done with a function, make sure you understand how it works. 
It is not good enough that it passes all the tests. 
You must know that the solution is correct. 

### G22: 把逻辑依赖改为物理依赖（Make Logical Dependencies Physical） 

Logical Dependency:
- The dependent module should not make **assumptions** about the module it depends upon.

Physical Dependency:
- it should explicitly ask that module for all the information it depends upon.

``` java
public class HourlyReporter {
  private HourlyReportFormatter formatter; 
  private List<LineItem> page;
  private final int PAGE_SIZE = 55;
  
  public void generateReport(List<HourlyEmployee> employees) { 
    for (HourlyEmployee e : employees) {
      addLineItemToPage(e);
      if (page.size() == PAGE_SIZE)
        printAndClearItemList(); 
    }
    if (page.size() > 0) 
      printAndClearItemList();
  }
  
  private void printAndClearItemList() { 
    formatter.format(page); 
    page.clear();
  }
  
  private void addLineItemToPage(HourlyEmployee e) { 
    LineItem item = new LineItem();
    item.name = e.getName();
    item.hours = e.getTenthsWorked() / 10;
    item.tenths = e.getTenthsWorked() % 10;
    page.add(item); 
  }
```

PAGE_SIZE应该是HourlyReportFormatter的职责； 此处HourlyReporter被假定知道PAGE_SIZE，所以是逻辑依赖。

- We can physicalize this dependency by creating a new method in `HourlyReportFormatter` named `getMaxPageSize()`. 
- `HourlyReporter` will then call that function rather than using the `PAGE_SIZE` constant.

### G23: 用多态替代 if-else 或switch-case（Prefer Polymorphism to If/Else or Switch/Case）
反例：
CartSectionHeaderAssembler

### G24: 遵循标准约定（Follow Standard Conventions）


### G25: 用命名常量替代魔术数（Replace Magic Numbers with Named Constants）


### G26: 准确（Be Precise）


### G27: 结构优于约定（Structure over Convention）


### G28: 封装条件（Encapsulate Conditionals） 


### G29: 避免否定性条件（Avoid Negative Conditionals）


### G30: 函数只该做一件事（Functions Should Do One Thing）


### G31: 掩蔽时序耦合（Hidden Temporal Couplings）


### G32: 别随意（Don’t Be Arbitrary）


### G33: 封装边界条件（Encapsulate Boundary Conditions）


### G34: 函数应该只在一个抽象层级上（Functions Should Descend Only One Level of Abstraction）


### G35: 在较高层级放置可配置数据（Keep Configurable Data at High Levels）


### G36: 避免传递浏览（Avoid Transitive Navigation）



