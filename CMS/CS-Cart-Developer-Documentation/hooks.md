钩子
===================================

钩子是的一个概念，是核心代码中提供给外部代码的一个入口点。

利用这种技术，你可以在需要的地方为插件创建一个钩子，然后插件就可以在外部影响核心程序功能。

钩子的使用例子可以在[高级插件教程](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-addon-tutorial.md)中找到。

CS-Cart提供两种类型的钩子，源代码钩子（PHP hooks）和模版钩子（TPL hooks）。

你也可以在[维基百科](http://en.wikipedia.org/wiki/Hooking)中查阅关于钩子的概念。

## PHP钩子（PHP Hooks）

PHP Hooks 是程序中定义的一个特殊部分，可以控制插件。通过在需要的地方调用一个特殊函数来定义一个钩子：

```php
fn_set_hook('hook_name', $params, [$param2], [$paramN]);
```

钩子是非常灵活的技术，钩子函数可以拥有任意数量的参数。可以通过我们的官方网站的[Hook base](http://www.cs-cart.com/api)工具查看CS-Cart不同的版本中对PHP hooks的支持。

### PHP钩子(php hook)是什么样子

这里有一个在CS-Cart中使用php钩子的例子，他被声明为一个函数 __fn_get_gift_certificate_info__ ，用在[Gift certificates 插件](http://www.cs-cart.com/index.php?dispatch=hooks_base.manage&product_id=9#134393)中:

```php
fn_set_hook('get_gift_certificate_info', $_certificate, $certificate, $type);
```

### 什么时候使用PHP钩子

钩子提供了一个便捷的方式，可以在程序的执行过程中通过插件为系统增加额外功能。当一个钩子被调用时，钩子中定义的所有变量可直接被插件使用。

钩子函数的命名规则：以“fn_”为前缀，紧跟着是插件名称，然后跟一个下划线，最后是钩子函数全名：

```php
fn_gift_certificates_get_gift_certificate_info($_certificate, $certificate, $type);
```

根据函数名可以得知，此函数是 __Gift certificates__ 插件一个名为 __get_gift_certificate_info__ 的钩子。

### 如何使用PHP钩子

钩子创建顺序：

* 在插件的init.php中注册钩子

```php
...
 
fn_register_hooks(
	'get_category_data_pre'
);
 
...
```

* 在插件的func.php文件中创建一个函数用于实现某些功能

```php
if ( !defined('AREA') ) { 
	die('Access denied'); 
}
 
function fn_my_addon_get_category_data_pre($category_id, $field_list, $get_main_pair, $skip_company_condition, $lang_code)
{
	...
}
```

仅此而已。此时通过钩子插件已被发现，并且CS-Cart将会通过钩子执行插件。

在[高级插件教程](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-addon-tutorial.md)中有一个钩子使用的完整例子。

## TPL钩子（TPL Hooks）

TPL钩子被包含在模版标签中。

```html
{hook name="section:hook_name"}
    ...
{/hook}
```

可以补充或完全重新定义任何插件。

### 在模版中TPL钩子是什么样子

这是文件 __skins/basic/admin/views/order_management/totals.tpl__ 中的一个模版钩子例子：

```html
{hook name="order_management:product_info"}
	{if $cp.product_code}
		<p>{$lang.sku}:&nbsp;{$cp.product_code}</p>
	{/if}
{/hook}
```

### TPL钩子作用

TPL钩子在模版中用来显示附件数据，比如，如果插件收集了一些数据，需要显示在商店后台的一个独立模块中，这个模块就需要添加使用一个TPL钩子。[高级插件教程](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-addon-tutorial.md)中描述了这样一个使用例子。

### 如何使用TPL钩子

和PHP钩子不同，TPL钩子不需要明确定义。只需一个正确地命名即可实现其功能。

命名规则如下：

> __skins/[skin name]/[admin|customer]/addons/[addon id]/hooks/[template name]/[hook name].[pre|post].tpl__

同样，[高级插件教程](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-addon-tutorial.md)中有这样的例子。
