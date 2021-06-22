# Java Object类方法

## 1、Object终极父类
Object类位于java.lang包中，java.lang包包含着Java最基础和核心的类，在编译时会自动导入；

Object类是所有Java类的祖先。每个类都使用Object作为超类。所有对象(包括数组)都实现这个类的方法。可以使用类型为Object的变量指向任意类型的对象

## 2、Object的主要方法
```
package java.lang;

import jdk.internal.HotSpotIntrinsicCandidate;

public class Object {

    /* 一个本地方法，具体是用C（C++）在DLL中实现的，然后通过JNI调用。*/     
    private static native void registerNatives();
    
    /* 对象初始化时自动调用此方法*/   
    static {
        registerNatives();
    }
   
    /*构造函数*/   
    @HotSpotIntrinsicCandidate
    public Object() {}
    

    /*返回此Object的运行时类。*/   
    @HotSpotIntrinsicCandidate
    public final native Class<?> getClass();
     
    /*返回此 Object 的hashCode。*/    
    @HotSpotIntrinsicCandidate
    public native int hashCode();

    /*判断两个Object是否相等*/   
    public boolean equals(Object obj) {
        return (this == obj);
    }

     /*创建并返回此对象的一个副本。*/   
    @HotSpotIntrinsicCandidate
    protected native Object clone() throws CloneNotSupportedException;

    
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    /*唤醒在此对象监视器上等待的单个线程。*/
    @HotSpotIntrinsicCandidate
    public final native void notify();

    /*唤醒在此对象监视器上等待的所有线程。*/
    @HotSpotIntrinsicCandidate
    public final native void notifyAll();

    
    /*在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。换句话说，此方法的行为就好像它仅执行 wait(0) 调用一样。   

     当前线程必须拥有此对象监视器。该线程发布对此监视器的所有权并等待，直到其他线程通过调用 notify 方法，或 notifyAll 方法通知在此对象的监视器上等待的线程醒来。然后该线程将等到重新获得对监视器的所有权后才能继续执行。*/
    public final void wait() throws InterruptedException {
        wait(0L);
    }

    
    /*在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量前，导致当前线程等待。*/  
    public final native void wait(long timeout) throws InterruptedException;

    /* 在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量前，导致当前线程等待。*/
    public final void wait(long timeout, int nanos) throws InterruptedException {
        if (timeout < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                                "nanosecond timeout value out of range");
        }

        if (nanos > 0) {
            timeout++;
        }

        wait(timeout);
    }

   /*当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。*/
   @Deprecated(since="9")
   protected void finalize() throws Throwable { }
}

```
## 3、hashCode()方法
hash值：Java中的hashCode方法就是根据一定的规则将与对象相关的信息（比如对象的存储地址，对象的字段等）映射成一个数值，这个数值称作为散列值。

重写hashCode（）方法的基本规则：

- 在程序运行过程中，同一个对象多次调用hashCode()方法应该返回相同的值。
- 当两个对象通过equals()方法比较返回true时，则两个对象的hashCode()方法返回相等的值。
- 对象用作equals()方法比较标准的Field，都应该用来计算hashCode值。

Object本地实现的hashCode()方法计算的值是底层代码的实现，采用多种计算参数，返回的并不一定是对象的(虚拟)内存地址，具体取决于运行时库和JVM的具体实现。
## 4、equals()方法
equals()方法：比较两个对象是否相等

所有的对象都拥有标识(内存地址)和状态(数据)，同时“==”比较两个对象的的内存地址，所以说使用Object的equals()方法是比较两个对象的内存地址是否相等，即若object1.equals(object2)为true，则表示equals1和equals2实际上是引用同一个对象。实际上JDK中，String、Math等封装类都对equals()方法进行了重写。

覆盖equals()函数的时候要遵守的规则

- 自反性：对于任意非空的引用值x，x.equals(x)返回值为真。
- 对称性：对于任意非空的引用值x和y，x.equals(y)必须和y.equals(x)返回相同的结果。
- 传递性：对于任意的非空引用值x,y和z,如果x.equals(y)返回真，y.equals(z)返回真，那么x.equals(z)也必须返回真。
- 一致性：对于任意非空的引用值x和y，无论调用x.equals(y)多少次，都要返回相同的结果。在比较的过程中，对象中的数据不能被修改。
 
 对于任意的非空引用值x，x.equals(null)必须返回假。

## 5、clone()方法
clone()方法：快速创建一个已有对象的副本

第一：Object类的clone()方法是一个native方法，native方法的效率一般来说都是远高于Java中的非native方法。这也解释了为什么要用Object中clone()方法而不是先new一个类，然后把原始对象中的信息复制到新对象中，虽然这也实现了clone功能。

第二：Object类中的clone()方法被protected修饰符修饰。这也意味着如果要应用 clone()方 法，必须继承Object类。

第三：Object.clone()方法返回一个Object对象。我们必须进行强制类型转换才能得到我们需要的类型。


克隆的步骤：

1：创建一个对象；   
2：将原有对象的数据导入到新创建的数据中。

clone方法首先会判对象是否实现了Cloneable接口，若无则抛出CloneNotSupportedException, 最后会调用internalClone. intervalClone是一个native方法，一般来说native方法的执行效率高于非native方法。

### 复制对象 or 复制引用
```
Person p = new Person(23, "zhang"); 
Person p1 = p; 
  
System.out.println(p); 
System.out.println(p1); 

打印出来：
com.pansoft.zhangjg.testclone.Person@2f9ee1ac
com.pansoft.zhangjg.testclone.Person@2f9ee1ac

打印的地址值是相同的，既然地址都是相同的，那么肯定是同一个对象。
p和p1只是引用而已，他们都指向了一个相同的对象Person(23, "zhang") 。 
可以把这种现象叫做引用的复制。

Person p = new Person(23, "zhang"); 
Person p1 = (Person) p.clone(); 

System.out.println(p); 
System.out.println(p1); 

打印出：
com.pansoft.zhangjg.testclone.Person@2f9ee1ac
com.pansoft.zhangjg.testclone.Person@67f1fba0

两个对象的地址是不同的，也就是说创建了新的对象， 而不是把原对象的地址赋给了一个新的引用变量

Person p = new Person(23, "zhang"); 
Person p1 = (Person) p.clone(); 

String result = p.getName() == p1.getName()?"clone是浅拷贝的":"clone是深拷贝的"; 

System.out.println(result);
打印出：
    clone是浅拷贝的
```

## 6、notify方法和wait方法
wait()，notify() 和 notifyAll() 可以让线程协调完成一项任务。

### 不同的 wait() 方法之间有什么区别

没有参数的 wait() 方法被调用之后，线程就会一直处于睡眠状态，直到本对象（就是 wait() 被调用的那个对象）调用 notify() 或 notifyAll() 方法。相应的wait(long timeout)和wait(long timeout, int nanos)方法中，当等待时间结束或者被唤醒时（无论哪一个先发生）将会结束等待。

#### notify() 和 notifyAll() 方法有什么区别
notify() 方法随机唤醒一个等待的线程，而 notifyAll() 方法将唤醒所有在等待的线程。

#### 线程被唤醒之后会发生什么
当一个线程被唤醒之后，除非本对象（调用 notify() 或 notifyAll() 的对象）的同步锁被释放，否则不会立即执行。唤醒的线程会按照规则和其他线程竞争同步锁，得到锁的线程将执行。所以notifyAll()方法执行之后，可能会有一个线程立即运行，也可能所有的线程都没运行。

## 7、toString()方法
toString方法会返回一个“以文本方式表示”此对象的字符串。结果应是一个简明但易于读懂的信息表达式。建议所有子类都重写此方法。

Object类的toString方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at标记符“@”和此对象哈希码的无符号十六进制表示组成。

## 8、getClass()方法
通过gerClass()方法可以得到一个和这个类有关的java.lang.Class对象（字节码对象）。返回的Class对象是一个被static synchronized方法封装的代表这个类的对象。这也是指向反射API,因为调用getClass()的对象的类是在内存中的，保证了类型安全。

还有其他方法得到Class对象吗？
获取Class对象的方法有两种。

- 可以使用类字面常量，它的名字和类型相同，后缀位.class；例如，Object.class。
- 另外一种就是调用Class的forName()方法。类字面常量更加简洁，并且编译器强制类型安全；如果找不到指定的类编译就不会通过。
通过forName()可以动态地通过指定包名载入任意类型地引用。但是，不能保证类型安全，可能会导致Runtime异常。

## 9、finalize()方法
finalize()方法：垃圾回收器准备释放内存的时候，会先调用finalize()。

(1).对象不一定会被回收。

(2).垃圾回收不是析构函数。

(3).垃圾回收只与内存有关。

(4).垃圾回收和finalize()都是靠不住的，只要JVM还没有快到耗尽内存的地步，它是不会浪费时间进行垃圾回收的。
