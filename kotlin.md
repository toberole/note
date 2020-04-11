定义变量：
var：可变变量
val：只读变量
var有setter和getter,val只有getter

"?" 表示可为空
"!!" 表示不能为空

lateinit和 by lazy
    lateinit 只能用在var类型，lateinit不能用在可空的属性上和java的基本类型上
    by lazy { } 只能用在val

companion代码块中的方法和属性都是静态的

根据返回结果，作用域函数可以分为以下两类：
1、apply 及 also 返回上下文对象。
2、let、run 及 with 返回 lambda 表达式结果.
选择作用域函数指南：
对一个非空（non-null）对象执行 lambda 表达式：let
将表达式作为变量引入为局部作用域中：let
对象配置：apply
对象配置并且计算结果：run
在需要表达式的地方运行语句：非扩展的 run
附加效果：also
一个对象的一组函数调用：with

