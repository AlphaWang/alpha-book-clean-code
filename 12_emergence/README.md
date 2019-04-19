# 12 Emergence

## 12.1 通过跌进设计达到整洁的目的

假如有4条简单的规矩，跟着做就能帮助你创造优良的设计，会如何？假使遵循这些规矩，你就能 **洞见** 代码的结构和设计，更轻易地应用SRP（Single Responsibility Principle 单一职责原则）和DIP（Dependence Inversion Principle 依赖倒置）之类原则，又会如何？假使这四条规则有利于良好的设计“浮现”出来，又会如何？

我们中的很多人认为，Kent Back关于简单设计的4条规则，对于创建良好的设计软件有着莫大的帮助。

据Kent所述，只要遵循以下规则，设计就能变得“简单”：

> ​    **运行所有测试；**
>
> ​    **不可重复；**
>
> ​    **表达了程序员的意图；**
>
> ​    **尽可能减少类和方法的数量；**

以上规则按其重要程度排序。



## 12.2 简单设计规则1：运行所有测试

设计必须制造出如预期一般工作的系统，这是首要因素。系统也许有一套绝佳设计，但如果缺乏验证系统是否真按预期那样工作的简单方法，那就无异于纸上谈兵。

全面测试并持续通过所有测试的系统，就是可以测试的系统。看似浅显，但却重要。不可测试的系统，同样不可验证。不可验证的系统绝不可以部署。

幸运的是，只要系统可测试，就会导向保持类短小且目的单一的设计方案。遵循SRP的类，测试起来较为简单。测试编写的越多，就能持续走向编写较易测试的代码。所以，确保系统完全可测试能帮助我们创建更好的设计。

紧耦合的代码难以编写测试。同样，编写测试越多，就越会遵循DIP原则，使用依赖注入，接口和抽象等工具尽可能减少耦合。如此一来，设计就有长足进步。

遵循有关编写测试并持续运行测试的简单、明确的规则，系统就会更贴近OO低耦合度、高内聚的目标。编写测试引致更好的设计。



## 12.3 简单设计规则2~4：重构

有了测试，就能保持代码和类的整洁，方法就是递增式地重构代码。添加了几行代码后，就要暂停，琢磨一下变化的设计。设计退步了吗？如果是，就要清理它，并且运行测试，保证没有破坏任何东西。测试消除了对清理代码就会破坏代码的恐惧。

在重构过程中，可以应用有关优秀软件设计的一切知识：

> 提升内聚性
>
> 降低耦合度
>
> 切分关注面
>
> 模块化系统性关注面
>
> 缩小函数和类的尺寸
>
> 选用更好的名称
>
> ......

这也是应用简单设计后三条规则的地方：消除重复，保证表达力，尽可能减少类和方法的数量。



## 12.4 不可重复

重复是拥有良好设计系统的大敌。它代表着额外的工作、额外的风险和额外切不必要的复杂度。重复又多种表现。及其雷同的代码行当然是重复。类似的代码往往可以调整的更相似。这样就能更容易的进行重构。重复也有实现上的重复等有其他一些形态。例如，在某个群集类中可能会有两个方法：

​    int size() {}

​    boolean isEmpty() {}

这两个方法可以分别实现。isEmpty 方法跟踪一个布尔值，而size方法跟踪一个计数器。或者，也可以通过在empty中使用size方法来消除重复：

​    boolean isEmpty() {

​        return 0 == size();    

​    }

要想创建整洁的系统，需要消除重复的意愿，即便对于短短几行也是如此。例如以下代码：

```java
public void scanToOneDimension(float desiredDimension, float imageDimension) {
        if (Math.abs(desiredDimension - imageDimension) < errorThreshould)
            return;
        float scalingFactor = desiredDimension / imageDimension;
        scalingFactor = (float) (Math.floor(scalingFactor *100) *0.01f);
        RenderedOp newImage = ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor);
        image.disose();
        System.gc();
        image = newImage;
    }
    public synchronized void rotate(int degrees) {
        RenderedOp newImage = ImageUtilities.getRotatedImage(image, degrees);
        image.dispose();
        System.gc();
        image = newImage;
}
```



要保持系统的整洁，应该消除scanToOneDimension 和 rotate 方法里面的少量重复：

```java
public void scanToOneDimension(float desiredDimension, float imageDimension) {
        if (Math.abs(desiredDimension - imageDimension) < errorThreshould)
            return;

        float scalingFactor = desiredDimension / imageDimension;
        scalingFactor = (float) (Math.floor(scalingFactor *100) *0.01f);
        replaceImage(ImageUtilities.getScaledImage(image, scalingFactor, scalingFactor));
    }

    public synchronized void rotate(int degrees) {
        replaceImage(ImageUtilities.getRotatedImage(image, degrees));
    }

    private void replaceImage(RenderedOp newImage) {
        image.dispose();
        System.gc();
        image = newImage;
    }

```

​    

做了一点点共性抽取后，我们意识到已经违反了SRP原则。所以，可以把一个新方法分解到另外的类中，从而提升其可见性。团队中的其他成员也许会发现进一步抽象新方法的机会，并且在其他场景中复用之。“小规模服用”可大量降低系统复杂性。要想实现大规模复用，必须理解如何实现小规模复用。

模板方法模式是一种移除高层级重复的通用技巧。例如：

```java

 public class VacationPolicy {
        public void accrueUSDivisionVacation() {
                //code to calculate vacation based on hours worked to date
                //...
                //code to ensure vacation means US minimums
                //...
                //code to apply vacation to payroll record
                //...
        }

        public void accrueEUDivisionVacation() {
                //code to calculate vacation based on hours worked to date
                //...
                //code to ensure vacation means EU minimums
                //...
                //code to apply vacation to payroll record
                //...
        }
}

```



除了计算法定最少数量假期的部分，accrue**US**DivisionVacation 和 accrue**EU**DivisionVacation 两个方法中有大量代码雷同。那部分的算法依据员工类型而变。

可以通过应用模板方法模式来消除明显的重复。

```java
abstract public class VacationPolicy {
        public void accrueUSDivisionVacation() {
                calculateBaseVacationHours();
                alterForLegalMinimums();
                applyToPayroll();
        }
        private void calculateBaseVacationHours() {/* ... */}
        abstract protected void alterForLegalMinimums();
        private void applyToPayroll() {/* ... */}
}
public class USVacationPolicyextends VacationPolicy {
    @Override
    protected  void alterForLegalMinimums() {
            //US specific logic
    }
}
public class EUVacationPolicyextends VacationPolicy {
    @Override
    protected  void alterForLegalMinimums() {
            //EU specific logic
    }
}

```

子类填充了accrueVacation算法中的“空洞”，提供了不重复的代码。



## 12.5 表达力

我们中的大多数人都经历过费解代码的纠缠。我们中的许多人自己就编写过费解的代码。写出自己能理解的代码很容易，因为在写这些代码时，我们正深入于要解决的问题中。代码的其他维护者不会那么深入，也就不易理解代码。

软件项目的主要成本在于长期维护。为了在修改时尽量降低出现缺陷的可能性，很有必要理解系统是做什么的。当系统变得越来越复杂，开发者就需要越来越多的时间去理解它，而且极有可能误解。所以，代码应当清晰的表达其作者的意图。作者把代码写的越清晰，其他人花在理解代码的时间也就越少，从而减少缺陷，缩减维护成本。

可以通过选用好名称来表达。我们想要听到好类名和好函数名，而且查看其权责时不会大吃一惊。

也可以通过保持函数和类尺寸短小来表达。短小的类和函数通常易于命名，易于编写，也易于理解。

还可以通过采用标准命名来表达。例如设计模式很大程度上就关乎沟通和表达。通过在实现这些模式的类的名称中采用标准模式名，例如COMMAND 或 VISITOR，就能充分地向其他开发者描述你的设计。

编写良好的单元测试，也具有表达性。测试的主要目的之一就是通过实例起到文档的作用。读到测试的人，应该能很快理解某个类是做什么的。

不过，做到有表达力的最重要方式却是尝试。有太多的时候，我们写出能工作的代码，就转移到下一个问题上，没有下足功夫调整代码，让后来者易于阅读。记住，下一位读代码的人最有可能是自己。

所以，多少尊重一下你的手艺吧。花一点点时间在每个函数和类上。选用较好的名称，将大函数切分成小函数，试试照拂自己创建的东西。**用心**是最珍贵的资源。



## 12.6 尽可能少的类和方法

即便是消除重复、代码表达力和SRP等最基础的概念也会被过度使用。为了保持类和函数短小，我们可能造出太多的细小类和方法。所以这条规则也主张函数和类的数量要少。

类和方法的数量太多，有事是毫无意义的教条主义导致的。例如某个编码标准就建成应当为每个类创建接口。也有开发者认为，字段和行为必须切分到数据类和行为类中。应该抵制这类教条，采用更实用的手段。

我们的目标是保持函数和类短小的同时，保持整个系统短小精悍。不过要记住，这在关于简单设计的四条规则里面是优先级最低的一条。所以，尽管使类和函数的数量尽量少是很重要的，但更重要的却是测试、消除重复和表达力。



## 12.7 小结

有没有能替代经验的一套简单手段呢？当然不会有。另一方面，本章中写道的时间来自于本书作者数十年经验的精炼总结。遵循简单设计的实践手段，开发者不必经年学习就能掌握好的原则和模式。



## 延伸：洞见式思维

当我们想办法解决问题，并感觉遇到困境的过程中，我们没有办法知道，我们遇到的困境是暂时的还是永久的。这是因为陷入困境很可能是遭遇下面三种情况之一：

>  我们没有解决问题所需要的信息、资源和时间；
>
> 我们达到了人脑能力的生物极限；
>
> 解决办法根本不存在；

有时候我们只需要几分钟时间，有时候我们需要几年时间，有时候甚至几千年。但不管有多长时间，困境就是困境。事实证明，当人类机体遭遇认知门槛时，不管是几秒钟还是几个世纪都一样。

幸运的是，僵局不仅可以被打破，而且常常被打破。

所需要的就是洞见。

例如：阿基米德称皇冠、牛顿发现万有引力

加速增长的复杂性超过了人脑发展出应对世界的新技能的速度，这种现象自古已然，于今尤甚。玛雅文明、高棉文明、罗马帝国的盛衰，深刻反映出人类在面对危机是治标不治本的短视和欲速则不达的狭隘。问题并没有得到最终解决，在长期的历史发展过程中，反而导致了更恶劣的后果。

以此类比我们的系统。公司的任何一个服务都是从第一行代码开始的，从单体到微服务，复杂度越来越高，直到没有人能清楚有哪些服务、功能分别是什么...总有一天我们需要面对一些超越自身能力的难题，甚至是困境。有些困境是个人的，有些是团队的，不管是个人还是团队，如何面对这些问题，走出困境，都需要洞见式思维。