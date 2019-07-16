Android Studio 创建一个项目的时候，rootProject下面会生成gradle.properties和local.properties文件。其中，gradle.properties中的内容不需要显示调用就可以直接在build.gradle中进行使用。在同一个项目中，build.gradle可以直接获取其同级或者其父级(父级也要有build.gradle)的properties文件

addr2line查看native异常：
arm-linux-androideabi-addr2line -cfe $symbol_file 0xyyyyyyyy
其中，&symbol是对应库作为的文件，而0xyyyy则是需要转换的地址















