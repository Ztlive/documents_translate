命名空间
===================================

Tygh(发音：[tai])是CS-Cart的命名空间，所有的核心类都被包含在这个命名空间内，一些功能类似的类将会加入子空间，比如：块管理(Tygh\BlockManager)和API(Tygh\Api)。命名空间的定义如下：

```php
namespace Tygh;
```

子空间的定义如下：

```php
namespace Tygh\BlockManager;
```

在命名空间内定义的所有的类，函数，常量等等都可以通过global或者在另一个命名空间内明确指定前者命名空间的名称访问到。

例如，一个文件有如下内容：

```php
namespace My\Name;
 
class MyClass {}
function myfunction() {}
const MYCONST = 1;
 
$a = new MyClass; // All correct here
```

在另一个文件中，如果不属于同一个命名空间，则需要声明命名空间的名称。

```php
$c = new \My\Name\MyClass; // 这样是正确的
 
$c = new MyClass; // 这样是错误的
```

现在，让我们来使用 use 指令(the right way to define a namespace):

```php
use My\Name;
 
$c = new \My\Name\MyClass; // 这样没问题
 
$c = new MyClass; // 这样也同样没问题，而且这样做更好
```

命名空间只能通过use指令引入，include或require文件是无效的。

> 每个使用命名空间的文件都需要在开头使用use指令。使用别名（alias）防止类名冲突。不建议在命名空间中使用完整的类名，也是没有必要的。

__强制将use指令写在一起__


use指令的作用和'require' ('include')类似：它不包含文件，只是在命名空间区域预先规整每个类名。

```php
use Tygh\Registry;
use Tygh\Settings;
use Tygh\Addons\SchemesManager as AddonSchemesManager;
use Tygh\BlockManager\SchemesManager as BlockSchemesManager;
use Tygh\BlockManager\ProductTabs;
use Tygh\BlockManager\Location;
use Tygh\BlockManager\Exim;
```