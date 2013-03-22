CS-Cart 开发者文档
===================================
### Author:Jason-wong
### 项目地址:https://github.com/jason-wong/documents_translate
### 原文地址：http://docs.cs-cart.com/



本资源主要针对那些想要补充或扩展CS-Cart功能的人。它涵盖了不同方面，包括编程、清晰明确的体系结构以及各种标准，会为你在自定义CS-Cart和开发插件上提供很大的帮助。所有文档均已按主题分组，所以你能很容易找到所需信息。编码规则和书写格式也会在这里指出。


## 引言
* [CS-Cart 结构和接口概述](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/cs-cart-architecture-api-overview.md) - 简短介绍CS-Cart的结构体系和应用程序接口。
* [应用技术](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/techologies-used.md) - 概述CS-Cart背后的技术


## 通用方法
* [初始化](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/initialization.md) - 初始化步骤描述
* [控制器](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/controllers.md) - 控制器结构和功能细节
	* [前置和后置控制器（Precontrollers and Postcontrollers）](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/precontrollers-and-postcontrollers.md) - 前置和后置控制器的概念和使用
* [数据库](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/database.md) - 数据库请求中的占位符使用
	* [链接远程数据库](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/connect-additional-databases.md) 怎样链接和使用远程数据库
* [钩子](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/hooks.md) - 钩子的描述和例子
* [微格式](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/microformats.md) - 微格式的描述和结构
* [预定义样式](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/special-classes.md) - 预定义样式用于节省CSS文件大小
* [AJAX](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/ajax.md) - 一个创建AJAX请求的例子
* [通知](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/notifications.md) - 弹窗通知上的信息
* [CS-Cart 3.1.x的结构变化](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/architecture-changes-in-cs-cart-3.1.x.md) - 介绍CS-Cart3.1.x的结构变化

## 插件
* [插件文件夹结构](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-folder-structure.md) - 介绍插件目录结构
* [插件机制](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-scheme.md) - 插件机制的描述
* [插件连接](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-connection.md) - 解释说明如何利用钩子连接插件
* [2.x版本的插件迁移到3.x](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/adapting-add-on-from-2.x-to-3.md) - CS-Cart 3插件迁移指南
* [教程](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/tutorials.md)
	* [CS-Cart插件基础篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/basic-cs-cart-add-on.md) - 学习如何编写一个CS-Cart插件
	* [CS-Cart插件高级进阶篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-cs-cart-add-on.md) - 插件高级篇：PHP钩子，TPL钩子，Post控制器
	* [插件协议](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/add-on-licensing.md) - 插件协议说明
	* [旗舰版店铺插件协议](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/storefront-based-add-on-licensing-in-ultimate-edition.md) - CS-Cart旗舰版店铺拥有独立的插件协议

## 设计和布局
* [块管理](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/block-manager.md) - 块机制和类型说明
* [My Changes插件](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/my-changes-add-on.md) - 使用My Changes插件
* [布局例子](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/layouting-example.md) - 一个简短的例子，创建一个类似Amazon.com的页面布局
* [设计变更](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/design-changes.md) - CS-Cart设计系统基础知识及轻微设计变更指南
* [店铺设计模版方案](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/storefront-design-template-scheme.md) - 店铺模版通用方法

## 附录
* [编码规范](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/coding-standards.md)
	* [代码规范](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/programming-standards.md) - CS-Cart基本的代码规范
	* [对象选择标准](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/object-selection-standards.md) - 搜索标准
	* [头部和文本格式](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/heading-and-text-formats.md) - 头部和文本格式的通用提示
	* [addon.xml](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/addon.xml.md) - addon.xml完整的注释例子
	* [基础钩子](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/hook-base.md) - 完整的钩子列表
	* [微格式列表](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/list-of-microformats.md) - CS-Cart微格式说明
	* [预定义样式列表](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/special-classes-list.md) - CS-Cart预定义样式说明
	* [设计变更列子](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/examples-of-design-changes.md) - 最常用的设计变更
		* [店铺背景](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/store-background.md)
		* [链接颜色](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/link-color.md)
		* [按钮](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/buttons.md)
		* [表单字段](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/form-fields.md)
		* [顶部菜单](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/top-menu.md)
		* [店铺宽度](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/storefront-width.md)
	* [工具和信息源](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/tools-and-information-sources.md)
		* [HTML基础](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/html-basics.md)
		* [CSS基础](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/css-basics.md)
		* [Smarty基础](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/smarty-basics.md)
		* [Smarty调试控制台](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/smarty-debug-console.md)