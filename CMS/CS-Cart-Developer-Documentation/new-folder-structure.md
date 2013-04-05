新目录结构
===================================

* __app__ - 商城的核心
	> 需要改动核心功能时应通过插件来影响而不是直接修改核心程序。
	* __addons__ - 商城插件，每个插件除了JS文件外，所有文件都应放置在其各自的目录中。
		* __sample_addon__ - 插件示例。目录结构和 __/app__ 目录类似。
			* __controllers__ - 前置或后置控制器，以及重写某个控制器。
			* __database__ - 数据库文件（将来会转移到var目录）
			* __schemas__ - 方案扩展
			* __templates__ - 模版以及模版扩展
			* __Tygh__ - 核心命名空间扩展和重写（重写核心类应谨慎！）
			* __SampleAddon__ - 插件命名空间类 'SampleAddon'
			* __lib__ - 附加库
	* __controllers__
		* __backend__ - 后台控制器
		* __common__ - 前后台公共的控制器
		* __frontend__ - 前台控制器
	* __functions__
	* __payments__
	* __schemas__
	* __shippings__
	* __Tygh__
	* __lib__
* __design__
	* __backend__
		* __css__
		* __mail__ - 后台email模版和媒体
		* __media__ - 静态数据：图片，字体等等
		* __templates__ - 后台模版
	* __frontend__
		* __mail__ - 前台email模版和媒体
		* __skins__
			* __basic__
				* __css__
				* __media__
				* __templates__ - 前台模版
* __images__
* __install__ - 安装程序
* __js__
	* __addons__
		* __sample_addon__ - 'sample_addon'插件的JS文件
		* __lib__
		* __Tygh__
* __var__
