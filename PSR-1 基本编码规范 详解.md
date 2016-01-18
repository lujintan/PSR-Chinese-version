#PSR-01 基本编码规范 详解

本篇规范制定了代码基本元素的相关标准，以确保共享的PHP代码间具有较高程度的技术互通性。
文档中关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" 的含义参见 [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)。

#1. 概述
* 文件`MUST`只能使用`<?php`和`<?=`标签
* 文件`MUST`只能用不带BOM头的UTF-8编码
* 文件`SHOULD`只能出现(classes, functions, constants, 等)声明代码或产生`从属效应`(如：产生输出，更改 .ini 设置，等)的代码中的一种，`SHOULD NOT`都存在
* 命名空间和类`MUST`符合“自动加载”规范，PSR: [[PSR-0](http://www.php-fig.org/psr/psr-0/), [PSR-4](http://www.php-fig.org/psr/psr-4/)]中的一种。(注：其中PSR-0已经废弃了)
* 类的命名`MUST`遵循`StudlyCaps`大写开头的驼峰命名规范
* 类中的常量所有字母都`MUST`大写，单词间用下划线分隔
* 方法名称必须符合`camelCase`式的小写开头驼峰命名规范

#2. 文件
##2.1 PHP标签

PHP代码`MUST`只能使用`<?php ?>`或`<?= ?>`标签中的一种

##2.2 文件编码

PHP代码`MUST`只能用不带BOM头的UTF-8编码。(注：不了解BOM是什么的，[点击这里](http://www.kkyfj.com/php/2016/01/03/BOM-introduction.html))

##2.3 从属效应

文件`SHOULD`只能出现(classes, functions, constants, 等)`声明`代码或产生`从属效应`(如：产生输出，更改 .ini 设置，等)的代码中的一种，`SHOULD NOT`都存在

“从属效应”(side effects)的意思是，不声明类、函数和常量等，而仅仅通过包含文件就执行的逻辑操作。

“从属效应”包含却不仅限于：产生输出、直接的`require`或`include`、连接外部服务、修改ini配置、抛出错误或异常、修改全局或静态变量、读或写文件等。

下面代码反例同时包含了`声明`和`从属效应`代码：

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

下面例子是包含了`声明`但不包含`从属效应`的代码：

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```

#3. 命名空间和类的命名

命名空间和类`MUST`符合“自动加载”规范，PSR: [[PSR-0](http://www.php-fig.org/psr/psr-0/), [PSR-4](http://www.php-fig.org/psr/psr-4/)]中的一种。(注：其中PSR-0已经废弃了)

类的命名`MUST`遵循`StudlyCaps`大写开头的驼峰命名规范

PHP 5.3及以后版本的代码`MUST`使用正式的命名空间(注：namespace的概念是PHP 5.3引入的)

例：

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

5.2.x及之前的版本`SHOULD`使用伪命名空间，通常类名前面已`Vendor_` 为前缀

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

#4. 类的常量、属性和方法

这里“类”指代所有的classes、interfaces和traits

##4.1 常量

类中的常量所有字母都`MUST`大写，单词间用下划线分隔，例：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

##4.2 属性

本规范不刻意要求属性名的书写方式，可以使用`StudlyCaps`(首字母大写驼峰),`camelCase`(首字母小写驼峰),`under_score`(下划线)等。
不管使用那种命名规范`SHOULD`在一定范围内保持统一，范围可以是一个命名空间、一个包、一个类或者一个方法。

##4.3 方法
方法名称必须符合`camelCase`式的小写开头驼峰命名规范


原文参考：[PSR-1: Basic Coding Standard](http://www.php-fig.org/psr/psr-1/)