插件连接
===================================

### 通过钩子连接插件

显示插件操作的结果，不需要检查插件的可用性（例如：: {if $settings.Addons.affiliate == "Y"}）和写一些代码来显示结果。钩子是用来连接插件的。

为了创建一个钩子需要做如下操作：

* 在 addon 目录创建 hooks 子目录（如果不存在）
* 在 hooks 子目录再创建一个目录并命名为将在控制器的什么地方调用
* 在目录中用下列格式创建模版：
  * __hook_name.action_performed_with_hook_code.tpl__
  * __action_performed_with_hook_code__ 可以拥有3个选项：
      * __- post__ —— 结果在钩子声明之后被添加
      * __- pre__ —— 结果在钩子声明之前被添加
      * __- override__ —— 结果覆盖钩子模块
      * 例如：product_info.post.tpl
* 通过模版中连接钩子的参数名称创建钩子函数块. 名称的指定应按照如下方式: (目录名称意味着在钩子模版中的调用位置，定义方法在上面第2点中有提到):(hook_name)

> __更改现有钩子模版时需要当心，因为他们可能在多个地方调用了。__

### 例子

1.使用 __pre__ 和 __post__ 行为，这两个是不同的模版：
  * rma/hooks/orders/xxx.pre.tpl 生成代码 <p>Front</p>
  * rma/hooks/orders/xxx.post.tpl 生成代码 <p>Back</p>

钩子声明：

```html
{hook name="orders:xxx"}
<p>Middle</p>
{/hook}
```

将会生成以下代码：

```html
<p>Front</p>
<p>Middle</p>
<p>Back</p>
```

2.使用 __override__ 行为，这是钩子模版：
* rma/hooks/orders/xxx.override.tpl 生成代码 <p>Substitute</p>

钩子声明：

```html
{hook name="orders:xxx"}
<p>Replaceable</p>
{/hook}
```


当启用插件的RMA，会生成以下代码：

```html
<p>Substitute</p>
```
