# Java equals和==的区别

## JVM中的内存分配知识
在JVM中，内存分为堆内存跟栈内存。他们二者的区别是：当我们创建一个对象(new Object)时，就会调用对象的构造函数来开辟空间，将对象数据存储到堆内存中，与此同时在栈内存中生成对应的引用，当我们在后续代码中调用的时候用的都是栈内存中的引用。还需注意的一点，基本数据类型是存储在栈内存中。

## equals与==的区别：
- ==是判断两个变量或实例是不是指向同一个内存空间，equals是判断两个变量或实例所指向的内存空间的值是不是相同 
- ==是指对内存地址进行比较 ， equals()是对字符串的内容进行比较
- ==指引用是否相同， equals()指的是值是否相同

![](https://github.com/chaofengliu/AndroidInterviews/blob/main/img/20180602183200272.png)

```
//比较的是物理地址，结果为false
Object obj = new Object();
Object obj1 = new Object();
System.out.println(obj.equals(obj1));//false
System.out.println(obj==obj1);//false

//String、Integer等类对equals进行了重写
//重写equals方法并不能改变hashcode值，在java中首先比较的就是hashcode
System.out.println(obj.toString().equals(obj1.toString());//false
```
## equals与==的区别详解：

- == 比较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是否是指相同一个对象。比较的是真正意义上的指针操作。
- equals用来比较的是两个对象的内容是否相等，由于所有的类都是继承自java.lang.Object类的，所以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是Object类中的方法，而Object中的equals方法返回的却是==的判断。

```
//a为一个引用
String a = new String("abcd");
//b为另一个引用，a和b的对象内容是一致的
String b = new String("abcd");
//把"abcd"放在常量池中
String c = "abcd";
//从常量池中查找"abcd"
String d = "abcd";
if(a == b){//false 非同一对象
	System.out.println("a==b");
}
//true String中equals被重写，物理地址不一样时，会比较值
if(a.equals(b)){
	System.out.println("a=b");
}
//true
if(d == c){
	System.out.println("d==c");
}
//true
if(c.equals(d)){
	System.out.println("c==d");
}
```

- String s="abcd"是一种非常特殊的形式,和new有本质的区别。它是java中唯一不需要new就可以产生对象的途径。
- 
- 以String s="abcd"形式赋值在java中叫直接量,它是在常量池中而不是象new一样放在压缩堆中。 这种形式的字符串，在JVM内部发生字符串拘留，即当声明这样的一个字符串后，JVM会在常量池中先查找有有没有一个值为"abcd"的对象,如果有,就会把它赋给当前引用。即原来那个引用和现在这个引用指点向了同一对象, 如果没有,则在常量池中新创建一个"abcd",下一次如果有String s1="abcd"又会将s1指向"abcd"这个对象,即以这形式声明的字符串,只要值相等,任何多个引用都指向同一对象。

- 而String s = new String("abcd")和其它任何对象一样.每调用一次就产生一个对象，只要它们调用。也可以这么理解: String str = "hello"; 先在内存中找是不是有"hello"这个对象,如果有，就让str指向那个"hello".如果内存里没有"hello"，就创建一个新的对象保存"hello". String str=new String ("hello") 就是不管内存里是不是已经有"hello"这个对象，都新建一个对象保存"hello"。
