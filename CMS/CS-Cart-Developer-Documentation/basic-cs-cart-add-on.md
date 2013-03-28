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

这是插件能够运行的基本必要文件，虽然它没有做任何动作，但它关系着插件的安装和卸载，甚至一些配置和翻译。

每个插件都能通过定义优先级来决定插件的加载顺序。通常优先级可以设置为任意数字，插件会在内置插件加载之后被加载。

就这样，你可以继续下一步并创建你的第一个CS-Cart插件。


### Hello World!

首先，我们来写一个经典的Hello World。

打开CS-Cart安装目录中的 __[addons]__ 目录，并创建一个名为 __[hello_world]__ 的目录并打开，这里就是你的工作目录。

正如之前所说，所有插件都需要有一个用来操作的addon.xml文件。我们来创建这个文件。

首先你需要提供一些插件的基本数据：

* __id__ - (必须和插件目录名称一致, 在我们的例子中，应该是“hello_world”)
* __version__ - 设置为 1.0
* __name__ - 默认语言下插件的名称 (英语无需指定)
* __description__ - 插件默认语言的简介 (英语无需指定)
* __priority__ - 设置一些大数字 比如 100500

现在这个文件应该看起来是这样的：

```xml
<?xml version="1.0"?>
<addon scheme='2.0'>
  <id>hello_world</id>
  <version>1.0</version> 
  <name>Hello World!</name>
  <description>Say hello to the world</description>
  <priority>100500</priority>
</addon>
```

这已经足够让我们的插件运行起来了。下面进入网站后台，并打开Administration->Add-ons，插件应该会出现在列表中，你可以 安装/卸载 以及 启用/禁用它了:

![Hello World Add-on](http://www.cs-cart.com/images/docs_nodes/5/resized_hw1.png)

注意：如看不到插件列表变化，可能需要清空系统缓存。在浏览器URL地址末尾添加&cc

请注意scheme='2.0'这个参数，插件系统已经弃用了这个参数，它仅为了兼容性而出现在一些独立的插件中。

好了，让我们为插件添加一些设置，我们需要在addon.xml文件中添加一部分settings代码.其中包含一些子项，每一项都包含name，type，default_value等，如下所示：

```xml
<settings>
  	<sections>
    	<section id="general">
	      	<items>
	        	<item id="some_prop">
	          		<name>Enter any text</name>
	          		<type>input</type>
	          		<default_value>Hello World!</default_value>         
	        	</item>
	        	<item id="some_dropdown">
	          		<name>Pick a value from the list</name>
	          		<type>selectbox</type>
	          		<default_value>blue</default_value>
	            	<variants>
	              		<item id="red">
	                		<name>Red</name>                            
	              		</item>
	              		<item id="green">
	                		<name>Green</name>
	              		</item>
	              		<item id="blue">
	                		<name>Blue</name>
	              		</item>
	            	</variants>
	        	</item>
	      	</items>
    	</section>
  	</sections>
</settings>
```

把这些添加到addon.xml文件中<priority>100500</priority>标签之后。

回到插件列表页面并重新安装我们的插件。你可以看到Edit链接可以点击，点击后会显示一个包含你刚刚添加的这些设置的对话框。

这不是在造火箭，对吧？但是这个插件目前没什么用处。下一步我们要创建一个更复杂和有用的插件。

### 制作基础插件

现在你已经掌握了基本的CS-Cart插件的创建方式，下一步将创建一个简单但有用的插件。

为什么不为你的店铺添加一点社交氛围？让我们来创建一个插件，用来在一个模块中显示指定用户的Twitter消息。

#### addon.xml

让我们来为你的插件创建addon.xml文件并设置一个可以定义用户的设置。

创建一个名为my_twitter_addon的插件目录（或者其他任何名称，这取决于你）。在目录中创建一个和目录同名的XML文件，并添加以下代码：

```xml
<?xml version="1.0"?>
<addon scheme='2.0'>
    <id>my_twitter_addon</id>
    <version>1.0</version> 
    <name>My Twitter Add-on</name>
    <description>Show Twitter feed</description>
    <priority>100501</priority>
    <settings>
        <sections>
            <section id="general">
                <items>
                    <item id="username">
                        <name>Twitter username</name>
                        <type>input</type>
                    </item>
                    <item id="number_of_tweets">
                        <name>Number of tweets</name>
                        <type>selectbox</type>
                        <default_value>10</default_value>
                        <variants>
                            <item id="5">
                                <name>5</name>
                            </item>
                            <item id="10">
                                <name>10</name>
                            </item>
                            <item id="15">
                                <name>15</name>
                            </item>
                        </variants>
                    </item>
                </items>
            </section>
        </sections>
    </settings>
</addon>
```

你大概能看到为插件设置对话框定义了两个可编辑的属性：Twttier账号和显示的消息数。

插件应该出现在Administration->Add-ons页面了，如果没有看到请清除一下系统缓存（在URL末尾添加&cc）。

两个值必须是可编辑的，并且能够正确保存。


#### feed.tpl

现在我们已经设置完插件，开始创建主要执行部分 —— template.

