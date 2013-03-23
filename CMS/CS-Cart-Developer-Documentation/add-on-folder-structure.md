插件目录结构
===================================

插件可以在所需目录中包含下列目录进行任意组合：

* /addons/[addon_name] – 插件的PHP文件的所需目录
* /var/skins_repository/basic/customer/addons/[addon_name] – 前台模版目录
* /var/skins_repository/basic/admin/addons/[addon_name] - 后台模版目录
* /var/skins_repository/basic/mail/addons/[addon_name] – e-mail模版目录

## 插件的PHP文件的所需目录 - /addons/addon_name

* addon.xml - 该文件描述了插件的主要数据，在安装和删除插件时是必须的。
* func.php - 插件控制器用到的函数。
* init.php - 首要功能 – 注册插件用到的钩子列表
* config.php - 插件的配置数据
* /controllers - 目录包含了插件的控制器以及程序标准控制器的前置和后置控制器。
* /schemas - 包含扩展程序标准模式的文件
* 此外，可根据开发者的需要在此目录创建子目录和文件，加密标准要求名称和结构要和主程序的结构完全一致。