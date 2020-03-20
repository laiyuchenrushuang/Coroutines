# Coroutines

##  协程学习目的

“线程”的调度控制，提升多线程并发处理效率，是不是淘汰线程池了？

[===================<<<2020-03-20 起草>>>=====================]

### 一.依赖

implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.1.1'

### 二.磨刀不误砍柴工

（1）runblocking   启动协程

   协程调用，suspend,挂载函数.[ 执行阻塞的顺序执行]
  
   例子： 
   
        runblocking{
          AA()
        } 
        suspend AA(){}
        
（2） CoroutineDispatcher 协程调度器

   在线程中 协程调度Dispatchers.Default，开启是一个子线程

   例子：
   
    runBlocking(Dispatchers.Default) {
        printlnWithTime("one - from thread ${Thread.currentThread().name}")
        printlnDelayed("two - from thread ${Thread.currentThread().name}")
    }

(3) lauch 创建一个新的协程

   launch表示启动一个新的协程并且不阻塞当前线程
   
   例子：
   
     launch {
   
     }
   
  请注意,launch函数是CoroutineScope的方法,如果要调用launch必须要有CoroutineScope对象,上面代码的launch{…} 其实是 this.launch{…},而这个this是runblocking闭包中的CoroutineScope对象如果没有CoroutineScope对象,可以使用GlobalScope.launch{…},GlobalScope是一个全局的CoroutineScope对象

（4） job 协程调度
    
    runBlocking {
        val job = launch {
                
                }
    }
            
    job.join()        //在一个协程里，可以随意控制在哪里执行，控制执行顺序,如果不使用job 相当于自启一个线程[感觉非常优秀]
    [join,cancel,isAlive,isComplete...]  ->> 是不是比线程好些，有cancel

（5）async  withContext 创建一个新的协程

  和lauch差不多，但是返回的结果不一样。lauch -> Job ||  async -> Deferred
  
  async{A} 如果后面加.await() 当前A执行完成才能执行下一个
  
  这种效果可以用这个方式替代 withContext(Dispatchers.Default)
  
  例子：
  
  原：
  
    val deferred1 = async { calculateHardThings(10) }.await()
   
  现：
  
    val result1 = withContext(Dispatchers.Default) { calculateHardThings(10) }
   

（6） Dispatchers 的类型
 
  Dispatchers.Main：使用这个调度器在 Android 主线程上运行一个协程。可以用来更新UI 。在UI线程中执行

  Dispatchers.IO：这个调度器被优化在主线程之外执行磁盘或网络 I/O。在线程池中执行

  Dispatchers.Default：这个调度器经过优化，可以在主线程之外执行 cpu 密集型的工作。例如对列表进行排序和解析 JSON。在线程池中执行。

  Dispatchers.Unconfined：在调用的线程直接执行













## 感谢

https://blog.csdn.net/weixin_44407870/article/details/86652989


