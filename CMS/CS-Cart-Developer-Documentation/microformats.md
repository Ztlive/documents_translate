微格式(Microformat)
===================================

微格式是一种特定CSS样式用于确定应该执行的JavaScript代码在一个指定的事件的html元素将被分配指定的微格式。

所有微格式的命名都应遵循如下规则：cm-new-microformat-name，前缀为cm-(CS-Cart microformat)，表明这个样式是一个微格式样式。和标准CSS样式不同的是，微格式的样式描述并不在CSS文件中，而是在 JavaScript 文件中。


在/js/目录中搜索处理某个微格式时，需要用.hasClass('cm-microformat-name')来检测

搜索JS代码，处理某个微格式，有必要用子字符串.hasClass('cm-microformat-name')搜索，在/js/目录中。

在[微格式列表](https://github.com/jason-wong/documents_translate/blob/master/CMS/CS-Cart-Developer-Documentation/microformats-list.md)中可以查阅CS-Cart所有的微格式列表。