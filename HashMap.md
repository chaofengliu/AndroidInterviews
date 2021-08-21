# HashMap连环炮
在面试中，HashMap基本必问，只是问法各有不同而已。因为使用HashMap就能考察面试者的很多知识点，所以需要大家熟练掌握。

## 1、说说HashMap底层数据结构是怎样的？
HashMap底层是hash数组和单向链表实现，JDK8后采用数组+链表+红黑树的数据结构。

## 2、说说HashMap的工作原理
如果第一题没问，直接问原理，那就必须把HashMap的数据结构说清楚。

HashMap底层是hash数组和单向链表实现，JDK8后采用数组+链表+红黑树的数据结构。

我们通过put和get存储和获取对象。当我们给put()方法传递键和值时，先对键做一个hashCode()的计算来得到它在bucket数组中的位置来存储Entry对象。当获取对象时，通过get获取到bucket的位置，再通过键对象的equals()方法找到正确的键值对，然后在返回值对象。

## 3、使用HashMap时，当两个对象的hashCode相同怎么办？
因为HashCode 相同，不一定就是相等的（equals方法比较），所以两个对象所在数组的下标相同，"碰撞"就此发生。又因为HashMap使用链表存储对象，这个Node会存储到链表中。

## 4、HashMap的哈希函数怎么设计的吗？

```
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
hash 函数是先拿到通过key的hashCode ，是32位的int值，然后让hashCode的高16位和低 16位进行异或操作。

两个好处：

- 一定要尽可能降低hash碰撞，越分散越好；

- 算法一定要尽可能高效，因为这是高频操作, 因此采用位运算；

## 5、HashMap遍历方法有几种？

HashMap遍历从大的方向来说，可分为以下4类：

- 迭代器（Iterator）方式遍历；
- For Each 方式遍历；
- Lambda 表达式遍历（JDK 1.8+）;
- Streams API 遍历（JDK 1.8+）。

但每种类型下又有不同的实现方式，因此具体的遍历方式又可以分为以下7种：

- 使用迭代器（Iterator）EntrySet 的方式进行遍历；
- 使用迭代器（Iterator）KeySet 的方式进行遍历；
- 使用 For Each EntrySet 的方式进行遍历；
- 使用 For Each KeySet 的方式进行遍历；
- 使用 Lambda 表达式的方式进行遍历；
- 使用 Streams API 单线程的方式进行遍历；
- 使用 Streams API 多线程的方式进行遍历。


1.迭代器 EntrySet

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
        while (iterator.hasNext()) {
            Map.Entry<Integer, String> entry = iterator.next();
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        }
    }
}
```
 

2.迭代器 KeySet

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        Iterator<Integer> iterator = map.keySet().iterator();
        while (iterator.hasNext()) {
            Integer key = iterator.next();
            System.out.print(key);
            System.out.print(map.get(key));
        }
    }
}
```

3.ForEach EntrySet

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        }
    }
}
```

4.ForEach KeySet

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        for (Integer key : map.keySet()) {
            System.out.print(key);
            System.out.print(map.get(key));
        }
    }
}
```

5.Lambda

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文社群");
        // 遍历
        map.forEach((key, value) -> {
            System.out.print(key);
            System.out.print(value);
        });
    }
}
```

6.Streams API 单线程

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        map.entrySet().stream().forEach((entry) -> {
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        });
    }
}
```

7.Streams API 多线程

```
public class HashMapTest {
    public static void main(String[] args) {
        // 创建并赋值 HashMap
        Map<Integer, String> map = new HashMap();
        map.put(1, "Java");
        map.put(2, "JDK");
        map.put(3, "Spring Framework");
        map.put(4, "MyBatis framework");
        map.put(5, "Java中文");
        // 遍历
        map.entrySet().parallelStream().forEach((entry) -> {
            System.out.print(entry.getKey());
            System.out.print(entry.getValue());
        });
    }
}
```

 
## 6、为什么采用hashcode的高16位和低16位异或能降低hash碰撞？
因为 key.hashCode()函数调用的是key键值类型自带的哈希函数，返回int型散列值。int值范围为**-2147483648~2147483647**，前后加起来大概 40 亿的映射空间。只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个 40 亿长度的数组，内存是放不下的。

HashMap 数组的初始大小才16，用之前需要对数组的长度取模运算，得到的余数才能用来访问数组下标。

当数组的长度很短时，只有低位数的hashcode值能参与运算。而让高16位参与运算可以更好的均匀散列，减少碰撞，进一步降低hash冲突的几率。并且使得高16位和低16位的信息都被保留了。

而在这里采用异或运算而不采用&或|运算的原因是异或运算能更好的保留各部分的特征，如果采用&运算计算出来的值的二进制会向1靠拢，采用|运算计算出来的值的二进制会向0靠拢

因为int是4个字节，所以右移16位。查看hashmap的源码，找到hash方法，按住ctrl点击方法里的hashcode，跳转到Object类，发现hashcode的数据类型是int。int为4个字节，1个字节8个比特位，就是32个比特位，所以16很可能是因为32对半的结果，也就是让高的那一半也来参与运算所以选择了16。

## 7、解决hash冲突的有几种方法？

1、再哈希法：如果hash出的index已经有值，就再hash，不行继续hash，直至找到空的index位置，要相信瞎猫总能碰上死耗子。这个办法最容易想到。但有2个缺点：

比较浪费空间，消耗效率。根本原因还是数组的长度是固定不变的，不断hash找出空的index，可能越界，这时就要创建新数组，而老数组的数据也需要迁移。随着数组越来越大，消耗不可小觑。

get不到，或者说get算法复杂。进是进去了，想出来就没那么容易了。

2、开放地址方法：如果hash出的index已经有值，通过算法在它前面或后面的若干位置寻找空位，这个和再hash算法差别不大。

3、建立公共溢出区： 把冲突的hash值放到另外一块溢出区。

4、链式地址法： 把产生hash冲突的hash值以链表形式存储在index位置上。HashMap用的就是该方法。优点是不需要另外开辟新空间，也不会丢失数据，寻址也比较简单。但是随着hash链越来越长，寻址也是更加耗时。好的hash算法就是要让链尽量短，最好一个index上只有一个值。也就是尽可能地保证散列地址分布均匀，同时要计算简单。

## 8、为什么要用异或运算符？

首先将高16位无符号右移16位与低16位做异或运算。如果不这样做，而是直接做&运算那么高16位所代表的部分特征就可能被丢失，将高16位无符号右移之后与低16位做异或运算使得高16位的特征与低16位的特征进行了混合得到的新的数值中就高位与低位的信息都被保留了。

而在这里采用异或运算而不采用&或|运算的原因是异或运算能更好的保留各部分的特征，保证了对象的hashCode的32位值只要有一位发生改变，整个hash()返回值就会改变，同时尽可能的减少碰撞。如果采用&运算计算出来的值会向1靠拢，采用|运算计算出来的值会向0靠拢


## 9、HashMap的table的容量如何确定？
①、table数组大小是由capacity这个参数确定的，默认是16，也可以构造时传入，最大限制是1<<30；

②、loadFactor是装载因子，主要目的是用来确认table数组是否需要动态扩展，默认值是0.75，比如table数组大小为16，装载因子为0.75时，threshold就是12，当table的实际大小超过 12 时，table就需要动态扩容；

③、扩容时，调用resize()方法，将table长度变为原来的两倍（注意是table长度，而不是 threshold）；

④、如果数据很大的情况下，扩展时将会带来性能的损失，在性能要求很高的地方，这种损失很可能很致命。

## 10、请解释一下HashMap的参数loadFactor，它的作用是什么

loadFactor表示HashMap的拥挤程度，影响hash操作到同一个数组位置的概率。

默认loadFactor等于0.75，当HashMap里面容纳的元素已经达到HashMap数组长度的75%时，表示HashMap太挤了，需要扩容，在HashMap的构造器中可以定制loadFactor。

至于为什么是0.75，节点出现的频率在hash桶中遵循泊松分布，当桶中元素到达8个的时候，概率已经变得非常小，也就是说用0.75作为加载因子，每个碰撞位置的链表长度超过８个是几乎不可能的。

## 11、说说HashMap中put方法的过程

由于JDK版本中HashMap设计上存在差异，这里说说JDK7和JDK8中的区别：

[对比](https://github.com/chaofengliu/AndroidInterviews/blob/main/img/70710b47dff9b7c924c718c3a4b59705ce4f293f.png%40942w_progressive.png)

具体put流程，请参照下图进行回答：

[流程](https://github.com/chaofengliu/AndroidInterviews/blob/main/img/b3927896b2cb11b327b3c18f8aff334ee7ac48f4.png%40942w_progressive.png)


## 12、当链表长度 >= 8时，为什么要将链表转换成红黑树？

因为红黑树的平均查找长度是log(n)，长度为8的时候，平均查找长度为3，如果继续使用链表，平均查找长度为8/2=4，所以，当链表长度>=8时 ，有必要将链表转换成红黑树。

## 13、new HashMap(18);此时HashMap初始容量为多少？
容量为32。

在HashMap中有个静态方法tableSizeFor ，tableSizeFor方法保证函数返回值是大于等于给定参数initialCapacity最小的2的幂次方的数值 。

## 14、说说resize扩容的过程
创建一个新的数组，其容量为旧数组的两倍，并重新计算旧数组中结点的存储位置。结点在新数组中的位置只有两种，原下标位置或原下标+旧数组的大小。


## 15、说说hashMap中get是如何实现的？
对key的hashCode进行hash值计算，与运算计算下标获取bucket位置，如果在桶的首位上就可以找到就直接返回，否则在树中找或者链表中遍历找，如果有hash冲突，则利用equals方法去遍历链表查找节点。

## 16、拉链法导致的链表过深问题为什么不用二叉查找树代替，而选择红黑树？为什么不一直使用红黑树？
之所以选择红黑树是为了解决二叉查找树的缺陷，二叉查找树在特殊情况下会变成一条线性结构（这就跟原来使用链表结构一样了，造成很深的问题），遍历查找会非常慢。而红黑树在插入新数据后可能需要通过左旋，右旋、变色这些操作来保持平衡，引入红黑树就是为了查找数据快，解决链表查询深度的问题，我们知道红黑树属于平衡二叉树，但是为了保持“平衡”是需要付出代价的，但是该代价所损耗的资源要比遍历线性链表要少，所以当长度大于8的时候，会使用红黑树，如果链表长度很短的话，根本不需要引入红黑树，引入反而会慢。

## 17、说说你对红黑树的了解
红黑树是一种自平衡的二叉查找树，是一种高效的查找树。

红黑树通过如下的性质定义实现自平衡：

节点是红色或黑色。

根是黑色。

所有叶子都是黑色（叶子是NIL节点）。

每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）

从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点（简称黑高）。

## 18、JDK8中对HashMap做了哪些改变？
1.在java 1.8中，如果链表的长度超过了8，那么链表将转换为红黑树。（桶的数量必须大于64，小于64的时候只会扩容）

2.发生hash碰撞时，java 1.7 会在链表的头部插入，而java 1.8会在链表的尾部插入

3.在java 1.8中，Entry被Node替代(换了一个马甲)。

## 19、HashMap 中的 key 我们可以使用任何类作为 key 吗？
平时可能大家使用的最多的就是使用 String 作为 HashMap 的 key，但是现在我们想使用某个自定 义类作为 HashMap 的 key，那就需要注意以下几点：

如果类重写了 equals 方法，它也应该重写 hashCode 方法。

类的所有实例需要遵循与 equals 和 hashCode 相关的规则。

如果一个类没有使用 equals，你不应该在 hashCode 中使用它。

咱们自定义 key 类的最佳实践是使之为不可变的，这样，hashCode 值可以被缓存起来，拥有

更好的性能。不可变的类也可以确保 hashCode 和 equals 在未来不会改变，这样就会解决与可变相关的问题了。

## 20、HashMap 的长度为什么是 2 的 N 次方呢？
为了能让 HashMap 存数据和取数据的效率高，尽可能地减少 hash 值的碰撞，也就是说尽量把数 据能均匀的分配，每个链表或者红黑树长度尽量相等。我们首先可能会想到 % 取模的操作来实现。下面是回答的重点哟：

取余（%）操作中如果除数是 2 的幂次，则等价于与其除数减一的与（&）操作（也就是说 hash % length == hash &(length - 1) 的前提是 length 是 2 的 n 次方）。并且，采用二进 制位操作 & ，相对于 % 能够提高运算效率。

这就是为什么 HashMap 的长度需要 2 的 N 次方了。

## 21、HashMap，LinkedHashMap，TreeMap 有什么区别？
LinkedHashMap是继承于HashMap，是基于HashMap和双向链表来实现的。

HashMap无序；LinkedHashMap有序，可分为插入顺序和访问顺序两种。如果是访问顺序，那put和get操作已存在的Entry时，都会把Entry移动到双向链表的表尾(其实是先删除再插入)。

LinkedHashMap存取数据，还是跟HashMap一样使用的Entry[]的方式，双向链表只是为了保证顺序。

LinkedHashMap是线程不安全的。

## 22、说说什么是 fail-fast？
fail-fast 机制是 Java 集合（Collection）中的一种错误机制。当多个线程对同一个集合的内容进行 操作时，就可能会产生 fail-fast 事件。

例如：当某一个线程 A 通过 iterator 去遍历某集合的过程中，若该集合的内容被其他线程所改变 了，那么线程 A 访问集合时，就会抛出 ConcurrentModificationException 异常，产生 fail-fast 事 件。这里的操作主要是指 add、remove 和 clear，对集合元素个数进行修改。

解决办法

建议使用“java.util.concurrent 包下的类”去取代“java.util 包下的类”。可以这么理解：在遍历之前，把 modCount 记下来 expectModCount，后面 expectModCount 去 和 modCount 进行比较，如果不相等了，证明已并发了，被修改了，于是抛出 ConcurrentModificationException 异常。

## 23、HashMap 和 HashTable 有什么区别？
①、HashMap 是线程不安全的，HashTable 是线程安全的；

②、由于线程安全，所以 HashTable 的效率比不上 HashMap；

③、HashMap最多只允许一条记录的键为null，允许多条记录的值为null，而 HashTable不允许；

④、HashMap 默认初始化数组的大小为16，HashTable 为 11，前者扩容时，扩大两倍，后者扩大两倍+1；

⑤、HashMap 需要重新计算 hash 值，而 HashTable 直接使用对象的 hashCode；

24、HashMap 是线程安全的吗？
不是，在多线程环境下，1.7 会产生死循环、数据丢失、数据覆盖的问题，1.8 中会有数据覆盖的问题，以 1.8 为例，当 A 线程判断 index 位置为空后正好挂起，B 线程开始往 index 位置的写入节点数据，这时 A 线程恢复现场，执行赋值操作，就把 A 线程的数据给覆盖了；还有++size 这个地方也会造成多线程同时扩容等问题。

25、如何规避 HashMap 的线程不安全？
单线程条件下，为避免出现ConcurrentModificationException，需要保证只通过HashMap本身或者只通过Iterator去修改数据，不能在Iterator使用结束之前使用HashMap本身的方法修改数据。因为通过Iterator删除数据时，HashMap的modCount和Iterator的expectedModCount都会自增，不影响二者的相等性。如果是增加数据，只能通过HashMap本身的方法完成，此时如果要继续遍历数据，需要重新调用iterator()方法从而重新构造出一个新的Iterator，使得新Iterator的expectedModCount与更新后的HashMap的modCount相等。

多线程条件下，可使用两种方式：

Collections.synchronizedMap方法构造出一个同步Map

直接使用线程安全的ConcurrentHashMap。

26、HashMap 和 ConcurrentHashMap 的区别？
都是 key-value 形式的存储数据；

HashMap 是线程不安全的，ConcurrentHashMap 是 JUC 下的线程安全的；

HashMap 底层数据结构是数组 + 链表（JDK 1.8 之前）。JDK 1.8 之后是数组 + 链表 + 红黑 树。当链表中元素个数达到 8 的时候，链表的查询速度不如红黑树快，链表会转为红黑树，红 黑树查询速度快；

HashMap 初始数组大小为 16（默认），当出现扩容的时候，以 0.75 * 数组大小的方式进行扩 容；

ConcurrentHashMap 在 JDK 1.8 之前是采用分段锁来现实的 Segment + HashEntry， Segment 数组大小默认是 16，2 的 n 次方；JDK 1.8 之后，采用 Node + CAS + Synchronized 来保证并发安全进行实现。

27、为什么 ConcurrentHashMap 比 HashTable 效率要高？
HashTable：使用一把锁（锁住整个链表结构）处理并发问题，多个线程竞争一把锁，容易阻塞；

ConcurrentHashMap：

JDK 1.7 中使用分段锁（ReentrantLock + Segment + HashEntry），相当于把一个 HashMap 分成多个段，每段分配一把锁，这样支持多线程访问。锁粒度：基于 Segment，包含多个 HashEntry。

JDK 1.8 中使用CAS + synchronized + Node + 红黑树。锁粒度：Node（首结点）（实现 Map.Entry<K,V>）。锁粒度降低了。

28、说说 ConcurrentHashMap中 锁机制
JDK 1.7 中，采用分段锁的机制，实现并发的更新操作，底层采用数组+链表的存储结构，包括两个核心静态内部类 Segment 和 HashEntry。

①、Segment 继承 ReentrantLock（重入锁） 用来充当锁的角色，每个 Segment 对象守护每个散列映射表的若干个桶；

②、HashEntry 用来封装映射表的键-值对；

③、每个桶是由若干个 HashEntry 对象链接起来的链表


JDK 1.8 中，采用Node + CAS + Synchronized来保证并发安全。取消类 Segment，直接用 table 数组存储键值对；当 HashEntry 对象组成的链表长度超过 TREEIFY_THRESHOLD 时，链表转换为红黑树，提升性能。底层变更为数组 + 链表 + 红黑树。


29、在 JDK 1.8 中，ConcurrentHashMap 为什么要使用内置锁 synchronized 来代替重入锁 ReentrantLock？
①、粒度降低了；

②、JVM 开发团队没有放弃 synchronized，而且基于 JVM 的 synchronized 优化空间更大，更加自然。

③、在大量的数据操作下，对于 JVM 的内存压力，基于 API 的 ReentrantLock 会开销更多的内存。

30、能对ConcurrentHashMap 做个简单介绍吗？
①、重要的常量：　　

private transient volatile int sizeCtl; 　　

当为负数时，-1 表示正在初始化，-N 表示 N - 1 个线程正在进行扩容；　　

当为 0 时，表示 table 还没有初始化；　　

当为其他正数时，表示初始化或者下一次进行扩容的大小。

②、数据结构：　　

Node 是存储结构的基本单元，继承 HashMap 中的 Entry，用于存储数据； 

TreeNode 继承 Node，但是数据结构换成了二叉树结构，是红黑树的存储结构，用于红黑树中存储数据；　　

TreeBin 是封装 TreeNode 的容器，提供转换红黑树的一些条件和锁的控制。

③、存储对象时（put() 方法）：　　

如果没有初始化，就调用 initTable() 方法来进行初始化；　　

如果没有 hash 冲突就直接 CAS 无锁插入；　　

如果需要扩容，就先进行扩容；　　

如果存在 hash 冲突，就加锁来保证线程安全，两种情况：一种是链表形式就直接遍历到尾端插入，一种是红黑树就按照红黑树结构插入；　　

如果该链表的数量大于阀值 8，就要先转换成红黑树的结构，break 再一次进入循环 　　

如果添加成功就调用 addCount() 方法统计 size，并且检查是否需要扩容。

④、扩容方法 transfer()：默认容量为 16，扩容时，容量变为原来的两倍。　　helpTransfer()：调用多个工作线程一起帮助进行扩容，这样的效率就会更高。

⑤、获取对象时（get()方法）：　　

计算 hash 值，定位到该 table 索引位置，如果是首结点符合就返回；　　

如果遇到扩容时，会调用标记正在扩容结点 ForwardingNode.find()方法，查找该结点，匹配就返回；　　

以上都不符合的话，就往下遍历结点，匹配就返回，否则最后就返回 null。

## 31、熟悉ConcurrentHashMap 的并发度吗？
程序运行时能够同时更新 ConccurentHashMap 且不产生锁竞争的最大线程数。默认为 16，且可以在构造函数中设置。当用户设置并发度时，ConcurrentHashMap 会使用大于等于该值的最小2幂指数作为实际并发度（假如用户设置并发度为17，实际并发度则为32）。