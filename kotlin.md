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