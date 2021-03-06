npm 安装模块
npm install ModuleName

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。
var express = require('express');

全局安装与本地安装
npm 的包安装分为本地安装（local）、全局安装（global）两种

npm install express          # 本地安装
npm install express -g   # 全局安装

本地安装
1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
2. 可以通过 require() 来引入本地安装的包。
全局安装
1. 将安装包放在 /usr/local 下或者 node 的安装目录。
2. 可以直接在命令行里使用。
如果希望具备两者功能，则需要在两个地方安装它或使用 npm link。

查看安装信息
查看所有全局安装的模块： npm list -g

Package.json 属性说明
name - 包名。
version - 包的版本号。
description - 包的描述。
homepage - 包的官网 url 。
author - 包的作者姓名。
contributors - 包的其他贡献者姓名。
dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
keywords - 关键字

卸载模块
    npm uninstall express
    卸载后，你可以到 /node_modules/ 目录下查看包是否还存在
    或者使用以下命令查看：npm ls
更新模块
    npm update express
搜索模块
    npm search express