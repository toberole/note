JS-WEB-API 常说的JS（浏览器执行的JS）包含两部分：
JS基础知识：ECMA 262标准
JS-WEB-API：W3C标准
W3C标准中关于JS的规定有：
DOM操作
BOM操作
事件绑定
Ajax请求（包括HTTP协议）
存储

 javascript 有三部分构成，ECMAScript，DOM和BOM，根据宿主（浏览器）的不同，具体的表现形式也不尽相同，ie和其他的浏览器风格迥异。

1. DOM 是 W3C 的标准； [所有浏览器公共遵守的标准]
2. BOM 是 各个浏览器厂商根据 DOM在各自浏览器上的实现;[表现为不同浏览器定义有差别,实现方式不同]
3. window 是 BOM 对象，而非 js 对象；

DOM（文档对象模型）是 HTML 和 XML 的应用程序接口（API）。

BOM 主要处理浏览器窗口和框架，不过通常浏览器特定的 JavaScript 扩展都被看做 BOM 的一部分。这些扩展包括：

弹出新的浏览器窗口 移动、关闭浏览器窗口以及调整窗口大小 提供 Web 浏览器详细信息的定位对象 提供用户屏幕分辨率详细信息的屏幕对象 对 cookie 的支持 IE 扩展了 BOM，加入了 ActiveXObject 类，可以通过 JavaScript 实例化 ActiveX 对象

javacsript是通过访问BOM（Browser Object Model）对象来访问、控制、修改客户端(浏览器)，由于BOM的window包含了document，window对象的属性和方法是直接可以使用而且被感知的，因此可以直接使用window对象的document属性，通过document属性就可以访问、检索、修改XHTML文档内容与结构。因为document对象又是DOM（Document Object Model）模型的根节点。可以说，BOM包含了DOM(对象)，浏览器提供出来给予访问的是BOM对象，从BOM对象再访问到DOM对象，从而js可以操作浏览器以及浏览器读取到的文档。其中
DOM包含：window

Window对象包含属性：document、location、navigator、screen、history、frames Document根节点包含子节点：forms、location、anchors、images、links

从window.document已然可以看出，DOM的最根本的对象是BOM的window对象的子对象。

区别：DOM描述了处理网页内容的方法和接口，BOM描述了与浏览器进行交互的方法和接口。


1、 API 介绍
API（Application Programming Interface,应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

任何开发语言都可以有自己的API
API的特征输入和输出(I/O)
API的使用方法
2、 Web API 接口的概念
浏览器提供的一套操作浏览器功能和页面元素的API(BOM和DOM)

此处的Web API特指浏览器给JS提供的API(一组方法)，Web API在后面的课程中有其它含义

浏览器的API一共提供了三种类型；

分别是 浏览器操控类(BOM)、页面文档操控类(DOM)、网络控制类；

但实际上，浏览器提供的API并不只有这三类，而是有很多类：

文档对象模型、设备API、通信API、数据管理API、特权API、已认证应用程序的私有API；

二、 文档对象模型 (DOM)
1、 基本概念
DOM是JavaScript操作网页的接口，全称为“文档对象模型”(Document Object Model)。 它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作(增删改查)。

浏览器会根据DOM模型，将结构化文档(比如HTML和XML)解析成一系列的节点， 再由这些节点组成一个树状结构(DOM Tree)。 所有的节点和最终的树状结构，都有规范的对外接口。

JavaScript是一门编程语言，而DOM是浏览器对HTML文档结构化后的一个模型;

严格地说，DOM不属于JavaScript，但是我们最常用的就是使用JavaScript操作DOM;

2、 节点的概念
DOM的最小组成单位叫做节点(node)。文档的树形结构(DOM树)，就是由各种不同类型的节点组成。

每个节点都可以看作是文档树的一片叶子。

最顶层的节点就是document节点，它代表了整个文档；是文档的根节点。

每张网页都有自己的document节点，window.document属性就指向这个节点的。

只要浏览器开始载入HTML文档，这个节点对象就存在了，可以直接调用。

每一个HTML标签元素，在DOM树上都会转化成一个Element节点对象;

文档里面最高一层一般是HTML标签，其他HTML标签节点都是它的下级。

除了根节点以外，其他节点对于周围的节点都存在三种关系:

父节点关系(parentNode):直接的那个上级节点 
子节点关系(childNodes):直接的下级节点 
同级节点关系(sibling):拥有同一个父节点的节点
