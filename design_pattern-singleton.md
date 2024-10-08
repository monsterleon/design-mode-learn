# Singleton 单例模式
what(是什么): 一个类只允许创建一个对象<br>
when(什么时候用)：常用场景：打印日志Logger,ID生成器<br>
why(为什么用)：节省内存空间，避免覆盖问题<br>
how(如何用)：懒汉，饿汉，双重检测，静态内部类，枚举<br>


``` 
<!-- 饿汉 在类加载的时候，instance 静态实例就已经创建并初始化好了 -->
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static final IdGenerator instance = new IdGenerator();
  private IdGenerator() {}
  public static IdGenerator getInstance() {
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}

<!-- 懒汉式  -->
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static IdGenerator instance;
  private IdGenerator() {}
  public static synchronized IdGenerator getInstance() {
    if (instance == null) {
      instance = new IdGenerator();
    }
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}

<!-- 双重检测 -->
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private static IdGenerator instance;
  private IdGenerator() {}
  public static IdGenerator getInstance() {
    if (instance == null) {
      synchronized(IdGenerator.class) { // 此处为类级别的锁
        if (instance == null) {
          instance = new IdGenerator();
        }
      }
    }
    return instance;
  }
  public long getId() { 
    return id.incrementAndGet();
  }
}
<!-- CPU 指令重排序可能导致在 IdGenerator 类的对象被关键字 new 创建并赋值给 instance 之后，还没来得及初始化（执行构造函数中的代码逻辑），就被另一个线程使用了。这样，另一个线程就使用了一个没有完整初始化的 IdGenerator 类的对象。要解决这个问题，我们只需要给 instance 成员变量添加 volatile 关键字来禁止指令重排序即可。 -->


<!-- 静态内部类 -->
public class IdGenerator { 
  private AtomicLong id = new AtomicLong(0);
  private IdGenerator() {}

  private static class SingletonHolder{
    private static final IdGenerator instance = new IdGenerator();
  }
  
  public static IdGenerator getInstance() {
    return SingletonHolder.instance;
  }
 
  public long getId() { 
    return id.incrementAndGet();
  }
}

<!-- 枚举 -->
public enum IdGenerator {
  INSTANCE;
  private AtomicLong id = new AtomicLong(0);
 
  public long getId() { 
    return id.incrementAndGet();
  }
}

```
单例缺点：
- 不支持有参数的构造函数
- 测试不友好

参数化单例
```
public class Config {
  public static final int PARAM_A = 123;
  public static final int PARAM_B = 245;
}

public class Singleton {
  private static Singleton instance = null;
  private final int paramA;
  private final int paramB;

  private Singleton() {
    this.paramA = Config.PARAM_A;
    this.paramB = Config.PARAM_B;
  }

  public synchronized static Singleton getInstance() {
    if (instance == null) {
      instance = new Singleton();
    }
    return instance;
  }
}
```



