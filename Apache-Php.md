
# non-thread-safe 非线程安全 与IIS 搭配环境
# thread-safe 线程安全 与apache 搭配的 环境


Define SRVROOT "D:/Apache24"

# 若你的80端口被占用（可在cmd下用命令netstat -a查看。也可以是netstat -ano），
# 可以选择让apache监听其他端口（比如：8080）或者终止占用80端口的进程。
Listen 80
# 开启重写模块，忘记开启会导致laravel项目除了主页其他全是404
LoadModule rewrite_module modules/mod_rewrite.so

LoadModule vhost_alias_module modules/mod_vhost_alias.so

# 追加如下内容
LoadModule php7_module "D:/Program Files/php-7.1.4-Win32-VC14-x64/php7apache2_4.dll"
PHPIniDir "D:/Program Files/php-7.1.4-Win32-VC14-x64"
AddType application/x-httpd-php .php .html .htm

<IfModule dir_module>
　　irectoryIndex index.php index.html
</IfModule>

3、安装Apache到服务

　　以管理员方式启动DOS窗口，输入：

"D:\Program Files\httpd-2.4.23-x64-vc14-r3\Apache24\bin\httpd.exe" -k install -n apache
切记，包含引号。该命令的意思是，安装apache服务，并将该服务名称命名为apache

删除服务：sc delete "apache"

4、启动服务
net start apache

5、关闭服务
net stop apache

Apache 配置虚拟目录和虚拟主机
# Create Virtual catalogue
<IfModule dir_module>
    DirectoryIndex index.html index.htm index.php
    Alias /MyWeb "c:/MyWeb" // 在C盘的根目录下有一个Myweb文件夹，可以把这个文件夹看做虚拟目录
    <Directory c:/MyWeb>
        Order allow,deny
        Allow from all
    </Directory>
</IfModule>



#配置自己的虚拟主机
<VirtualHost 127.0.0.1:80>
    DocumentRoot "c:/MyWeb"
    DirectoryIndex index.html index.htm index.php
    <Directory />
    Options FollowSymLinks
    #Not allow others to correct our page
    AllowOverride None
    Order allow,deny
    Allow from all
    </Directory>
</VirtualHost>