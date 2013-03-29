CS-Cart插件高级进阶篇
===================================

本教程演示一些通过钩子和后置控制器写一些插件的进阶技术。

钩子是一个非常强大的技术，我们在CS-Cart中大量使用了它。PHP钩子可以用来执行额外的前置以及后置处理的数据，或者重写并覆盖默认行为。TPL钩子则是用来处理数据渲染等等，显示额外数据不需要修改原始模版。

后置控制器（包括前置控制器）是插件中的一个PHP文件，在一个或另一个标准控制器之前（或之后）执行（取决于他们在插件的文件结构中的名称和位置）。

在本教程中我们将创建一个插件用来演示PHP钩子和TPL钩子的用法。

该插件将收集关于用户浏览分类的数据并将其存储在数据库中。这些数据将会在后台管理主页通过表格显示用户以及对应分类的浏览量。

### 要求

完成本教程你需要先安装一个CS-Cart程序，你可以到我们的官方网站下载[30天试用](https://www.cs-cart.com/trial.html)版本。

需要掌握一些PHP的基础知识，以及Smarty和CS-Cart插件结构。推荐先阅读[插件目录结构](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-folder-structure.md)和[CS-Cart插件基础篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/basic-cs-cart-add-on.md)章节。


### 插件初始化：addon.xml, init.php

打开你安装的CS-Cart程序根目录并进入addon文件夹，然后创建一个名为advanced_addon的目录，然后在这个目录中创建addon.xml文件并写入以下内容（[下载](http://docs.cs-cart.com/index.php?dispatch=docs.getfile&attachment_id=87826)）

> addons/advanced_addon/addon.xml

```xml
<?xml version="1.0"?>
<addon scheme="2.0">
	<id>advanced_addon</id>
	<name>Advanced Add-on</name>
	<description>Advanced Add-on</description>
	<version>1.0</version>
	<priority>100500</priority>
	<status>active</status>
 
	<!-- 
		Installation and uninstallation routines:
		initialize a database table to store data 
		and delete it on add-on uninstallation
	-->
	<queries>
		<item for="install">DROP TABLE IF EXISTS ?:test_addon_data;</item>
		<item for="install">
			CREATE TABLE `?:test_addon_data` (  
				`user_id` int(11) unsigned NOT NULL DEFAULT 0,  
				`categories` text NOT NULL DEFAULT '',
				PRIMARY KEY (`user_id`)
			) Engine=MyISAM DEFAULT CHARSET UTF8;
		</item>
		<item for="uninstall">DROP TABLE IF EXISTS ?:test_addon_data;</item>
	</queries>
</addon>
```
addon.xml文件结构可参考[插件机制](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-scheme.md)章节，并且可以在[addon.xml](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/addon.xml.md)章节找到一个addon.xml文件的完整注释列子。

在相同的目录创建init.php文件并写入如下内容：

> addons/advanced_addon/init.php

```php
<?php

if ( !defined('AREA') ) { die('Access denied'); }
 
fn_register_hooks(
	'get_category_data_pre'
);
 
?>
```

> CS-Cart的函数名称通常是显而易见的（例如，'get_products'是获取产品的），钩子的命名通常在函数名后追加'_pre' 和 '_post'来定义前置或后置操作（例如，'get_products_pre' 和 'get_products_post'）。

在这个文件中我们定义了连接钩子 __get_category_data_pre__，它将在获取分类数据之前被调用，你可以在[钩子基础](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/hook-base.md)章节找到关于这部分的信息。

### 获取数据：func.php

在插件目录（__addons/advanced_addon__）创建一个func.php文件，它将包含实际的函数嵌入钩子（[下载](http://docs.cs-cart.com/index.php?dispatch=docs.getfile&attachment_id=87828)）：

> addons/advanced_addon/func.php

```php
<?php
 
if ( !defined('AREA') ) { die('Access denied'); }
 
function fn_advanced_addon_get_category_data_pre($category_id, $field_list, $get_main_pair, $skip_company_condition, $lang_code)
{
	//获取认证数据来识别用户
	$auth = $_SESSION['auth'];
 
	//验证用户已经登录并且正在浏览前台页面
	if (!empty($auth['user_id']) && AREA == 'C') {
		//验证该用户在数据库中是否有数据.
		//如果不存在则创建新的记录, 如果存在则更新现有数据.
 
		$viewed_categories = db_get_field('SELECT categories FROM ?:test_addon_data WHERE user_id = ?i', $auth['user_id']);
 
		if (!empty($viewed_categories)) {
			$viewed_categories = unserialize($viewed_categories);
		}
 
		$viewed_categories[$category_id] = true;
		$viewed_categories = serialize($viewed_categories);
 
		db_query('REPLACE INTO ?:test_addon_data VALUES (?i, ?s)', $auth['user_id'], $viewed_categories);
	}
}

?>
```

函数 __fn_advanced_addon_get_category_data_pre__ 将获取当前浏览的分类，并和当前用户关联起来存入数据库。

> 这是需要遵循的命名规则：__'fn_' + [addon id] + '_' + [hook name]__.其他的命名规则将会被忽略。

### 在后台显示数据：extra.tpl, index.post.php

为了在后台展示数据，我们需要在后台首页模版(__skins/basic/admin/views/index.tpl__)中使用TPL钩子 __extra__。

打开目录 __skins/basic/admin/addons__ 并创建名为 __advanced_addon__ 的目录，并创建一个名为 __hooks__的子目录，并且在 __hooks__ 目录中再创建一个名为 __index__ 的子目录。在这个目录中创建一个名为 __extra.pre.tpl__ 的文件并写入如下内容（[下载](http://docs.cs-cart.com/index.php?dispatch=docs.getfile&attachment_id=87839)）：

> skins/basic/admin/addons/advanced_addon/hooks/index/extra.pre.tpl

```html
<div class="statistics-box">
	<div class="statistics-body">
		<p class="strong">Viewed categories</p>
		<div class="clear">
			{if $viewed_categories}
				<ul>
					{foreach from=$viewed_categories item="category_data"}
						<li><strong><a href="{"profiles.update?user_id=`$category_data.user_id`"|fn_url}">{$category_data.user_name}</a></strong>:&nbsp;
							{foreach from=$category_data.categories key="category_id" item="category_name"}
								<a href="{"categories.update?category_id=`$category_id`"|fn_url}">{$category_name}</a>, 
							{/foreach}
						</li>
					{/foreach}
				</ul>
			{else}
				<ul>
					<li>No data found</li>
				</ul>
			{/if}
		</div>
	</div>
</div>
```

> 和PHP钩子不一样，TPL钩子无需显示声明，只需将它写在指定目录的指定模版中，位置和命名约定如下：
> skins/[skin name]/[admin|customer]/addons/[addon id]/hooks/[template name]/[hook name].[pre|post].tpl.

模版不能直接从数据库读取数据，这个操作由index.php控制器的后置控制器来完成。

打开目录 __addons/advanced_addon__ 并创建子目录 __controllers/admin__，在这个目录中创建名为 __index.post.php__ 的文件，并写入如下内容（[下载](http://docs.cs-cart.com/index.php?dispatch=docs.getfile&attachment_id=87840)）

> addons/advanced_addon/controllers/admin/index.post.php

```php
<?php
 
if ( !defined('AREA') ) { die('Access denied'); }
 
$viewed_categories = db_get_array('SELECT * FROM ?:test_addon_data');
 
if (!empty($viewed_categories)) {
	foreach ($viewed_categories as $key => $category_data) {
		$category_data['user_name'] = fn_get_user_name($category_data['user_id']);
		$category_data['categories'] = unserialize($category_data['categories']);
		$category_data['categories']  = fn_get_category_name(array_keys($category_data['categories']));
 
		$viewed_categories[$key] = $category_data;
	}
 
	Registry::get('view')->assign('viewed_categories', $viewed_categories);
}
 
?>
```

> 仔细检查所有文件的路径，名称以及内容来确保插件能够正常运行。

### 结果

为了能看到插件的行为，你必须先要安装它，打开后台（通常是 [your_domain]/admin.php）并进入Administration -> Add-ons，找到 __Advanced Add-on__ 项并点击install，你应该能看到安装成功的消息。

打开后台主页并滚动到最新订单区域，你应该能看到类似这样的内容：

![viewed-categories-empty](http://www.cs-cart.com/images/docs_nodes/5/viewed-categories-empty.png)

如你所见，目前还没有数据，不过这部分显示是正确地。

打开前台，在店铺页面中随便浏览，随机打开一些分类，你也可以尝试使用不同的账号浏览。

刷新后台首页，检查“查看分类”部分的状态：

![viewed-categories-data](http://www.cs-cart.com/images/docs_nodes/5/viewed-categories-data.png)

这里应该会显示你刚刚浏览器过的分类数据。