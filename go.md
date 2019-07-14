goroot：
    go安装路径
gopath：
    工作路径，其下一般包含bin、src、pkg目录
    go编译时找自定义包默认就是在gopath/src下面的。
    可以设置多个gopath，下载的包默认会下载安装在第一个gopath下面的bin目录。

go get
1. 从远程下载需要用到的包
2. 执行go install

go install
如果是编译的可执行文件，会生成可执行文件直接放到bin目录下；如果是一个普通的包，会被编译生成到pkg目录下该文件是.a结尾


go的工具会用到GOPATH环境变量.
GOPATH是作为编译后二进制的存放目的地和import包时的搜索路径 (其实也是工作目录, 可以在src下创建自己的go源文件, 然后开始工作)。
    GOPATH之下主要包含三个目录: bin、pkg、src
    bin目录主要存放可执行文件; pkg目录存放编译好的库文件, 主要是*.a文件; src目录下主要存放go的源文件，不要把GOPATH设置成go的安装路径,

go_project     // go_project为GOPATH目录
  -- bin
     -- myApp1  // 编译生成
     -- myApp2  // 编译生成
     -- myApp3  // 编译生成
  -- pkg
  -- src
     -- myApp1     // project1
        -- models
        -- controllers
        -- others
        -- main.go 
     -- myApp2     // project2
        -- models
        -- controllers
        -- others
        -- main.go 
     -- myApp3     // project3
        -- models
        -- controllers
        -- others
        -- main.go 

package 名称为 main 的包可以包含 main 函数。

一个可执行程序有且仅有一个 main 包。

通过 import 关键字来导入其他非 main 包

":=" 声明并且赋值
a,b := 10,"hello" 自动类型推导

