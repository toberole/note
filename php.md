mac开启Apache服务  
    sudo apachectl start  
mac停止Apache服务
    sudo apachectl stop
mac重启Apache服务
    sudo apachectl restart

查看Apache版本号  
    sudo apachectl -v

查看php版本
    php -v


Apache服务器 与 PHP
    Apache服务器接收到客户端的php请求会调用PHP模块，PHP模块处理完用户请求 将数据给Apache返回给客户端。php模块相当于Apache服务器的插件扩展。
Apache 与 Tomcat
    一般用Apache处理用户的静态请求，比如html、js；用Tomcat处理用户的动态请求，比如javaee servlet