# Activity知识点汇总

1. Handler的实现原理
2. 子线程中能不能直接new一个Handler,为什么主线程可以
3. 主线程的Looper第一次调用loop方法,什么时候,哪个类
4. Handler导致的内存泄露原因及其解决方案
5. 一个线程可以有几个Handler,几个Looper,几个MessageQueue对象
6. Message对象创建的方式有哪些 & 区别？
7. Message.obtain()怎么维护消息池的
8. Handler 有哪些发送消息的方法
9. Handler的post与sendMessage的区别和应用场景
10. handler postDealy后消息队列有什么变化，假设先 postDelay 10s, 再postDelay 1s, 怎么处理这2条消息
11. MessageQueue是什么数据结构
12. Handler怎么做到的一个线程对应一个Looper，如何保证只有一个MessageQueue
13. ThreadLocal在Handler机制中的作用
14. HandlerThread是什么 & 好处 &原理 & 使用场景
15. IdleHandler及其使用场景
16. 消息屏障,同步屏障机制
17. 子线程能不能更新UI
18. 为什么Android系统不建议子线程访问UI
19. Android中为什么主线程不会因为Looper.loop()里的死循环卡死
MessageQueue#next 在没有消息的时候会阻塞，如何恢复？
20. Handler消息机制中，一个looper是如何区分多个Handler的
21. 当Activity有多个Handler的时候，怎么样区分当前消息由哪个Handler处理
22. 处理message的时候怎么知道是去哪个callback处理的
23. Looper.quit/quitSafely的区别
24. 通过Handler如何实现线程的切换
25. Handler 如何与 Looper 关联的
26. Looper 如何与 Thread 关联的
27. Looper.loop()源码
28. MessageQueue的enqueueMessage()方法如何进行线程同步的
29. MessageQueue的next()方法内部原理
20. 子线程中是否可以用MainLooper去创建Handler，Looper和Handler是否一定处于一个线程
31. ANR和Handler的联系

