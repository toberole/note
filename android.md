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















