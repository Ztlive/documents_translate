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