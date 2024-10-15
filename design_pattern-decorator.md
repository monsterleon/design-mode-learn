# Decorator 装饰器模式
>what(是什么): 一种用于解决继承关系过于复杂的设计模式<br>
>when(什么时候用): 当需要在不增加大量子类的情况下扩展类的功能。<br>
>why(为什么用)：<br>
>how(如何用)：<br>
- 定义组件接口：创建一个接口，规定可以动态添加职责的对象的标准。
- 创建具体组件：实现该接口的具体类，提供基本功能。
- 创建抽象装饰者：实现同样的接口，持有一个组件接口的引用，可以在任何时候动态地添加功能。
- 创建具体装饰者：扩展抽象装饰者，添加额外的职责。

``` 

<!-- 装饰器模式  -->
<!-- 
Component接口：定义了可以被装饰的对象的标准。
ConcreteComponent类：实现Component接口的具体类。
Decorator抽象类：实现Component接口，并包含一个Component接口的引用。
ConcreteDecorator类：扩展Decorator类，添加额外的功能。
-->
<!-- Component接口：定义了可以被装饰的对象的标准。 -->
public interface Shape {
   void draw();
}
<!-- ConcreteComponent类：实现Component接口的具体类。 -->
public class Rectangle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Shape: Rectangle");
   }
}
public class Circle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Shape: Circle");
   }
}
<!-- Decorator抽象类：实现Component接口，并包含一个Component接口的引用。  -->
public abstract class ShapeDecorator implements Shape {
   protected Shape decoratedShape;
 
   public ShapeDecorator(Shape decoratedShape){
      this.decoratedShape = decoratedShape;
   }
 
   public void draw(){
      decoratedShape.draw();
   }  
}
<!-- ConcreteDecorator类：扩展Decorator类，添加额外的功能。 -->
public class RedShapeDecorator extends ShapeDecorator {
 
   public RedShapeDecorator(Shape decoratedShape) {
      super(decoratedShape);     
   }
 
   @Override
   public void draw() {
      decoratedShape.draw();         
      setRedBorder(decoratedShape);
   }
 
   private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}

<!-- 使用 -->
public class DecoratorPatternDemo {
   public static void main(String[] args) {
 
      Shape circle = new Circle();
      ShapeDecorator redCircle = new RedShapeDecorator(new Circle());
      ShapeDecorator redRectangle = new RedShapeDecorator(new Rectangle());
      //Shape redCircle = new RedShapeDecorator(new Circle());
      //Shape redRectangle = new RedShapeDecorator(new Rectangle());
      System.out.println("Circle with normal border");
      circle.draw();
 
      System.out.println("\nCircle of red border");
      redCircle.draw();
 
      System.out.println("\nRectangle of red border");
      redRectangle.draw();
   }
}
```

