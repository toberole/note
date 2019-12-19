xcode快捷键
    cmd+shift+y 关闭打开log面板
    cmd+a ctrl+i 代码格式化
    ctrl+k 删除本行

OC中的关键字都是@开头

import 可以避免头文件重复包含的问题，最好不要使用c风格的include

类型都需要“()” 包起来

方法调用需要放在"[]"中，[x y]:调用x类的y方法

"."调用 代表get和set方法

Xcode判断是否为init方法规则：
    方法返回id，并且名字以init +大写字母开头+其他。

"%@" 打印对象 NSLog(@"%@",stu);

description类toString方法

类的成员变量默认是protectd
类的私有方法写在 “.m”文件中，没有在“.h”文件中声明，就是私有的

从Xcode4.4以后@property已经独揽了@synthesize的功能主要有三个作用：
 (1)生成了成员变量get/set方法的声明
 (2)生成了私有的带下划线的的成员变量因此子类不可以直接访问，但是可以通过get/set方法访问。那么如果想让定义的成员变量让子类直接访问那么只能在.h文件中定义成员　　　　变量了，因为它默认是@protected
 (3)生成了get/set方法的实现
@property、@synthesize: 
@property编译器会自动实现 用在头文件中 setter和getter
    @property age 编译器会自动生成setAge和getAge
@property 用于声明

使用@synthesize，编译器会在类中自动生成一个成员变量 用在实现文件中
eg:
    @synthesize xxx 生成成员变量xxx
    @synthesize xxx=yyy 属性xxx与成员变量yyy关联 ，操作属性xxx相当于操作yyy

只使用property声明变量x，编译会生成一个成员变量_x
同时使用property和synthesize会生成x

@dynamic 编译器不要自动生成 

任何继承子NSObject的对象内部都有一个引用计数器，当使用alloc、new、copy创建一个对象时，对象的引用计数都会被设置为1。调用对象的retain使得引用计数加1，调用release使得引用计数减1，当对象的引用计数为0时，对象会被回收 系统会自动调用对象的dealloc方法。通过调用retainCount可以获取对象的引用计数。

内存管理：
assign：
    该方法只会针对“纯量类型”(CGFloat或NSInteger等)的简单赋值操作，id类型也要用assign。
strong: 
    使用该特性实例变量在赋值时，会释放旧值同时设置新值，对对象产生一个强引用，用MRC来说就是引用计数+1。
weak: 
    属性表明了一种”非拥有关系“，既不释放旧值，也不保留新值。用MRC就是引用计数不变，当指向的对象被释放时，该属性自动被设置为nil。weak的runtime实现是通过hash表完成的，用变量名做键，一旦发现属性所指的对象被释放了，立刻设置为nil。
unsafe_unretained：
    和weak一样，唯一的区别就是当对象被释放后，该属性不会被设置为nil。
copy：
    和strong类似，不过该属性会被复制一个新的副本。很多时使用copy是为了方式Mutable（可变类型）在我们不知道的情况下修改了属性值，而用copy可以生成一个不可变的副本防止被修改。如果我们自己实现setter方法的话，需要手动copy。

dealloc 对象释放的时候会自动的调用该方法

#pragma mark 注释

typeof(xxx) 注意不同于C语言，OC中是指返回xxx的类型


类别与扩展[匿名类别]
类别：
①为现有的类添加新方法；
②将类的实现分散到多个不同文件或多个不同框架中(把一个大的类按功能划分成几块,便于维护)
③通过替换现有类中的方法，修正现有类(没有源码文件的情况下)的功能
扩展：
可以在类扩展中声明属性和实例变量。
类扩展声明必须在 @implementation在实现。
所以类扩展的成员变量，方法，都不能被外部方法访问。
1、类别中原则上只能增加方法（能添加属性的的原因通过runtime解决无setter/getter的问题，添加的属性是共有的，本类和子类都能访问）；

2、类扩展中声明的方法没被实现，编译器会报警，但是类别中的方法没被实现编译器是不会有任何警告的。这是因为类扩展是在编译阶段被添加到类中，而类别是在运行时添加到类中。

3、类扩展不能像类别那样拥有独立的实现部分（@implementation部分），也就是说，类扩展所声明的方法必须依托对应类的。m部分来实现。

4、类扩展不仅可以增加方法，还可以增加实例属性；定义在 .m 文件中的类扩展方法和属性为私有的，定义在 .h 文件（新建扩展文件的。h）中的类扩展方法和属性为公有的。类扩展是在 .m 文件中声明私有方法的非常好的方式。
扩展通常用来在类的实现文件中添加私有的成员变量、属性和方法

Selector:
1）编译时，通过编译器指令 @selector 来获取.
SEL aSelector = @selector(methodName);
2）运行时，通过字符串来获取一个方法名 NSSelectorFromString
SEL aSelector = NSSelectorFromString(@”methodName”);

使用Selector:
使用已经创建好的Selector。你可以通过 performSelector : 来调用某个方法。
// 创建一个run方法选择器 
SEL aSelector = @selector(run);
// 通过 performSelector: 来调用对象的 run 方法 
[aDog performSelector:aSelector]; 
[anStudent performSelector:aSelector];

block:代码块
returnType (^blockName)(parameterTypes) = ^(parameters) {
        statements;
};
block 默认不能访问局部变量,可以在局部变量前面加__block使其可以被访问。

GCD：
    Grand Central Dispatch 基于C语言的一种支持并行操作的机制,。
在 GCD 中有两种队列：串行队列和并发队列。两者都符合 FIFO（先进先出）的原则。两者的主要区别是：执行顺序不同，以及开启线程数不同。

串行队列（Serial Dispatch Queue）：

每次只有一个任务被执行。让任务一个接着一个地执行。（只开启一个线程，一个任务执行完毕后，再执行下一个任务）

并发队列（Concurrent Dispatch Queue）：

可以让多个任务并发（同时）执行。（可以开启多个线程，并且同时执行任务）

注意：并发队列的并发功能只有在异步（dispatch_async）函数下才有效

多线程
// 创建串行队列
dispatch_queue_t sq = dispatch_queue_create(NULL, DISPATCH_QUEUE_SERIAL);
    
// 创建并发队列
dispatch_queue_t cq = dispatch_queue_create(NULL, DISPATCH_QUEUE_CONCURRENT);
对于串行队列，GCD 提供了的一种特殊的串行队列：主队列（Main Dispatch Queue）。
- 所有放在主队列中的任务，都会放到主线程中执行。
- 可使用dispatch_get_main_queue()获得主队列。
// 主队列的获取方法
dispatch_queue_t queue = dispatch_get_main_queue();

对于并发队列，GCD 默认提供了全局并发队列（Global Dispatch Queue）。

可以使用dispatch_get_global_queue来获取。需要传入两个参数。第一个参数表示队列优先级，一般用DISPATCH_QUEUE_PRIORITY_DEFAULT。第二个参数暂时没用，用0即可。
// 全局并发队列的获取方法
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);

创建任务：
GCD 提供了同步执行任务的创建方法dispatch_sync和异步执行任务创建方法dispatch_async。

两种队列（串行队列/并发队列+两种任务执行方式（同步执行/异步执行）
1、同步执行 + 并发队列
2、异步执行 + 并发队列
3、同步执行 + 串行队列
4、异步执行 + 串行队列
两种特殊队列：全局并发队列、主队列。
全局并发队列可以作为普通并发队列来使用。但是主队列因为有点特殊，所以我们就又多了两种组合方式。这样就有六种不同的组合方式了。
5、同步执行 + 主队列
6、异步执行 + 主队列

ios4之前系统不支持多线程

IOS程序中显示的都是UIView[或者是起子类]，View的显示以及操作都是由UIViewController控制的。UIViewController之间的切换一般情况下都是由UINavigationController、UITabBarController、UISplitViewController控制的。

UIResponder代表一个可以接收屏幕触摸事件对象。

关联返回类型:
根据Cocoa的命名规则，满足下述规则的方法：
（1）类方法中，以alloc或new开头
（2）实例方法中，以autorelease，init，retain或self开头
会返回一个方法所在类类型的对象，这些方法就被称为是关联返回类型的方法。换句话说，这些方法的返回结果以方法所在的类为类型。

instancetype的作用，就是使那些非关联返回类型的方法返回所在类的类型！

__strong修饰符是id类型和对象类型默认的所有权修饰符。
 __strong修饰符表示对对象的强引用，持有强引用的变量在出其作用域时被废弃，随着强引用的失效，引用的对象随之释放。
 __weak 解决循环引用，带有__weak修饰符的变量不持有对象，所以在超出其变量作用域时，对象即被释放。
在block中调用self会引起循环引用，但是在block中需要对weakSelf进行strong,保证代码在执行到block中，self不会被释放，当block执行完后，会自动释放该strongSelf。












