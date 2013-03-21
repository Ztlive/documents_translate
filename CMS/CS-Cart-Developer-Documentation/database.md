数据库
===================================

## 数据库层

从CS-Cart 2.0开始，数据库请求使用占位符方式：

### ?u —— 用于构造一个数据更新，接收数组参数

```php
$data = array (
    'payment_id' => 5
);
$order_id = 3;
db_query('UPDATE ?:orders SET ?u WHERE order_id = ?i', $data, $order_id);
```

作用和这条SQL语句等效：

```sql
UPDATE cscart_orders SET payment_id = '5' WHERE order_id = 3;
```


### ?e —— 用于构造一个数据插入，接收数组参数

```php
$data = array (
    'payment_id' => 5,
    'order_id' => 3
);
db_query('INSERT INTO ?:orders ?e', $data);
```

作用和这条SQL语句等效：

```sql
INSERT INTO cscart_orders (payment_id, order_id) VALUES ('5', '3');
```

### ?i —— 将数据转换为整型，接收字符串和数字参数

```php
$order_id = 4;
db_query('SELECT * FROM ?:orders WHERE order_id = ?i', $order_id);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id = 4;
```

### ?s —— 将数据转换为字符串（自动添加转义符），接收字符串和数字参数

```php
$order_id = 'adasd';
db_query('SELECT * FROM ?:orders WHERE order_id = ?s', $order_id);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id = 'adasd';
```

### ?l —— 将数据转换为字符串用于实现LIKE操作（替换双斜杠和反斜杠之后添加斜杠），接收字符串参数

```php
$piece = '%black\white%';
db_query('SELECT * FROM ?:product_descriptions WHERE product LIKE ?l', $piece);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_product_descriptions WHERE product LIKE '%black\\\\white%';
```

### ?d —— 将数据转换为浮点型数字，接收字符串和数字参数

```php
$order_id = '123.345345';
db_query('SELECT * FROM ?:orders WHERE order_id = ?d', $order_id);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id = '123.35';
```

### ?a —— 处理数据为一组字符串用于构造IN()查询，接收字符串、数字和数组参数

```php
$order_id = '123';
db_query('SELECT * FROM ?:orders WHERE order_id IN (?a)', $order_id);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id IN ('123');
```

### ?n —— 处理数据为一组整数用于构造IN()查询，接收字符串、数字和数组参数

```php
$order_id = '123.45';
db_query('SELECT * FROM ?:orders WHERE order_id IN (?n)', $order_id);
```

作用和这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id IN (123);
```

### ?p —— 嵌入一组完整的值

```php
$order_id = 'order_id = 4';
db_query('SELECT * FROM ?:orders WHERE ?p', $order_id);
```

作用和下面这条SQL语句等效：

```sql
SELECT * FROM cscart_orders WHERE order_id = 4;
```

### ?w —— 用于WHERE条件的数据，接收数组参数

```php
$data = array (
    'payment_id' => 5,
    'order_id' => 3
);
 
db_query('SELECT * FROM ?:orders WHERE ?w', $data);
```

作用和下面这条SQL语句等效：

```sql
SELECT * cscart_orders WHERE payment_id = '5' AND order_id = '3';
```

### ?f —— 检查变量值是否是有效的字段名，如果不是将返回空

```php
$data = 'payment_id';
db_query('SELECT * FROM ?:orders WHERE ?f = 5', $data);
```

作用和下面这条SQL语句等效：

```sql
SELECT * cscart_orders WHERE  = 5;
```

## 老版本的差异

* 弃用 db_update_by_array, db_insert_by_array，统一使用db_query来替代，返回自增长字段值
* 函数db_quote添加到独立进程请求
* 函数db_get_hash_multi_array, db_get_hash_single_array的附件参数通过数组传递

//here is what we had

```php
db_get_hash_single_array("SELECT * FROM ?:orders WHERE payment_id = '5'", 'order_id', 'payment_id');
```

// and we got the following

```php
db_get_hash_single_array("SELECT * FROM ?:orders WHERE payment_id = ?i", array('order_id', 'payment_id'), $payment_id)
```
