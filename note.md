# GCC命令
gcc [options] [filenames]
options为编译选项
-o 指定生成的可执行文件的文件名称

-c 选项告诉GCC仅把源代码编译为目标代码而跳过汇编和连接的步骤；

-S 编译选项告诉GCC 在为 C代码产生了汇编语言文件后停止编译。GCC 产生的汇编语言文件的缺省扩展名是.s

-E选项指示编译器仅对输入文件进行预处理。当这个选项被使用时，预处理器的输出被送到标准输出（默认为屏幕）而不是储存在文件里。

-O选项告诉GCC对源代码进行基本优化从而使得程序执行地更快；而-O2选项告诉GCC产生尽可能小和尽可能快的代码。使用-O2选项编译的速度比使用-O时慢，但产生的代码执行速度会更快。

-g选项告诉GCC产生能被GNU调试器使用的调试信息以便调试你的程序，可喜的是，在GCC里，我们能联用-g和-O (产生优化代码)。

-pg选项告诉GCC在你的程序里加入额外的代码，执行时，产生gprof用的剖析信息以显示你的程序的耗时情况。

# GDB调试器
  GCC用于编译程序，而Linux的另一个GNU工具gdb则用于调试程序。gdb是一个用来调试C和C++程序的强力调试器，我们能通过它进行一

系列调试工作，包括设置断点、观查变量、单步等。

其最常用的命令如下：

file：装入想要调试的可执行文件。

kill：终止正在调试的程序。

list：列表显示源代码。

next：执行一行源代码但不进入函数内部。

step：执行一行源代码而且进入函数内部。

run：执行当前被调试的程序

quit：终止gdb

watch：监视一个变量的值

break：在代码里设置断点，程序执行到这里时挂起

make：不退出gdb而重新产生可执行文件

shell：不离开gdb而执行shell


# 解释编译和连接

编译器使用源码文件来产生某种形式的目标文件(object files)，在编译过程中，外部的符号参考并没有被解释或替换（即外部全局变量和函数并没有被找到）。因此，在编译阶段所报的错误一般都是语法错误。而连接器则用于连接目标文件和程序包，生成一个可执行程序。在连接阶段，一个目标文件中对别的文件中的符号的参考被解释，如果有符号不能找到，会报告连接错误。编译和连接的一般步骤是：第一阶段把源文件一个一个的编译成目标文件，第二阶段把所有的目标文件加上需要的程序包连接成一个可执行文件。

GNU Make的主要工作是读进一个文本文件，称为makefile。这
个文件记录了哪些文件（目的文件，目的文件不一定是最后的可执行程序，它可以是任何一种文件）由哪些文件（依靠文件）产生，用什么命令来产生。Make依靠此makefile中的信息检查磁盘上的文件，如果目的文件的创建或修改时间比它的一个依靠文件旧的话，make就执行相应的命令，以便更新目的文件。

在makefile中加入变量，另外。环境变量在make过程中也被解释成make的变量。这些变量是大小写敏感的，一般使用大写字母。Make变量可以做很多事情，例如：

i) 存储一个文件名列表；

ii) 存储可执行文件名；

iii) 存储编译器选项。

要定义一个变量，只需要在一行的开始写下这个变量的名字，后面跟一个=号，再跟变量的值。引用变量的方法是写一个$符号，后面跟（变量名）

Linux进程在内存中包含三部分数据：代码段、堆栈段和数据段。代码段存放了程序的代码。代码段可以为机器中运行同一程序的数个进程共享。堆栈段存放的是子程序（函数）的返回地址、子程序的参数及程序的局部变量。而数据段则存放程序的全局变量、常数以及动态数据分配的数据空间（比如用malloc函数申请的内存）。与代码段不同，如果系统中同时运行多个相同的程序，它们不能使用同一堆栈段和数据段。

# 进程间通信

Linux的进程间通信（IPC，InterProcess Communication）通信方法有管道、消息队列、共享内存、信号量、套接口等。管道分为有名管道和无名管道，无名管道只能用于亲属进程之间的通信，而有名管道则可用于无亲属关系的进程之间。

消息队列用于运行于同一台机器上的进程间通信，与管道相似；
共享内存通常由一个进程创建，其余进程对这块内存区进行读写。得到共享内存有两种方式：映射/dev/mem设备和内存映像文件。前一种方式不给系统带来额外的开销，但在现实中并不常用，因为它控制存取的是实际的物理内存；常用的方式是通过shmXXX函数族来实现共享内存：

# 驱动程序
设备驱动程序是操作系统内核和机器硬件之间的接口，它为应用程序屏蔽硬件的细节，一般来说，Linux的设备驱动程序需要完成如下功能：

（1）初始化设备；

（2）提供各类设备服务；

（3）负责内核和设备之间的数据交换；

（4）检测和处理设备工作过程中出现的错误。

源文件首先会生成中间目标文件，再由中间目标文件生成执行文件。在编译时，编译器只检测程序语法，和函数、变量是否被声明。如果函数未被声明，编译器会给出一个警告，但可以生成Object File。而在链接程序时，链接器会在所有的Object File中找寻函数的实现，如果找不到，那到就会报链接错误码（Linker Error）

 Makefile文件用来告诉make命令需要怎么样的去编译和链接程序


# Makefile的规则
基本格式：

  target: prerequisites

  command（指令）

target也就是一个目标文件，可以是Object File，也可以是执行文件。还可以是一个标签（Label）
prerequisites就是，要生成那个target所需要的文件或是目标。command也就是make需要执行的命令（任意的Shell命令）。这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件，其生成规则定义在command中。说白一点就是说，prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。这就是Makefile的规则。也就是Makefile中最核心的内容。

默认的情况下，make命令会在当前目录下按顺序找寻文件名为“GNUmakefile”、“makefile”、“Makefile”的文件，找到了解释这个文件。


## GNU的make工作时的执行步骤入下：（其它的make也是类似）

1.    读入所有的Makefile。

2.    读入被include的其它Makefile。

3.    初始化文件中的变量。

4.    推导隐晦规则，并分析所有规则。

5.    为所有的目标文件创建依赖关系链。

6.    根据依赖关系，决定哪些目标要重新生成。

7.    执行生成命令。

# Makefile书写规则

 规则包含两个部分，一个是依赖关系，一个是生成目标的方法。

在Makefile中，规则的顺序是很重要的，因为，Makefile中只应该有一个最终目标，其它的目标都是被这个目标所连带出来的，所以一定要让make知道你的最终目标是什么。一般来说，定义在Makefile中的目标可能会有很多，但是第一条规则中的目标将被确立为最终的目标。如果第一条规则中的目标有很多个，那么，第一个目标会成为最终的目标。make所完成的也就是这个目标。

# 使用通配符
make支持三个通配符：“*”，“?”和“[...]”

"~"      //在linux中代表用户根目录

#include<> 引用的是编译器的类库路径里面的头文件。

#include" " 引用的是你程序目录的相对路径中的头文件。没找到，则去类库路径下面找

# GCC && G++
gcc只能编译C语言编写的程序，有的程序是用C++写的，编译的时候就要使用G++，或者手动加上标准C++库。编译可以用gcc/g++，但链接用g++或者gcc -lstdc++。因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常使用g++来完成联接。但在编译阶段，g++会自动调用gcc，二者等价。
# C代码 C++代码混编
C和CPP代码混编的时候需要注意 由于GCC和G++链接的问题

# 什么是gcc / g++

首先说明：gcc 和 GCC 是两个不同的东西

GCC:GNU Compiler Collection(GUN 编译器集合)，它可以编译C、C++、JAV、Fortran、Pascal、Object-C、Ada等语言。

gcc是GCC中的GUN C Compiler（C 编译器）

g++是GCC中的GUN C++ Compiler（C++编译器）

gcc和g++并不是编译器，也不是编译器的集合，它们只是一种驱动器，根据参数中要编译的文件的类型，调用对应的GUN编译器而已，比如，用gcc编译一个c文件的话，会有以下几个步骤：

Step1：Call a preprocessor, like cpp.

Step2：Call an actual compiler, like cc or cc1.

Step3：Call an assembler, like as.

Step4：Call a linker, like ld

由于编译器是可以更换的，所以gcc不仅仅可以编译C文件

所以，更准确的说法是：gcc调用了C compiler，而g++调用了C++ compiler

gcc和g++的主要区别

1. 对于 *.c和*.cpp文件，gcc分别当做c和cpp文件编译（c和cpp的语法强度是不一样的）

2. 对于 *.c和*.cpp文件，g++则统一当做cpp文件编译

3. 使用g++编译文件时，g++会自动链接标准库STL，而gcc不会自动链接STL

4. gcc在编译C文件时，可使用的预定义宏是比较少的

5. gcc在编译cpp文件时/g++在编译c文件和cpp文件时（这时候gcc和g++调用的都是cpp文件的编译器），会加入一些额外的宏，这些宏如下：

#define __GXX_WEAK__ 1
#define __cplusplus 1
#define __DEPRECATED 1
#define __GNUG__ 4
#define __EXCEPTIONS 1
#define __private_extern__ extern

6. 在用gcc编译c++文件时，为了能够使用STL，需要加参数 –lstdc++ ，但这并不代表 gcc –lstdc++ 和 g++等价，它们的区别不仅仅是这个

# 主要参数

-g - turn on debugging (so GDB gives morefriendly output)

-Wall - turns on most warnings

-O or -O2 - turn on optimizations

-o - name of the output file

-c - output an object file (.o)

-I - specify an includedirectory 指定头文件的位置

-L - specify a libdirectory  指定链接库的目录

-l - link with librarylib.a 指定链接库的名字

使用示例：

  // -L/home/fred/lib 结构让gcc先在/home/fred/lib下查找库文件,然后再到默认的库文件搜索路径下进行查找.
  -l选项使得链接程序使用指定的函数库中的目标代码,也就是本例中的libnew.so

  g++ -o helloworld -I/homes/me/randomplace/include -L/home/fred/li -lnew helloworld.C

默认情况下,gcc使用共享库进行链接,所以在需要链接静态库时必须使用-static选项来保证只使用静态库.


由于 Makefile 中包含众多的目标文件以及依赖文件，为了简化,引入了自动变量这一表示。下面是常见的一些自动变量：

$*     不包含扩展名的目标文件名称
$<     第一个依赖文件的名称
$@     目标文件的完整名称
$^     所有不重复的依赖文件

# g++ 常用命令选项

-ansi	只支持 ANSI 标准的 C 语法。这一选项将禁止 GNU C 的某些特色， 例如 asm 或 typeof 关键词。
-c	只编译并生成目标文件。
-DMACRO	以字符串"1"定义 MACRO 宏。
-DMACRO=DEFN	以字符串"DEFN"定义 MACRO 宏。
-E	只运行 C 预编译器。
-g	生成调试信息。GNU 调试器可利用该信息。
-IDIRECTORY	指定额外的头文件搜索路径DIRECTORY。
-LDIRECTORY	指定额外的函数库搜索路径DIRECTORY。
-lLIBRARY	连接时搜索指定的函数库LIBRARY。
-m486	针对 486 进行代码优化。
-o	FILE 生成指定的输出文件。用在生成可执行文件时。
-O0	不进行优化处理。
-O	或 -O1 优化生成代码。
-O2	进一步优化。
-O3	比 -O2 更进一步优化，包括 inline 函数。
-shared	生成共享目标文件。通常用在建立共享库时。
-static	禁止使用共享连接。
-UMACRO	取消对 MACRO 宏的定义。
-w	不生成任何警告信息。
-Wall	生成所有警告信息。

# 静态库和动态库
## 静态库和动态库
库文件有动态和静态之分，他们的命名规范为 lib库名.后缀，在链接目标文件和库时，使用 -l 库名（空格可省略）选项，也可以添加-L /path来规定优先搜索库文件的目录。

例如：C中的数学函数库math.h的动态库文件名为libm.so，那么我们编译连接文件时就需要添加-lm的选项。如果要指定库文件路径为/usr/lib64/libm.so，那么可添加-L /usr/lib64来指定库文件优先查找目录。另外静态和动态库文件搜索目录顺序不一样。

## 静态库
静态库文件一般是以.a为后缀的库文件，它在编译连接时会将库文件的内容全部添加到可执行文件中，在编译连接完成后，静态库文件便不再影响可执行文件。

它的优点是简单粗暴，但如果库文件内部有改动的话需要重新对所有引用此库文件的可执行文件重新编译。

一般编译步骤如下：

    gcc -c static.c -o static.o // 编译静态库文件的源文件
    ar -r static.a static.o // 生成静态库文件
    gcc -o main -lstatic // 连接静态库文件生成可执行文件
编译连接时，静态库文件搜索目录顺序为：

* 编译连接时 -L 参数指定的目录；
* 环境变量目录 LIBRARY_PATH；
* 固定目录 /lib、/usr/lib、/usr/local/lib等；


# cmake 构建工具
cmake 要求CMakeLists.txt在工程主目录或者所有存放源代码子目录下[存放源码的目录下面也可以单独的写这个目录相关的cmakelists.txt]。
使用cmake工具，需要写cmakelists文件。其实cmake就是个根据cmakelists自动生成makefile的工具，减少写makefile的工作。
注意：在使用cmake命令构建工程的时候，cmake会生成makefile等中间文件，最好是新建一个build文件夹，在build文件夹里面执行cmake命令【这样cmake生成的中间文件就会在build目录下面 不至于搞乱了工程项目】
eg：
第一步：cmake “CMakeLists.txt所在目录” //在build目录里面
第二步：make // 在build目录里面

cmake中使用target_link_library的时候，前面添加的对后面添加的有依赖，所以越底层的库越应该在下面出现


1、在不同的平台编译的时候，会用到一些系统内置的变量，比如操作系统名称，版本号之类：
CMAKE_SYSTEM：系統全名，如 "Linux-2.4.22"，"FreeBSD-5.4-RELEASE" 或 "Windows 5.1"
CMAKE_SYSTEM_NAME：系統名称，如 "Linux", "FreeBSD" or "Windows"，注意大小写
CMAKE_SYSTEM_VERSION：只显示系统全名中的版本部分
CMAKE_SYSTEM_PROCESSOR：CPU名称

2、系统标志：下面的变量都是BOOL类型的，如果与当前系统或编译器相符，值为True，反之为False
UNIX
WIN32 for MINGW,CYGWIN,MSYS
APPLE
BORLAND
WATCOM
MSVC,MSVC_IDE,CMAKE_COMPILER_2005,MSVC60/70/71/80/90/10，针对不同的Visual C++
CMAKE_COMPILER_IS_GUNCXX/CMAKE_COMPILER_IS_GUNCC

3、编译时选项：
BUIlD_SHARED_LIBS：将所有程序库的target设置成共享库
CMAKE_BUIlD_TYPE：控制构建类型，以下为可选参数
None：default；Debug：生成调试信息；Release：发布版本，进行最佳化，需要注意这个值不会在configure的时候自动初始化，需要手动指定
CMAKE_C_FLAGS
CMAKE_C_FLAGS_DEBUG
CMAKE_C_FLAGS_RELEASE

CMAKE_CXX_FLAGS
CMAKE_CXX_FLAGS_DEBUG
CMAKE_CXX_FLAGS_RELEASE


ldd 查看一个程序运行所加载的库
which a 查看a在什么位置
file a 查看a的属性
env命令查看系统变量

$@ $^ $< 自动变量只能再规则的命令中使用
$@ 目标
$^ 所有依赖
$< 第一个依赖
= 是最基本的赋值
:= 是覆盖之前的值
?= 是如果没有被赋值过就赋予等号后面的值
+= 是添加等号后面的值
