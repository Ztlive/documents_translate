AJAX
===================================

Ajax技术用于在不刷新页面的情况下更新页面数据。CS-Cart大量使用了这种技术。

## AJAX在CS-Cart中的概念

CS-Cart基于jQuery的ajax模块做了一些扩展，可查阅/js/ajax.js文件。下面有一些关于这部分的方法：

* ajaxSubmit - 提取表单数据并准备提交。
* ajaxRequest - 发送AJAX请求到相应的控制器，使用jquery的ajax方法。
* ajaxResponse - 基于上面的ajaxRequest执行的前提下，负责服务器响应和结果显示。

当服务初始化处理ajax请求，ajax请求处理系统通过fn_init_ajax(/core/fn.init.php)方法执行后：一个AJAX类对象（/core/class.ajax.php）被创建；它执行必要的转换之前，并且在AJAX请求并生成JSON数组之后，进一步转移到ajaxResponse。

在PHP代码中将常量 AJAX_REQUEST 设置为true。

在代码中使用常量 AJAX_REQUEST 能够限制程序只能被ajax请求执行。

例子：

```php
if (defined('AJAX_REQUEST')) { 
	fn_set_notification('E', fn_get_lang_var('warning'), $msg, true, 'insecure_password'); 
}
```

当AJAX脚本执行完成，或者当终止函数exit();被调用，AJAX类 (/core/class.ajax.php) 的析构函数会被执行，析构函数将以JSON格式返回数组，并通过ajaxResponse方法输出。

控制器以下面的格式输出结果：

data:
	notifications:
		id:
			text
			title
		id:
			text
			title
			...
	html:
		block_id:
			block_content
		block_id:
			block_content
		...

## 微格式调用AJAX：

AJAX请求可以通过链接和表单发送，这两种情况，微格式cm-ajax应指定各自的元素：

### 表单

如果表单包含cm-ajax class，表单内容将会通过AJAX方式请求到表单的action属性指定的地址。

这样的话表单也应包含一个名为result_ids的隐藏表单元素，它包含被更新的AJAX数据的元素标识符。

这有一个AJAX表单的例子：

```html
<!-- form元素有一个名为cm-ajax的class，并定义了AJAX请求会发送到index.php -->
<form class="cm-ajax" action="index.php" method="post" name="product_form_817">
 
<!-- 隐藏表单元素用于存储HTML元素被更新的标识符 -->
	<input type="hidden" name="result_ids" value="my_id" />
	...
 
<!-- 当提交按钮被点击后，AJAX请求被发送到index.php -->
	<input id="button_cart_817" type="submit" name="dispatch[checkout.add..817]" value="Add to cart" />
	...
</form>
```

### 链接

链接的class属性定义了cm-ajax，当链接被点击后会发起一个AJAX请求到链接属性href指定的地址中。

应指定rev属性以表明ids元素被更新。

这有一个AJAX链接的列子：

```html
<a class="cm-ajax" rev="content_usergroups" href="{"profiles.request_usergroup?usergroup_id=`$usergroup.usergroup_id`&amp;status=`$ug_status`"|fn_url}">{$_link_text}</a>
```

### 额外微格式

微格式cm-ajax寄宿在链接和表单上，但是有很多可选的微格式,给开发者在CS-Cart中更多的控制AJAX，你可以在[微格式列表](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/microformats-list.md)中找到更详细的列表。


## 立即使用ajaxRequest

[编码规范](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/coding-standards.md)中提到不推荐直接在HTML中内嵌调用，所有的Javascript方法应放置到独立的文件中，并且通过CSS选择器关联HTML元素（id,class等等），虽然AJAX能够通过ajaxRequest方法直接发送内嵌调用：

```html
<input id="enable_block_1" type="checkbox" name="enable_block_1" value="Y" onclick="jQuery.ajaxRequest('{$index_script}?dispatch=block_manager.enable_disable&amp;block_id=1&amp;enable=' + (this.checked ? this.value : 'N'), {literal}{method: 'POST', cache: false}{/literal});" />

```