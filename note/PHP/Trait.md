## Trait

> php从以前到现在一直都是单继承的语言，无法同时从两个基类中继承属性和方法，为了解决这个问题，php出了Trait这个特性

### 用法：通过在类中使用use 关键字，声明要组合的Trait名称，具体的Trait的声明使用Trait关键词，Trait不能实例化

### 优先级：Trait中的方法会覆盖基类中的同名方法，而本类会覆盖Trait中同名方法

注意点：当trait定义了属性后，类就不能定义同样名称的属性，否则会产生 fatal error，除非是设置成相同可见度、相同默认值。不过在php7之前，即使这样设置，还是会产生E_STRICT 的提醒

### 一个类可以组合多个Trait，通过逗号相隔，如下
use trait1,trait2


作者：奋斗live
链接：https://www.jianshu.com/p/fc053b2d7fd1
