插件机制（Add-on Scheme）
===================================

### __addon.xml__

任何插件都必须包含此文件，内容是插件的唯一标识符参数，比如名称，描述，序列号等等。

可以在附录中找到[addon.xml](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/addon.xml.md)文件的完整例子。

在后台每当打开插件页面，系统就会扫描/addons/目录，并读取没有被安装的插件的addon.xml文件，并根据文件在列表中显示他们。

当插件被安装，除设置结构外，系统将会把插件的addon.xml的所有数据保存在数据库中，设置结构是每次从运行中的插件的文件中读取。

对于已安装的插件，当在插件页面修改插件时系统会调用addon.xml并只读取这个文件。

### 位置

文件必须位于插件的根目录 — /addons/[addon_name]/addon.xml

### Scheme 2.0

从CS-Cart3开始使用了新的addon.xml机制，采用旧插件机制开发的插件将不会被显示在Administration->Addons列表中，如果没有正确的迁移计划，旧的插件也不会正常工作。

### 结构

下面对addon.xml的结构进行完整描述，参考这种方案来开发你的插件。

### 主模块（TOP LEVEL）

* __addon__ 属性：
	* __@scheme__ - 插件方案版本，应使用“2.0”，插件方案“1.0”已被弃用。
	* __@edition_type__ - 一个可选属性可用于任何设置相关元素，它定义了版本中一个或另一个设置是可用的，如果为空将使用父元素的值，如果后者也没被设置，该值则被认为是ROOT。可用的值: PRO:ROOT, ULT:ROOT, ULT:VENDOR, MVE:ROOT, MVE: VENDOR, ROOT 以及 VENDOR
	* __id__ - 插件标识符。必须和所属分类名称一致。
	* __version__ - 插件版本。
	* __default_language__ - 插件本地化语言，可选参数，如果没有指定，语言将被认为是英语（EN）
	* __name__ - 插件在默认语言的名字
	* __description__ - 插件在默认语言中的描述
	* __priority__ - 优先级越高，插件越靠后被加载
	* __status__ - 设置插件被安装后的状态 (active/disabled)，默认为“disabled”

### 兼容模块（Compatibility block）

* __compatibility__ - 其他插件的兼容性描述
	* __dependencies__（依赖） - 列出安装此插件之前必须要安装的依赖插件列表，如果不满足将会显示错误信息。
	* __conflicts__（冲突） - 此参数列出的插件，将在当前插件安装之前被禁用并显示通知。

### 翻译模块（Translations block）

* __translations__ - 非默认语言的译文
	* __item__ - 一个译文项目，属性：
		* __@for__ - 指向翻译点，参数值：name/description/tooltip。可选参数，默认值为“name”
		* __@lang__ - 译文语言，如果没有则此项会被忽略。

### 设置模块

* __settings__ - 插件设置模块，可选。属性：
	* __@layout__ - 定义设置页面在哪里被开启（popup/separate）。可选属性，默认为“popup”
	* __@edition_type__ - 参考主模块中的__@edition_type__属性
* __sections__ - 插件设置页面的标签列表
	* __section__ - 设置标签，属性：
		* __@id__ - 文本标识符。可以通过 Registry::get('addons.[addon_id].[setting_id]') 访问。
		* __@edition_type__ - 参考主模块中的__@edition_type__属性
	* __name__ - 默认语言的标签名
	* __translations__ - 参考翻译模块的__translations__属性。
	* __items__ - 标签页的设置项列表
		* __item__ - 插件设置项。属性：
			* __@id__ - 设置标识符
			* __@edition_type__ - 参考主模块中的__@edition_type__属性
		* __type__ - 元素类型：input, textarea, password, checkbox, selectbox, multiple select, multiple checkboxes, countries list, states list, file, info, header, template
		* __name__ - 默认语言的设置名称。
		* __translations__ - 参考翻译模块
		* __tooltip__ - 提示信息
		* __default_value__ - 默认值，变量列表（多复选框，多下拉菜单）
		* __variants__ - Variants for the types selectbox, multiple select, multiple checkboxes, combo select
			* __item__ - Variant item.属性：
				* __@id__ - Variant标识符
			* __name__ - Variant名称
			* __translations__ - 和翻译模块类似，不同的是只使用"for"属性
			* __handler__ -  'info'类型设置处理函数。指定 函数的返回值将被作为文本输出。


### 语言变量模块

* __queries__ - 附加数据库请求
	* __item__ - 数据库请求项。属性：
		* __@for__ - 如果这个参数设置为“install”或者没有设置，会在插件安装时执行，如果设置为“uninstall”，会在插件卸载时执行。
		* __@editions__ - 逗号分隔版本号。如果这个属性设置值，请求会针对指定版本执行。

### 函数模块

* __functions__ - 在某一事件上调用用户定义函数：before_install —— 插件安装前；install —— 安装插件之后，但其模版，设置，语言在激活和清理缓存之前；uninstall —— 卸载之前。
* __item__ - 函数项。属性：
	* __@for__ - 函数的触发器，函数会在指定事件发生时被调用。可取的值：before_install, install, uninstall