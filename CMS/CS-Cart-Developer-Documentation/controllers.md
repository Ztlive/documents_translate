控制器

### 原文：http://docs.cs-cart.com/docs-controllers
===================================

> __此节内容来源于网络，作者未知。并对原翻译做了细节修改。__

## 控制器
软件运行的基本模式是调用两个主程序PHP文件(admin.php或index.php)其中之一，然后通过进一步连续执行其它的PHP文件来实现程序功能。

> __1. index.php or admin.php -> 2.prepare.php -> 3.init.php -> 4.[controller_name].php__

在CS-Cart规则中，上述第4项中被连接的文件被叫做控制器。程序通过这个文件实施数据处理—包括从数据库中提取必要的数据、数据处理、计算、转换等，并为进一步显示准备数据。

通过传递给主程序文件“dispatch”参数，程序可以自动定义到控制器的路径和它的名称。“dispatch”参数格式为 “[controller_name].[mode_name]“，其中 “[controller_name]”是控制器名称， “[mode_name]” 为所调用控制器的运行方法。调用的文件名将是”[controller_name].php”。

所有针对管理面板(执行文件 – admin.php)的控制器都位于“/controllers/admin”目录，针对客户区域 (index.php)的控制器都位于 “/controllers/customer”目录。如果调用的控制器不在这两个目录，系统将尝试从“controllers/common”目录读取，这个目录一般用来存放前后台公用的控制器。

控制器连接通过函数[/core/fn.control.php]/fn_dispatch()进行，该函数不接收任何参数，它主要有以下几点功能：

1.检查参数 “dispatch”有效性。
2.检查当前用户是否具有所调用控制器的权限。
3.如有必要，重定向至安全协议 (HTTPS) 。
4.为继续连接准备一个正确顺序的前置和后置控制器(来自插件和核心)清单。
5.自动定义一个显示用的模板。

示例1.

执行admin.php文件并设置参数"dispatch=products.manage"，在浏览器中输入："http://cscart_dir/admin.php?dispatch=products.manage"

连接控制器位于目录 - "/controllers/admin/products.php"。参数“manage”用来定义控制器对数据实施的动作。在这个例子里面，“manage”指示控制器将从数据库中选取一些产品并将其显示在后台产品页面。

注意！控制器名称必须唯一。如果一个插件定义的控制器名称和标准控制器名称相同，那么当调用其中任何一个控制器时将出错。


## 控制器结构

每个控制器都可以包含以下逻辑区块：
1.处理POST请求
2.处理GET请求
3.定义供控制器调用的局部函数

参数“dispatch”中的 “[mode_name]”部分是GET请求中设定的用来执行的操作方法。

### POST请求
处理POST请求总是优先于处理GET请求，从控制器传递到函数fn_dispatch()的控制区块末端总会有一个字符串。

```php
return array(CONTROLLER_STATUS_OK, "$index_script?dispatch=controller_name$suffix");
```

参数CONTROLLER_STATUS_OK包含了一个代表控制器运行成功状态的常量，第2个参数是POST请求后的重定向URI字符串。

例子1.
```php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
	if ($mode == 'add') {
    	// here comes the code which should be executed when submitting product addition form
	}
	return array(CONTROLLER_STATUS_OK, "$index_script?dispatch=products$suffix");
}
```

### GET请求
处理GET请求的区块总是放置于POST区块后面。

例子1.
```php
if ($mode == 'manage') {
   // here comes the code which should be executed when running "http://cscart_dir/admin.php?dispatch=products.manage"
}
```

这里给出一段检查参数“mode”(dispatch=controller.mode)值并当该值等于“manage”时运行特定代码的代码。这段代码将在GET请求时被执行。

在GET模式操作的结尾，控制器通常会执行下列关闭动作：

1.将控制传递到显示子系统或模板
2.通过GET方法重定向
3.完成控制器执行并中止任何行为：exit;

首先，控制将自动转移到fn_dispatch()函数，如果没有被明确指定，系统将继续执行第二和第三项动作。使用下面的方法将变量渲染到模版：

```php
$view->assign('template_var_name', $php_var_name);
```

这里 “template_var_name” 定义了在模板里能够识别的有效变量名称， “$php_var_name”定义了该变量的内容。

```php
if ($mode == 'manage') {
	$product_name = 'Product 1';
	$product_description = 'Product description';
	$view->assign('tpl_product_name', $product_name);
	$view->assign('tpl_product_description', $product_description);
}
```

控制器中这段代码执行后，系统开始解析模版，此时$tpl_product_name 和 $tpl_product_description变量已生效。

### 函数
控制器函数的定义按照通用的函数定义规则来定义。如果一个控制器函数需要被另外的控制器调用，那么这个函数就应该位于核心程序或插件代码里面。

## 可用的数据
在控制器中使用程序数据，你需要使用下列标准数组：

1.$_REQUEST – 包括来自GET和POSt请求的所有变量。该数组中所有变量都以一种特定方式处理：HTML标记被移除；PHP语言自动添加的斜线（如果相应设置启用）被删除。
2.$_SESSION – 储存SESSION数据的标准PHP数组。
3.$Registry – 一个可以在程序任何位置访问数据的特定静态类库。例如，程序配置参数在启动时读取，并被绑定到Registry类中。该类的特点是任何存储在里面的数据都可以缓存。通过数据缓存可以避免重复读取在数据库中很少更新的信息的请求。

## 传递控制至模板
控制器执行完毕后，控制将返回给函数fn_dispatch()，它将控制程序和路径传递给模板，它们将被模板生成器执行和显示。

注意，可以在控制器中使用中止脚本或者将页面重定向到另一个地址，以此来阻止解析模版的行为。

默认情况下模板路径将自动按以下格式定义：

> __/[customer|admin|mail]/views/[controller_name]/[mode_name].tpl__

例子1.
> http://cscart_dir/admin.php?dispatch=products.manage

被显示的模版路径为：/admin/views/products/manage.tpl