## 三个方法

* interrupt()

  用于线程中断，该方法并不能直接中断线程，只会将线程的中断标志位改为true。它只会给线程发送一个中断状态，线程是否中断取决于线程内部对该中断信号做什么响应，若不处理该中断信号，线程就不会中断。

  **简而言之，就是通知线程你需要执行中断了，具体对该中断的影响需要依赖当前线程对中断的处理。**（本人理解）

* interrupted() 与 isInterrupted() 傻傻分不清？

  直接上源码。

  ```java
  public static boolean interrupted() {
      return currentThread().isInterrupted(true);
  }
  ```

  ```java
  public boolean isInterrupted() {
      return isInterrupted(false);
  }
  ```

  interrupted() 和 isInterrupted() 方法都调用了isInterrupted(false)的有参构造方法。来，看下这个有参构造方法：

  ```java
  private native boolean isInterrupted(boolean ClearInterrupted);
  ```

  看下这个方法的源码注释。

  > Tests if some Thread has been interrupted. The interrupted state is reset or not based on the value of ClearInterrupted that is passed.

  这个方法返回线程是否被中断了，中断状态是否被清楚依赖于ClearInterrupted变量的值。

  so，interrupted() 与 isInterrupted() 的区别：

  * interrupted() ：判断线程是否处于中断状态，并重置中断状态。
  * isInterrupted()：判断线程是否处于中断状态，不会重置中断状态。

