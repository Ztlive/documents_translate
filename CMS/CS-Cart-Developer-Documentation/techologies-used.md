应用技术
===================================

我们尝试使用可靠且成熟的技术让CS-Cart良好地运作。我们期望CS-Cart易于定制，所以需要使用成熟且流行的技术。于是我们选择了一套完美的解决方案：LAMP（Linux-Apache-MySQL-PHP）。

## MVC
CS-Cart遵循通用的MVC ([Model-View-Controller](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller))开发模式

数据模型是直接存储在数据库中。数据库使用的是MYSQL，所以程序从模型中得到数据，MYSQL的SQL查询语句使用占位符帮助(help of placeholders)。示例：

```php
$data = array (
    'payment_id' => 5
);
$order_id = 3;
db_query('UPDATE ?:orders SET ?u WHERE order_id = ?i', $data, $order_id);
```

控制器使用PHP编写，通常CS-Cart的控制器均被放在controllers目录，虽然插件也可以定义他们自己的控制器并分别存放在不同的插件目录中。

数据通过视图向用户展示，CS-Cart使用了Smarty模版引擎，并且使用Javascript脚本来表现数据。下面一个例子用于展示在Smarty模版里嵌入Javascript脚本。

```html
{** block-description:my_twitter_addon **}
<script src="http://widgets.twimg.com/j/2/widget.js"></script>
<script type="text/javascript">
new TWTR.Widget({ldelim}
    version: 2,
    type: 'profile',
    rpp: {$addons.my_twitter_addon.number_of_tweets},
    interval: 6000,
    width: 'auto',
    height: 300,
    theme: {ldelim}
        shell: {ldelim}
            background: '#FFFFFF',
            color: '#373737'
        {rdelim},
        tweets: {ldelim}
            background: '#D9EFF3',
            color: '#373737',
            links: '#005865'
        {rdelim}
    {rdelim},
    features: {ldelim}
        scrollbar: true,
        loop: true,
        live: true,
        hashtags: true,
        timestamp: true,
        avatars: true,
        behavior: 'default'
    {rdelim}
{rdelim}).render().setUser('{$addons.my_twitter_addon.username}').start();
</script>
```

## MYSQL
MYSQL是当今web领域中最流行的关系型数据库管理系统。CS-Cart使用MYSQL，因为它是一款高性能并且非常易于使用的软件，MYSQL的查询是直接使用CS-Cart控制器（占位符帮助(with the help of placeholders)），所以要想扩展CS-Cart，掌握它的语法是基本要求。

## PHP
CS-Cart使用PHP编写，PHP是当今web领域使用最广泛的语言。

## Smarty
CS-Cart使用Smarty模版引擎替代原生的HTML，它提供了更灵活的数据表现，并可以通过Smarty标记语言来构建复杂的页面结构。

## Javascript
大量的使用AJAX和Javascript构建动态的页面内容。