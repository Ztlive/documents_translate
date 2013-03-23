通知
===================================

在CS-Cart 2.0版本中，通知的表现形式是在屏幕的右上角弹出一个矩形弹框。

通知有三中类型：

* 提示（type 'N'）
* 错误（type 'E'）
* 警告（type 'W'）

每一种类型都有不同的颜色：通知——绿色；错误——红色；警告——橙色。视觉样式定义在CSS文件中的通知区域。

可以通过fn_set_notification (/core/fn.common.php)函数创建一个通知。

在每个页面的HTML中增加一个空容器，当通知被定义后将会显示在这个容器中，通过下面这个模版创建一个通知：

> __/[zone_name]/common_templates/notification.tpl__

## 设置通知的方法：

通过两个方式创建通知：

* 执行标准脚本，显示到页面前面。
* 页面加载之后，通过一个特定的事件使用AJAX请求。

第一种，当页面加载完成后，通知会被创建并显示出来。
第二种，在需要时通过特定事件，无需刷新页面的情况下显示通知。

让我们仔细看一下第一种的创建方式：

1.在控制器代码中调用设定函数，例如：

```php
if ($mode == 'add_to_cart') {
    fn_set_notification('E', fn_get_lang_var('warning'), $msg, true, 'insecure_password'); 
}
```

这个函数添加一个新的元素到通知数组。

2.模版引擎处理模版 __/[zone_name]/common_templates/notification.tpl__ 并通过foreach数组遍历显示函数fn_set_notifications返回的列表中的所有通知

3.如果通知数组不是空的，那么HTML元素notification就会被显示。

第二种情况：

1.页面加载的过程中，模版引擎处理通知模版 __/[zone_name]/common_templates/notification.tpl__并为通知创建一个HTML容器（<div class="cm-notification-container">），这个容器可以是空的，也可以像第一种方式那样设置。

2.当控制器代码执行AJAX请求时，通知设置函数也会被调用：

```php
if (defined('AJAX_REQUEST')) {
    fn_set_notification('E', fn_get_lang_var('warning'), $msg, true, 'insecure_password'); 
}
```

这个函数添加一个新的元素到通知数组。

3.当脚本执行完成或者终止函数exit();被显示调用，就会触发AJAX类（/core/class.ajax.php）中的析构函数，析构函数将以JSON格式传递通知数组并通过JS脚本中的ajaxResponse方法(/js/ajax.js)传递数据。

4.然后ajaxResponse将会调用showNotification方法（/js/core.js），并向其传递所有已定义的AJAX通知数组，在showNotification函数中通过notification.append方法显示通知数据，它会添加HTML