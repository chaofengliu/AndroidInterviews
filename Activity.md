# Activity知识点汇总

## 1.Activity生命周期
![生命周期](https://github.com/chaofengliu/AndroidInterviews/blob/main/img/0_1314838777He6C.png "Activity生命周期图")

- onCreate():
当我们点击activity的时候，系统会调用activity的onCreate()方法，在这个方法中我们会初始化当前布局setContentLayout()方法。

- onStart():
onCreate()方法完成后，此时activity进入onStart()方法,当前activity是用户可见状态，但没有焦点，与用户不能交互，一般可在当前方法做一些动画的初始化操作。

- onResume():
onStart()方法完成之后，此时activity进入onResume()方法中，当前activity状态属于运行状态 (Running)，可与用户进行交互。

- onPouse():
当另外一个activity覆盖当前的acitivty时，此时当前activity会进入到onPause()方法中，当前activity是可见的，但不能与用户交互状态。

- onStop():
onPause()方法完成之后，此时activity进入onStop()方法，此时activity对用户是不可见的，在系统内存紧张的情况下，有可能会被系统进行回收。所以一般在当前方法可做资源回收。

- onDestory():
onStop()方法完成之后，此时activity进入到onDestory()方法中，结束当前activity。

- onRestart():
onRestart()方法在用户按下home()之后，再次进入到当前activity的时候调用。调用顺序onPouse()->onStop()->onRestart()->onStart()->onResume()

## 2.Activity的四个状态

- Running->当前显示在屏幕的activity(位于任务栈的顶部)，用户可见状态。

- Paused->依旧在用户可见状态，但是界面焦点已经失去，此Activity无法与用户进行交互。

- Stopped->用户看不到当前界面,也无法与用户进行交互 完全被覆盖.

- Killed->当前界面被销毁，等待这系统被回收

## 3.当AActivity切换BActivity的所执行的方法

AActivity:onCreate()->onStart()->onResume()->onPause()

BActivity:onCreate()->onStart()->onResume()

AActivity:onStop()->onDestory()

## 4.AActivity切换BActivity（此activity是以dialog形式存在的）所执行的方法:

AActivity:onCreate()->onStart()->onResume()-> onPause()

BActivity:onCreate()->onStart()->onResume()


## 5. Activity的启动流程 

## 6.onSaveInstanceState()和onRestoreInstanceState的调用时机

onSaveInstanceState函数在Activity生命周期中执行。

###outState参数作用:
数据保存，Activity声明周期结束的时候, 需要保存 Activity 状态的时候, 会将要保存的数据使用键值对的形式 保存在 Bundle 对象中;


### 调用时机 :

- Activity被销毁的时候调用, 也可能没有销毁就调用了;

- 按下Home键:Activity进入了后台, 此时会调用该方法;

- 按下电源键:屏幕关闭, Activity进入后台;

- 启动其它Activity:Activity被压入了任务栈的栈底;

- 横竖屏切换 : 会销毁当前Activity并重新创建；

### onSaveInstanceState方法调用注意事项:
- 用户主动销毁不会调用:当用户点击回退键或者调用了finish()方法, 不会调用该方法;
- 调用时机不固定 : 该方法一定是在onStop()方法之前调用, 但是不确定是在onPause()方法之前还是之后调用;
- 布局中组件状态存储 : 每个组件都实现了onSaveInstance()方法, 在调用函数的时候, 会自动保存组件的状态, 注意, 只有有id的组件才会保存;
- 关于默认的super.onSaveInstanceState(outState):该默认的方法是实现组件状态保存的;

### onRestoreInstanceState方法回调时机 
- 在Activity被系统销毁之后恢复Activity时被调用, 只有销毁了之后重建的时候才调用, 如果内存充足, 系统没有销毁这个Activity, 就不需要调用;
- Bundle对象传递:该方法保存的Bundle对象在Activity恢复的时候也会通过参数传递到onCreate()方法中;


## 7.activity的启动模式和使用场景
### 1).standard模式

standard是activity默认的启动模式，在不指定Activity启动模式的情况下，所有activity使用的都是standard模式。
在standard模式下，每当启动一个新的activity，他就会进入任务栈，并处于栈顶的位置，对于使用standard模式的Activity，系统不会判断该activity在栈中是否存在，每次启动都会创建一个新的实例。

### 2).singleTop模式
singleTop模式与standard类似，不同的是，当启动的activity已经位于栈顶时，则直接使用它不创建新的实例。如果启动的activity没有位于栈顶时，则创建一个新的实例位于栈顶。

#### 使用场景

singleTop是和接收通知启动的内容显示页面。例如，某个新闻客户端的新闻内容页面，如果收到10个新闻推送，每次都打开一个新闻内容页面就不合理了。

### 3).singleTask模式
如果希望Activity在整个应用程序中只存在一个实例，可以使用singleTask模式，当activity的启动模式指定为singleTask，每次启动该activity时，系统首先会检查栈中是否存在该activity的实例，如果存在，就直接使用该实例，并将当前activity之上的所有activity出栈，如果没有发现则创建一个新的实例。

#### 使用场景

最常见的应用场景就是保持我们应用开启后仅仅有一个activity实例，最典型的样例就是应用中展示的主页（Home页）
假设用户在主页跳转到其他页面，运行多次操作后想返回到主页，假设不使用singletask模式，在点击返回的过程中会多次看到主页，这明显就是设计不合理了。

### 4).singleInstance模式
在程序开发过程中，如果需要activity在整个系统中都只有一个实例，这时需要用到singleInstance模式。不同于以上介绍的三种模式，指定为singleInstance模式的activity会启动一个新的任务栈来管理这个activity。

singleInstance模式加载activity的时候，无论从哪个任务栈中启动该activity，只会创建一个activity实例，并且会使用一个全新的任务栈来装载该activity实例，采用这种模式启动activity分为两种情况：

1)、如果要启动的activity不存在，系统会创建一个新的任务栈，再创建该activity的实例，并把该activity加入栈顶

2)、如果要启动的activity已经存在，无论位于哪个应用程序或者哪个任务栈中，系统都会把该activity所在的任务栈转到前台，从而使该activity显示出来

#### 使用场景
singleInstance适合需要与程序分离的页面，例如闹铃提醒，将闹铃提醒与闹铃设置分离，singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如;A->B(singleInstance)->C,完全退出后，再次启动，首先打开的是B


## 8.activity的进程优先级
前台进程>可见进程>service进程>后台进程>空进程

### 前台进程:
1. 当前进程activity正在与用户进行交互。
2. 当前进程service正在与activity进行交互或者当前service调用了startForground()属于前台进程或者当前service正在执行生命周期（onCreate(),onStart(),onDestory()）
3. 进程持有一个BroadcostReceiver,这个BroadcostReceiver正在执行onReceive()方法

### 可见进程:
1. 进程持有一个activity，这个activity不再前台，处于onPouse()状态下，当前覆盖的activity是以dialog形式存在的。
2. 进程有一个service，这个service和一个可见的Activity进行绑定。

### service进程:
当前开启startSerice()启动一个service服务就可以认为进程是一个服务进程。

### 后台进程:
activity的onStop()被调用，但是onDestroy()没有调用的状态。该进程属于后台进程。

### 空进程:
改进程没有任何运行的数据了，且保留在内存空间，并没有被系统killed,属于空进程。该进程很容易被杀死。



## 9. ActivityA跳转ActivityB，再按返回键，生命周期执行的顺序

## 10. 横竖屏切换,按home键,按返回键,锁屏与解锁屏幕,跳转透明Activity界面,启动一个Theme为Dialog的Activity，弹出Dialog时Activity的生命周期


## 11. onStart和onResume、onPause和onStop的区别

onStart()是activity界面被显示出来的时候执行的，用户可见，包括有一个activity在他上面，但没有将它完全覆盖，用户可以看到部分activity但不能与它交互。
onResume()是当该activity与用户能进行交互时被执行，用户可以获得activity的焦点，能够与用户交互。


onStart()通常就是onStop()（也就是用户按下了home键，activity变为后台后），之后用户再切换回这个activity就会调用onRestart()而后调用onStart()。

onResume()是onPause()（通常是当前的acitivty被暂停了，比如被另一个透明或者Dialog样式的Activity覆盖了），之后dialog取消，activity回到可交互状态，调用onResume()。


onPause： Activity是去焦点，但仍然可见；

onStop：Activity在后台不可见时触发（完全被另一个Activity挡住，或者程序在后台运行）

 

场景：

1：锁屏的时候户依次调用onPause()和onStop()

2：Toast，Dialog，menu ，三者都不会使Activity调用onPause();

3：一个非全屏的Activity在前面时，后面的Activity只调用onPause();

 

总结：

Dialog不会调用onPause()和onStop()， 非全屏Activity会调用onPause()不会调用onStop()，全屏Activity 会调用onPause()和onStop()。


## 12. Activity之间传递数据的方式Intent是否有大小限制，如果传递的数据量偏大，有哪些方案

普通的App由Zygote孵化而来的用户进程，所映射的Binder内存大小是不到1M的，准确说是1024*1024-4096 *2：这个限制定义在frameworks/native/libs/binder/processState.cpp类中，如果传输说句超过这个大小，系统就会报错，因为Binder本身就是为了进程间频繁而灵活的通信所设计的，并不是为了拷贝大数据而使用的：

```
#define BINDER_VM_SIZE ((1*1024*1024) - (4096 *2))
```

## 13. Activity的onNewIntent()方法什么时候会执行
###1).standard  
默认启动模式，每次激活Activity时都会创建Activity，并放入任务栈中，永远不会调用onNewIntent()。  
###2).singleTop  
如果在任务的栈顶正好存在该Activity的实例， 就重用该实例，并调用其onNewIntent()，否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例，只要不在栈顶，都会创建实例，而不会调用onNewIntent()，此时就跟standard模式一样)。  
###3).singleTask  
如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的onNewIntent())。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移除栈。如果栈中不存在该实例，将会创建新的实例放入栈中（此时不会调用onNewIntent()）。   
###4).singleInstance  
在一个新栈中创建该Activity实例，并让多个应用共享改栈中的该Activity实例。一旦改模式的Activity的实例存在于某个栈中，任何应用再激活改Activity时都会重用该栈中的实例，其效果相当于多个应用程序共享一个应用，不管谁激活该Activity都会进入同一个应用中。

![总结](https://github.com/chaofengliu/AndroidInterviews/blob/main/img/BA832041-DB2D-415F-B368-9B3E9AC1D318.png "汇总")

#### 备注:
为了让getIntent()方法获取到正确的Intent对象。在OnNewItent方法中需要调用setIntent(Intent intent)方法。

## 14. 显示启动和隐式启动
 启动Activity主要是通过Intent（意图）来实现。主要分为显示的和隐式的两种。

###1)、隐式启动Activity
优点：

只要知道被启动Activity的Action和Category即可，不用知道对应的类名或者是包名。

只要Activity有对应的action和Category都会被启动起来。然后提供给用户选择要启动哪一个。

需要被启动的Activity，需要在自己的AndroidManifest.xml定义对应的action 和 category。  
   
```
<activity
	android:name="com.android.activity.demo.SecondActivity"
   	android:label="@string/second_activity_name" >
   	<intent-filter>
   		<action android:name="android.intent.action.SECONDACTIVITY_START" />
      	<category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```
启动Activity的地方，把对应的Action填入即可。

```
    private void startSecondActivityByAction() {
        Intent intent = new Intent();
        intent.setAction("android.intent.action.SECONDACTIVITY_START");
        intent.addCategory(Intent.CATEGORY_DEFAULT);
        try {
            startActivity(intent);
        } catch (Exception e) {
        }
    }
```

###2)、显式的启动

#### 2.1 、通过类名类启动Activity，一般是同一个APK里面使用

```
    private void startSecondActivityByClass() {
        XLog.i(TAG, "startSecondActivityByClass()");
        Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
        try {
            startActivity(intent);
        } catch (Exception e) {
            XLog.i(TAG, "start activity error!");
        }
    }
```

#### 2.2、 通过包名加类名启动
不足：被启动的应用的包名或者类名发生变化后，就会无法启动。

```
    private void startSecondActivityByPackageName() {
        XLog.i(TAG, "startSecondActivityByPackage()");
        Intent intent = new Intent();
        intent.setClassName(getPackageName(), getPackageName() + ".SecondActivity");
        try {
            startActivity(intent);
        } catch (Exception e) {
            XLog.i(TAG, "start activity error!");
        }
    }
```

#### 2.3 、通过ComponentName启动
不足：被启动的应用的包名或者类名发生变化后，就会无法启动。

```
    private void startSecondActivityByComponent() {
        XLog.i(TAG, "startSecondActivityByComponent()");
        Intent intent = new Intent();
        intent.setComponent(new ComponentName(getPackageName(), getPackageName() + ".SecondActivity"));
        try {
            startActivity(intent);
        } catch (Exception e) {
            XLog.i(TAG, "start activity error!");
        }
    }
```

如果是Apk里面使用的话，可以使用显示或者是隐式的。
如果是不同APK的话，建议使用隐式的方式，做到低耦合。

## 15. scheme使用场景,协议格式,如何使用
### 1）什么是URL Scheme？
android中的scheme是一种页面内跳转协议，是一种非常好的实现机制，通过定义自己的scheme协议，可以非常方便跳转app中的各个页面；
通过scheme协议，服务器可以定制化告诉App跳转那个页面，可以通过通知栏消息定制化跳转页面，可以通过H5页面跳转页面等。

### 2）URL Scheme应用场景：
客户端应用可以向操作系统注册一个URL scheme，该scheme用于从浏览器或其他应用中启动本应用。通过指定的URL字段，可以让应用在被调起后直接打开某些特定页面，比如商品详情页、活动详情页等等。也可以执行某些指定动作，如完成支付等。也可以在应用内通过html页来直接调用显示app内的某个页面。

### 3）URL Scheme协议格式：

xl://goods:8888/goodsDetail?goodsId=10011002

通过上面的路径 Scheme、Host、port、path、query全部包含，基本上平时使用路径就是这样子的。

- xl代表该Scheme协议名称
- goods代表Scheme作用于哪个地址域
- goodsDetail代表Scheme指定的页面
- goodsId代表传递的参数
- 8888代表该路径的端口号

### 4）URL Scheme如何使用：

#### 1）在AndroidManifest.xml中对标签增加设置Scheme

```
<activity
            android:name=".GoodsDetailActivity"
            android:theme="@style/AppTheme">
            <!--要想在别的App上能成功调起App，必须添加intent过滤器-->
            <intent-filter>
            
                <!--协议部分，随便设置-->
                <data 
                android:scheme="xl" 
                android:host="goods" 
                android:path="/goodsDetail"
                android:port="8888"/>
                
                <!--下面这几行也必须得设置-->
                <category android:name="android.intent.category.DEFAULT"/>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.BROWSABLE"/>
            </intent-filter>
</activity>
```

#### 2）获取Scheme跳转的参数

```
Uri uri = getIntent().getData();
if (uri != null) {
    // 完整的url信息
    String url = uri.toString();
    Log.e(TAG, "url: " + uri);
    // scheme部分
    String scheme = uri.getScheme();
    Log.e(TAG, "scheme: " + scheme);
    // host部分
    String host = uri.getHost();
    Log.e(TAG, "host: " + host);
    //port部分
    int port = uri.getPort();
    Log.e(TAG, "host: " + port);
    // 访问路劲
    String path = uri.getPath();
    Log.e(TAG, "path: " + path);
    List<String> pathSegments = uri.getPathSegments();
    // Query部分
    String query = uri.getQuery();
    Log.e(TAG, "query: " + query);
    //获取指定参数值
    String goodsId = uri.getQueryParameter("goodsId");
    Log.e(TAG, "goodsId: " + goodsId);
}
```

#### 3）调用方式
- 网页上打开

```
<a href="xl://goods:8888/goodsDetail?goodsId=10011002">打开商品详情</a>
```
- 原生调用

```
Intent intent = new Intent(Intent.ACTION_VIEW,
Uri.parse("xl://goods:8888/goodsDetail?goodsId=10011002"));
startActivity(intent);
```

#### 4）如何判断一个Scheme是否有效

```
PackageManager packageManager = getPackageManager();
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("xl://goods:8888/goodsDetail?goodsId=10011002"));
List<ResolveInfo> activities = packageManager.queryIntentActivities(intent, 0);
boolean isValid = !activities.isEmpty();
if (isValid) {
    startActivity(intent);
}
```

## 16. ANR的四种场景
###1).什么是ANR
在Android上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应用程序无响应（ANR：Application Not Responding）对话框。用户可以选择让程序继续运行，但是，他们在使用你的应用程序时，并不希望每次都要处理这个对话框。因此，在程序里对响应性能的设计很重要，这样，系统不会显示ANR给用户。

###2).ANR产生的原因
ANR产生的根本原因是APP阻塞了UI线程。在android系统中每个App只有一个UI线程，是在App创建时默认生成的，UI线程默认初始化了一个消息循环来处理UI消息，ANR往往就是处理UI消息超时了。那么UI消息来源有哪些呢？主要有两种来源：

#### 2.1 来自于AMS的回调消息
在Android系统中，应用程序是有Android的四大组件组成，AMS负责对应用程序四大组件生命周期的管理，当AMS对应用程序组件的生命周期进行回调超过AMS定义的响应时间时，AMS就会报ANR。

出现这种情况，一般是因为在这些组件的回调函数里面进行了耗时操作（如网络操作、SD卡文件操作、数据库操作、大量计算等），AMS对组件常见的回调函数及超时时间如下：

```
Activity: 
onCreate(), onResume(), onDestroy(), 
onKeyDown(), onClick()等，超时时间5s

Application: 
onCreate(), onTerminate()等，超时时间5s

Service: 
onCreate(), onStart(), onDestroy()等，超时时间20s

BroadcastReceiver：
onReceiver()，前台APP广播超时时间是10s，后台App是60s
```

#### 2.2 App自己的发出的消息
除了AMS对四大组件的回调消息运行在UI线程外，有些操作也是运行在UI线程的：

```
AsyncTask: 
onPreExecute(), onProgressUpdate(), onPostExecute(), onCancel()等，超时5s

Mainthread handler: 
handleMessage(), post*(runnable r)等，超时5s
```
###3).怎样避免ANR
当我们知道ANR的产生原因之后，就可以较为轻松的避免ANR了。并且Android为我们提供了很多解决方法。主要归为一下几类。

- UI线程尽量只做跟UI相关的工作，但一些复杂的UI操作，还是需要一些技巧来处理，不如你让一个Button去setText一个10M的文本，UI肯定崩掉了，不过对于此类问题，分段加载貌似是最好的方法了。
- 让耗时的工作（比如数据库操作，I/O，连接网络或者别的有可能阻碍UI线程的操作）把它放入单独的线程处理。
- 尽量用Handler来处理UIthread和别的thread之间的交互。

###4).发布的程序怎样收集ANR异常
对于发布的程序，ANR异常是很那捕获不到的（我查找过很多资料，如果您有很好的捕获办法，欢迎再下方留言），所以我们需要采用其它的方法来分析ANR。app在产生ANR异常后，会将异常信息写入"/data/anr/traces.txt"文件我们可以通过收集用户的这个文件，就可以来获取用户产生ANR的地方了。



## 17. onCreate和onRestoreInstance方法中恢复数据时的区别

## 18. activty间传递数据的方式

## 19. 跨App启动Activity的方式,注意事项

## 20. Activity任务栈是什么

## 21. 有哪些Activity常用的标记位Flags

## 22. Activity的数据是怎么保存的,进程被Kill后,保存的数据怎么恢复的

