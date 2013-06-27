### 浅释Rewrite和Redirect ###

Rewrite和Redirect是两种常见的配置文件级别的URL重定向解决方案，另外，在脚本文件中也能通过相关函数实现URL重定向，比如：在PHP中，可以使用header()函数向浏览器发送HTTP header，实现URL重定向。

    <?php

    header('HTTP/1.1 301 Moved Permanently');
    header('Location: relative path of target file')
    exit;

这种处理虽然简单，但不利于维护，应尽量避免使用。

#### 一，Rewrite ####

##### 1，使用 #####

配置文件中使用，包括分布式配置文件.htaccess，Apache配置文件httpd.conf和虚拟主机配置文件httpd-vhosts.conf。其中，httpd.conf最先被加载，其次是httpd-vhosts.conf，这两个文件在服务器启动时，加载一次，之后不再重复加载，除非重启服务器。而.htaccess文件在每次应用被请求时，都会被加载一次。后面加载的文件配置规则会覆盖前面的。

##### 2，场景 #####

对SEO不友好的URL进行重定向。比如：动态脚本带一堆参数的情况（http://yourdomain/yii.php?r=news/index/id/1）。以及将一些无预期的URL重定向到指定路径，从而起到在高层一定程度上预防攻击的作用。

##### 3，特点 #####

优点是控制灵活，功能强大，即便复杂的URL也可以通过正则表达式进行匹配，从而重定向。缺点是对正则表达式的要求较高，代码不易读。

##### 4，示例 #####

    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteBase /
        RewriteCond %{REQUEST_FILENAME} !-f	# not file
        RewriteCond %{REQUEST_FILENAME} !-d	# not dir
        RewriteRule . /index.php [L]
    </IfModule>

将这段代码贴在三个配置文件中的任何一个，注意：改变.htaccess不需要重启服务器，而更新另外两个配置文件，则需要重启服务器才能使改动生效。这样设置后，当用户访问无效文件或目录时，就会重定向到index.php文件。

#### 二，Redirect ####

##### 1，使用 #####

分布式配置文件.htaccess和脚本文件使用。

##### 2，场景 #####

URL发生改变时，将旧的URL重定向到新的URL（如：从搜索引擎点过来）。常见的Redirect为301，就是永久重定向。

##### 3，特点 #####

优点是规则简单，代码易读，缺点是不能使用正则表达式对相同模式的URL进行匹配，因而只能对少数特定URL进行重定向。

##### 4，示例 #####

    Redirect permanent /index.php /test.php

将这行代码贴在.htaccess中，当用户访问index.php文件时，就会重定向到test.php文件。

#### 三，Tips ####

有一点需要说明的是，一般情况下，Rewrite实现的URL重定向URL是不会改变的（p.s. 通过在RewriteRule中定义Redirect如[R=301]，可以改变URL），但通过Redirect实现的URL重定向URL会改变为重定向规则定义的目标URL。

（完）
