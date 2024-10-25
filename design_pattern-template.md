# template 模版模式
>what(是什么): 模板方法模式在一个方法中定义一个算法骨架，并将某些步骤推迟到子类中实现。模板方法模式可以让子类在不改变算法整体结构的情况下，重新定义算法中的某些步骤<br>
>when(什么时候用): 当发现多个类的整体流程相似时，不妨试试模版模式<br>
>why(为什么用)：<br>
>how(如何用): 抽象类父类作为主流程，子类定制化流程中的一些方法<br>

个人理解： 实际开发常用的的一种设计模式，抽象类作为父类一些特殊的方法交给子类去实现，实现了代码的复用和扩展



``` 
<!-- 示例 -->
public abstract class AbstractClass {
  public final void templateMethod() {
    //...
    method1();
    //...
    method2();
    //...
  }
  
  protected abstract void method1();
  protected abstract void method2();
}

public class ConcreteClass1 extends AbstractClass {
  @Override
  protected void method1() {
    //...
  }
  
  @Override
  protected void method2() {
    //...
  }
}

public class ConcreteClass2 extends AbstractClass {
  @Override
  protected void method1() {
    //...
  }
  
  @Override
  protected void method2() {
    //...
  }
}

AbstractClass demo = ConcreteClass1();
demo.templateMethod();

```



