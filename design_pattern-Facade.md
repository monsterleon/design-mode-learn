# Facade 门面模式
>what(是什么): 门面模式为子系统提供一组统一的接口，定义一组高层接口让子系统更易用<br>
>when(什么时候用): 
- 解决易用性问题:封装接口易于使用
- 解决性能问题：合并多个接口，减少交互次数
- 解决分布式事务问题：多个接口需要在同个事务中执行，合并多接口成为一个新的接口再用事务处理
<br>
>why(为什么用)：<br>
>how(如何用):<br>

个人理解： 更像多合一的思想，在独立解耦的思路中难免会增加交互的次数从而影响性能，但多合一的思路就能解决解耦后带来的问题，尽量保持接口的可复用性，但针对特殊情况，允许提供冗余的门面接口，来提供更易用的接口


结构
- 门面提供一个简化的接口，封装了系统的复杂性。外观模式的客户端通过与外观对象交互，而无需直接与系统的各个组件打交道。
- 子系统（Subsystem）:
由多个相互关联的类组成，负责系统的具体功能。外观对象通过调用这些子系统来完成客户端的请求。
- 客户端（Client）:
使用外观对象来与系统交互，而不需要了解系统内部的具体实现。

``` 
<!-- 门面 -->
public interface Shape {
   void draw();
}
public class Rectangle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Rectangle::draw()");
   }
}
public class Square implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Square::draw()");
   }
}
public class Circle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Circle::draw()");
   }
}
public class ShapeMaker {
   private Shape circle;
   private Shape rectangle;
   private Shape square;
 
   public ShapeMaker() {
      circle = new Circle();
      rectangle = new Rectangle();
      square = new Square();
   }
 
   public void drawCircle(){
      circle.draw();
   }
   public void drawRectangle(){
      rectangle.draw();
   }
   public void drawSquare(){
      square.draw();
   }
}
public class FacadePatternDemo {
   public static void main(String[] args) {
      ShapeMaker shapeMaker = new ShapeMaker();
 
      shapeMaker.drawCircle();
      shapeMaker.drawRectangle();
      shapeMaker.drawSquare();      
   }
}

```



