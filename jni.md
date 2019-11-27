# 本地线程中调用java对象 
问题1： 
JNIEnv是一个线程相关的变量 
JNIEnv 对于每个 thread 而言是唯一的 
JNIEnv *env指针不可以为多个线程共用 
解决办法： 
但是java虚拟机的JavaVM指针是整个jvm公用的，我们可以通过JavaVM来得到当前线程的JNIEnv指针. 
可以使用javaAttachThread保证取得当前线程的Jni环境变量 
static JavaVM *gs_jvm=NULL; 
gs_jvm->AttachCurrentThread((void **)&env, NULL);//附加当前线程到一个Java虚拟机 
jclass cls = env->GetObjectClass(gs_object); 
jfieldID fieldPtr = env->GetFieldID(cls,"value","I"); 

问题2： 
不能直接保存一个线程中的jobject指针到全局变量中,然后在另外一个线程中使用它。 
解决办法： 
用env->NewGlobalRef创建一个全局变量，将传入的obj(局部变量)保存到全局变量中,其他线程可以使用这个全局变量来操纵这个java对象 
注意：若不是一个 jobject，则不需要这么做。如： 
jclass 是由 jobject public 继承而来的子类，所以它当然是一个 jobject，需要创建一个 global reference 以便日后使用。 
而 jmethodID/jfieldID 与 jobject 没有继承关系，它不是一个 jobject，只是个整数，所以不存在被释放与否的问题，可保存后直接使用。


# Android OS加载JNI Lib的方法有两种 
- JNI_OnLoad(动态注册) 
- 如果JNI Lib实现中没有定义JNI_OnLoad,则jvm调用jvm ResolveNativeMethod进行动态解析(静态注册) 
因此，当 Java 通过 System.loadLibrary 加载完 JNI 动态库后，紧接着会调用 JNI_OnLoad 的函数。 

# 静态注册基本原理
根据函数名来建立java方法和JNI函数间的一一对应关系。

静态有两个非常重要的关键字JNIEXPORT和JNICALL，这两个关键字时宏定义，主要用于说明该函数是JNI函数，在虚拟机加载so库时，如果发现函数含有上面两个宏定义时，就会链接到对应java层的native方法。


# 静态注册弊端：

1、后期类名、文件名改动，头文件所有函数将失效，需要手动改，易出错；

2、代码编写不方便，由于JNI层函数的名字必须遵循特定的格式，且名字特别长；

3、会导致工作量很大，因为必须为所有声明了native函数的java类编写JNI头文件；

4、程序运行效率低，因为初次调用native函数时需要根据根据函数名在JNI层中搜索对应的本地函数，然后建立对应关系，这个过程比较耗时。


# 动态注册
直接告诉native函数其在JNI中对应函数的指针；

动态注册的原理：
JNI 允许我们提供一个函数映射表，注册给 JVM，这样 JVM 就可以用函数映射表来调用相应的函数， 
而不必通过函数名来查找相关函数(这个查找效率很低，函数名超级长)。

# 实现过程：

利用结构体JNINativeMethod保存Java Native函数和JNI函数的对应关系；

在一个JNINativeMethod数组中保存所有native函数和JNI函数的对应关系；

在Java中通过System.loadLibrary加载完JNI动态库之后，调用JNI_OnLoad函数，开始动态注册；

JNI_OnLoad中会调用AndroidRuntime::registerNativeMethods函数进行函数注册；

AndroidRuntime::registerNativeMethods中最终调用jniRegisterNativeMethods完成注册。


# jni中引用的java对象的生命周期 

Java对象做为引用被传递到本地方法中，所有这些Java对象的引用都有一个共同的父类型jobject(相当于java中的 Object类是所有类的父类一样)。 这些对象引用都有其生命周期。在JNI中对Java对象的引用根据生命周期分为:全局引用，局部引用、弱全局引用 
1、Local Reference 本地引用， 
函数调用时传入jobject或者jni函数创建的jobejct，都是本地引用. 
其特点就是一旦JNI层函数返回，jobject就被回收掉，所以需要注意其生命周期。可以强制调用DeleteLocalRef进行立即回收。 
jstring pathStr = env->NewStringUTF(path) 
.... 
env->DeleteLocalRef(pathStr); 

2、Global Reference 全局引用 ，这种对象如不主动释放，它永远都不会被垃圾回收 
创建： env->NewGlobalRef(obj); 
释放： env->DeleteGlobalRef(obj) 
若要在某个 Native 代码返回后，还希望能继续使用 JVM 提供的参数, 或者是过程中调用 JNI 函数的返回值（比如 g_mid）， 则将该对象设为 global reference，以后只能使用这个 global reference；若不是一个 jobject，则无需这么做。 

3、Weak Global Reference 弱全局引用 
一种特殊的 Global Reference ,在运行过程中可能被垃圾回收掉，所以使用时请务必注意其生命周期及随时可能被垃圾回收掉,比如内存不足时。 
使用前可以利用JNIEnv的 IsSameObject 进行判定它是否被回收 
env->IsSameObject(obj1,obj2); 

JNI 层局部引用的实现
一个对象从JVM传递给本地方法时，就把控制权移交了过去，JVM会为每一个对象的传递创建一条记录，一条记录就是一个本地代码 中的引用和JVM中的对象的一个映射。记录中的对象不会被GC回收。所有传递到本地代码中的对象和从JNI函数返回的对象都被自动 地添加到映射表中。当本地方法返回时，VM会删除这些映射，允许GC回收记录中的数据。

JNI导致的异常，不会立刻终止native方法的执行。