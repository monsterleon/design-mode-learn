# Chain Of Responsibility Design Pattern 责任链模式
>what(是什么): 将请求的发送和接收解耦，让多个接收对象都有机会处理这个请求。将这些接收对象串成一条链，并沿着这条链传递这个请求，直到链上的某个接收对象能够处理它为止。<br>
>when(什么时候用): 当<br>
>why(为什么用)：<br>
>how(如何用): 用个数组去存储处理对象，然后循环调用对象去处理<br>

个人理解： 其实就是链式任务，A -> B -> C, 任务一定会按ABC三个顺序执行。到某个步骤执行完后就会停止。（但是不是也会有处理部分的职责，还有部分需要后续步骤处理？在生活中常出现step 1,2,3 1成功后才会走2， A部门处理完相应内容需要B部门处理后续任务等等）
但这个的优势就是不用特定的去找谁才是负责人，就按顺序执行，谁可以执行谁执行。




``` 
<!-- 责任链 -->
public interface IHandler {
  boolean handle();
}

public class HandlerA implements IHandler {
  @Override
  public boolean handle() {
    boolean handled = false;
    //...
    return handled;
  }
}

public class HandlerB implements IHandler {
  @Override
  public boolean handle() {
    boolean handled = false;
    //...
    return handled;
  }
}

public class HandlerChain {
  private List<IHandler> handlers = new ArrayList<>();

  public void addHandler(IHandler handler) {
    this.handlers.add(handler);
  }

  public void handle() {
    for (IHandler handler : handlers) {
      boolean handled = handler.handle();
      if (handled) {
        break;
      }
    }
  }
}

// 使用举例
public class Application {
  public static void main(String[] args) {
    HandlerChain chain = new HandlerChain();
    chain.addHandler(new HandlerA());
    chain.addHandler(new HandlerB());
    chain.handle();
  }
}

```



