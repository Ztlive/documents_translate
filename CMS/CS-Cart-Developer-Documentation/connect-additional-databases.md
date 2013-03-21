连接远程数据库
===================================

有时我们需要连接远程数据库用于扩展我们的数据、同步、备份或者其他目的。CS-Cart能够另开发者方便的设置不同于默认数据库连接的其他连接方式，这在核心程序中已经提供支持。

* __db_initiate__ 使用这个函数来设定连接远程服务器上的数据库。

用法：

```php
db_initiate(<hostname>, <user>, <password>, <db_name>, <alias>, <table_prefix>);
```

参数：

* __hostname__ - 目标服务器（比如：localhost）
* __user__ - 目标数据库的连接账号
* __password__ - 目标数据库的连接密码
* __db_name__ - 目标数据库名
* __alias__ - 目标数据库映射到本地的别名
* __table_prefix__ - 目标数据库的表前缀，会被“?:”占位符所替代

例子：

```php
db_initiate('localhost', 'mysqluser', 'mysqlpassword', 'cscart_backup', 'bckp', 'cscart_');
$data = db_get_array("bckp#SELECT * FROM ?:products");
```

在上例中，会将localhost服务器上的cscart_backup数据库中的cscart_products表的内容赋值给变量$data.
