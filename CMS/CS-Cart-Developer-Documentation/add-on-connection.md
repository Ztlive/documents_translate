插件连接
===================================

### 通过钩子连接插件

显示插件操作的结果，不需要检查插件的可用性（例如：: {if $settings.Addons.affiliate == "Y"}）和写一些代码来显示结果。钩子是用来连接插件的。

为了创建一个钩子需要做如下操作：

* 在 addon 目录创建 hooks 子目录（如果不存在）
* 在 hooks 子目录再创建一个目录并命名为将在控制器的什么地方调用
* 在目录中用下列格式创建模版：
  __hook_name.action_performed_with_hook_code.tpl__
  __action_performed_with_hook_code__ 可以拥有3个选项：
  __- post__ —— 结果在钩子声明之后被添加
  __- pre__ —— 结果在钩子声明之前被添加
  __- override__ —— 结果覆盖钩子模块
  例如：product_info.post.tpl
