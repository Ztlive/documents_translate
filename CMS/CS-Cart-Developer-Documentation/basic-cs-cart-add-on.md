CS-Cart插件基础篇
===================================

Contents
* Required skills and tools
* Basic Concept. Primitive Add-on
* Hello World!
* Basic Add-on Making

注意：本插件教程只针对CS-Cart 3，本教程解决方案开发的插件将不会工作在老版本的CS-Cart

### 需要的技术和工具

你需要一些基础的PHP和Javascript知识，以及能够理解XML和常规软件结构，你可以在[工具和信息源](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/tools-and-information-sources.md)找到一些有用的信息。

你可以使用你喜欢的编辑器来做。

### 原始插件的基本概念

每个插件需要在各自的文件夹内（[addons]），整个插件的描述在文件addon.xml中。

文件中的信息分别是插件的id, 优先级, 版本, 名称, 等等。（参考[addon.xml](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/addon.xml.md)章节）.