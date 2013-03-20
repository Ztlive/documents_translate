CS-Cart 结构和接口概述
===================================
CS-Cart 遵循模块化架构体系：这是负责基本功能的核心部分，许多插件都使用API工具扩展它。
> __强烈建议不要直接对核心函数，控制器，模版以及方案进行修改，尽量使用API提供的方法去改变默认的软件行为。__

下面列出了在CS-Cart中影响默认行为的API方法的基本组成和简短描述。

## 核心函数
CS-Cart的核心函数存储在core目录。这些函数实现了软件的所有核心功能，并且根据不同的目的被分组到不同的文件中：fn.catalog.php, fn.cart.php 等等。

PHP钩子用来扩展并重写默认行为或核心函数。在[钩子](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/hooks.md)篇可以看到更多关于PHP钩子的内容。关于钩子的实际使用请参阅[CS-Cart插件高级进阶篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-cs-cart-add-on.md)教程。

## 方案
方案是一个特殊的文件，用来描述一些对象的结构，比如块、设置、促销等等。所有的方案均被存储在schemas目录。插件可以扩展和局部（或全部）覆盖原有方案，通过[扩展方案](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/extend-schemes.md)章节来学习如何操作。

## 控制器
遵循MVC模式，CS-Cart使用控制器进行数据操作。

所有默认的控制器存储在controllers目录，插件控制器存储在插件目录——addons/[add-on_name]。可以通过[控制器](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/controllers.md)章节查阅到更多信息。

为了影响控制器的默认行为，可以使用前置和Post控制器。可以到[前置控制器和Post控制器（Precontrollers and Postcontrollers）](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/precontrollers-and-postcontrollers.md) 章节学习使用这种技术，并且Postcontrollers的实际使用在[CS-Cart插件高级进阶篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-cs-cart-add-on.md)教程中。

## 模版
CS-Cart使用Smarty来渲染数据，关于Smarty的基本信息可以参阅[Smarty基础](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/smarty-basics.md)。CS-Cart将前后台所有的主题模版存储在skins目录。

可以使用TPL钩子扩展模版，关于模版钩子的实际使用可以参阅[CS-Cart插件高级进阶篇](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/advanced-cs-cart-add-on.md)教程。