Android Studio 创建一个项目的时候，rootProject下面会生成gradle.properties和local.properties文件。其中，gradle.properties中的内容不需要显示调用就可以直接在build.gradle中进行使用。在同一个项目中，build.gradle可以直接获取其同级或者其父级(父级也要有build.gradle)的properties文件

addr2line查看native异常：
arm-linux-androideabi-addr2line -cfe $symbol_file 0xyyyyyyyy
其中，&symbol是对应库作为的文件，而0xyyyy则是需要转换的地址

IntentFilter 过滤规则
其中包括：action、category、data，三个选项中的每个选项可以存在多个。
如果过滤规则中三者都存在，那么必须三者都匹配则才能匹配成功。

系统在调用startActivity或者startActivityForResult的时候会默认为intent加上“android.intent.category.DEFAULT”,因此为了使得Activity能够隐式调用，必须在intent-filter中指定“android.intent.category.DEFAULT”。

IPC[Inter-Process Communication] 进程间通信或者跨进程通信。
android中java层启动多进程需要设置android:process属性
android:process=":remote"  
    ":" 表示要在当前的进程名字前面加上当前的包命，属于当前进程的私有进程，其他应用的组件不可以和其跑在同一条进程中。
android:process="com.xxx.org.remote" 
    此中方式配置的进程是全局的进程，其他引用可以通过shareUID方式和其跑在同一个进程中。
    两个应用通过shareUID跑在同一个进程中需要这两个应用有相同的shareUID并且具有相同的签名

Serializable 序列化时候 static和transient成员不会被序列化

AIDL实现的IPC是阻塞式的.    

AIDL文件就是通信双方的协议接口文件，IDE会自动更具AIDL文件生成通信的客户端java文件，服务端具体业务可以通过实现 IXXX.Stub。客户端在获取到服务端的Binder后，需要利用IXXX.Stub.asInterface 转换为对应接口的实现。如果被调用的服务Binder与调用在同一个进程中，那么返回的就是LocalInterface，否则返回的是服务端BInder的Proxy。

ContentProvider
    当调用ContentProvider与调用者在一个进程中的时，那么ContentProvider被调用的方法则与其调用者跑在同一条线程中。
    当调用ContentProvider与调用者不在一个进程中的时，那么ContentProvider被调用的方法则跑在Binder线程池中，IPC原理。
    ContentProvider 不仅支持传统的CRUD方法调用，还支持自定义call方法调用。

同一个SQLiteDatabase内部对数据库的操作是同步处理的。

View处理相关的工具类
Velocity 计算速度的工具类
GestureDetector 手势处理类
Scroller 滚动处理类

View平移：
1、scrollTo、scrollBy，只能改变view的内容的位置，不能改变View的位置。
    mScrollX：
        从左往右滑动为负值，从右往左为正值。
    mScrollY：
        从右往左滑动为负值，从左往右为正值。
2、动画
3、改变LayoutParams

dispatchTouchEvent、onInterceptTouchEvent都是有父View触发调用。在重写这些方法时，注意需要调用super的该方法，否则处理逻辑就混乱了[不符合系统事件处理规范]。

... Activity#dispatchTouchEvent -> dispatchTouchEvent -> onInterceptTouchEvent -> OnTouchListener[onTouch false] -> onTouchEvent -> OnClickListener
onInterceptTouchEvent、OnTouchListener是在dispatchTouchEvent里面调用的。
OnClickListener是在onTouchEvent里面调用的。

android底层都是基于事件驱动的。
Touch事件传递：
    底层驱动接收到事件，会封装成消息，post到main handler messagequeue里面，...,最后到Activity，再派发到Activity里面的View。在Activity#dispatchTouchEvent调用Thread.dumpStack()打印调用栈，可以查看具体的调用顺序。
























