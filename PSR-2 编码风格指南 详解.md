本篇规范制定基于[PSR-1]()基本编码规范进行扩展。

通过一系列通用的规则和建议去规范PHP代码的书写，以降低在浏览不同作者开发的代码造成的不便。

本篇的规则总结各种不同的项目的通用特性。当不同的开发者在并行开发不同的项目时需要有一个统一的规范来指导开发。因此规范的作用并不在于规则本身，而在于所有开发人员一同对规范的遵守。

文档中关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" 的含义参见 [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)。

#1. 概述

* 代码`MUST`遵守[PSR-1]()规范
* 代码缩进`MUST`使用4个空格，`MUST NOT`使用`tab`
* `MUST NOT`强制限制单行代码长度，软性要求`MUST`在120字以内，通常情况`SHOULD`为80字(注：其他语言的规范通常也会做类似的限制，但HTML语言在制定规范时通常不做单行字符数限制)
* `namespace`声明语句下面`MUST`有一个空行，`use`声明语句块下面`MUST`有一个空行
* 类起始的花括号`{`必须在类声明语句的下一行，单独成行。结束花括号`}``MUST`在内容结束后单独一行
* 方法起始的花括号`{`必须在方法声明语句的下一行，单独成行。结束花括号`}``MUST`在内容结束后单独一行
* 类中的任何属性和方法`MUST`设置访问修饰符(`private`,`public`,`protected`)，`abstract`,`final`必须在访问修饰符之前，`static``MUST`在访问修饰符之后（注：PHP不设置访问修饰的属性默认为是public的）
* 控制结构的关键词后面`MUST`有一个空格，方法的调用`MUST NOT`
* 控制结构的起始的花括号`{``MUST`和控制语句在同一行，结束花括号`}``MUST`在内容结束后另起一行
* 控制结构的起始的括号`(`后面`MUST NOT`有空格，结束括号`)`前面`MUST NOT`有空格

##1.1 例

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

#2. 通用规范
##2.1 通用编码规范

代码`MUST`遵循[PSR-1](/php/2016/01/06/PSR-01.html)

##2.2 文件

所有PHP文件`MUST`使用 Unix LF(linefeed)换行符作为结尾（注：要了解更多Unix LF，[点击这里]()）
所有PHP文件`MUST`已一行空行作为文本的结束（注：此处我个人的理解是，Unix把文件的处理视作流式的，也就是说可以任意拼接的）
所有PHP文件`MUST`删除结束符`?>`，只保留开始的`<?php`（注：PHP文件不加结束符，默认已文件结尾作为结束。如果加了结束符，当文件被include的时候，结束符后面的内容将会被当做标准输出）

##2.3 行

`MUST NOT`强制限制单行代码长度。

软性要求`MUST`在120字以内，如果超出，代码自动检测工具`MUST`报异常，但`MUST NOT`报错误。

单行字符长度`SHOULD NOT`超过80字，如果超过，`SHOULD`切换成多行，并且保持每行不超过80字。

空行中`MUST NOT`添加多余的空格。

添加空行`MAY`增加代码的可读性并指示出不同的代码块。

每行语句数`MUST NOT`多与一条

##2.4 缩进

代码缩进`MUST`使用4个空格，`MUST NOT`使用`tab`（注：也有的规范建议可以使用2个空格或4个空格，个人理解，2个空格更容易控制单行字符数不超过80，但可读性较差，目前绝大多数都建议使用4个字符）

备注：只用空格而不是空格和`tab`混合使用，可以避免文件diff、打补丁、浏览历史记录和注释造成的不便。只使用空格还可以使行内更细粒度的缩进变得更加简单。

(注：比如我想实现下面的效果，让所有的访问修饰符，变量的等号等对其，是不是使用空格更容易搞定)

```php
<?php
namespace Vendor\Package;

class Foo
{
    public  $a     = 1;
    private $abc   = 2;
    private $abcd  = 3;
    private $b     = 4;
}

```

##2.5 关键词 和 Ture/False/Null

PHP 关键词 `MUST` 使用小写
PHP 常量 `true`,`false`,`null` `MUST`使用小写

#3 namespace 和 use 的声明

`namespace`声明语句下面`MUST`有一个空行

`use`的声明`MUST`在`namespace`声明之后

每一个声明`MUST`只能有一个`use`

`use`声明语句块下面`MUST`有一个空行

例：

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...
```

#4. 类、属性和方法

这里提到了类表示所有 `class`, `interface` 和 `trait`

##4.1 继承 和 实现

`extends`和`implements`关键词`MUST`和类名声明在同一行

类起始的花括号`{`必须在类声明语句的下一行，单独成行。结束花括号`}``MUST`在内容结束后单独一行

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

实现可以分割成多行，但随后每行`MUST`独立成行。一旦按多行方式去编写代码，第一个实现的接口`MUST`在类名的下一行，每个接口自成一行。

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

##4.2 属性

类中的任何属性`MUST`设置访问修饰符(`private`,`public`,`protected`)

`MUST NOT`用`var`关键词进行属性声明（注：使用`var`效果等同于`public`）

每条语句`MUST NOT`声明超过一个属性

属性命名`SHOULD NOT`通过在用一个下划线开始来标识是`protected`或`private`

一个属性声明应该入下面所示：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

##4.3 方法

类中任何方法`MUST`设置访问修饰符(`private`,`public`,`protected`)

方法命名`SHOULD NOT`通过在用一个下划线开始来标识是`protected`或`private`

方法命后面`MUST NOT`有空格，起始的花括号`{``MUST`单独一行，结束花括号`}``MUST`在内容结束后另起一行。起始的括号`(`后面`MUST NOT`有空格，结束括号`)`前面`MUST NOT`有空格

一个方法的声明如下所示，注意`(`,`,`,` `,`{`的位置：

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

##4.4 方法参数

参数列表中每个逗号后面`MUST NOT`有空格，逗号前面`MUST`有空格。

参数有默认`MUST`在参数列表的最后。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

参数列表`MAY`分成多行书写，每行要进行一次缩进。当按照多行书写参数，第一个参数`MUST`在方法名下一行且每行只能写一个参数。

当参数分成多行书写，结束`)`和起始`{``MUST`独占一行，并且两个符号之间有一个空格。

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

##4.5. abstract、final 和 static

当使用`abstract`和`final`修饰符时，`MUST`在权限修饰符之前。

当使用`static`修饰符时，`MUST`在权限修饰符之后。

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

##4.6. 方法调用

当发生方法调用，方法名和起始`(`之间`MUST NOT`有空格，开始`(`后`MUST NOT`有空格。结束`)`前`MUST NOT`有空格。参数列表中逗号前`MUST NOT`有空格，逗号后`MUST`有空格。

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

参数列表`MAY`分成多行书写，每行要进行一次缩进。当按照多行书写参数，第一个参数`MUST`在方法名下一行且每行只能写一个参数。

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

#5. 控制结构

对控制结构通用的规范如下:

* 每个控制结构关键词后`MUST`有一个空格
* 起始`(`后面`MUST NOT`有空格
* 结束`)`前面`MUST NOT`有空格
* 结束`)`和起始`{`之间`MUST`有空格
* 控制结构体内容`MUST`进行一次缩进
* 结束`}``MUST`在内容结束后单独一行
* 结构体内容`MUST`被`{`、`}`包裹。这使得结构体看起来更加标准化，减少新添加行时引入错误的可能性。

##5.1. if, elseif, else

`if`结构体如下示例。注意`(`、` `和`{`的位置；`else`和`elseif`与之前内容的结束`}`在同一行。

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

`SHOULD`使用`elseif`代替`else if`，以便使所有的关键词看起来都是一个独立的词。

##5.2. switch, case

`switch`结构体如下示例。注意`(`、` `和`{`的位置；`case``MUST`比`switch`多缩进一次，`break`(或者其他表示结束的关键词)`MUST`和`case`下的内容保持统一缩进。当不需要`break`直接穿透到后面`case`的时候，`MUST`加一行注释：`// no break`。

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

##5.3. while, do while

`while`结构体如下示例。注意`(`、` `和`{`的位置；

```php
<?php
while ($expr) {
    // structure body
}
```

`do while`结构体如下示例。注意`(`、` `和`{`的位置；

```php
<?php
do {
    // structure body;
} while ($expr);
```

##5.4. for

`for`结构体如下示例。注意`(`、` `和`{`的位置；

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

##5.5. foreach

`foreach`结构体如下示例。注意`(`、` `和`{`的位置；

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

##5.6. try, catch

`try catch`结构体如下示例。注意`(`、` `和`{`的位置；

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

#6. 闭包

闭包的声明在`function`关键词后`MUST`有一个空格，在`use`关键词的前后要各有一个空格。

开始`{``MUST`在声明语句同一行，结束`}`必须在内容结束后单独一行。

开始参数列表和变量列表的开始`(`后`MUST NOT`有空格，在结束`)`前`MUST NOT`有空格

在参数和变量列表中，在逗号前`MUST NOT`有空格，逗号后`MUST`有空格。

闭包参数有默认值的`MUST`在参数列表的最后。

闭包的声明如下面所示。注意`(`、`,`、` `和`{`的位置：

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

参数和变量列表`MAY`分成多行书写，每行要进行一次缩进。当按照多行书写参数，第一个参数或变量`MUST`在方法名下一行且每行只能写一个参数或变量。

在被分成多行的列表(参数或变量)结尾，结束`)` 和 开始`{``MUST`在同一行且中间要有空格分隔。

下面示例分别展示了当存在或不存在参数列表和变量列表时多行列表的代码样式。

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

注意，以上规范对于作为方法参数的闭包依然适用。

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```

#7. 总结

在这份规范中，有许多编码风格和实践都刻意的删掉了。其中包含但不仅限于：

* 全局变量和常量的声明
* 方法的声明
* 操作符和赋值
* 行内对其
* 注释和文档描述
* 类名前缀和后缀
* 最佳实践

后续的规范建议`MAY`针对这些或其他的方面进行修改和扩展。


原文参考：[PSR-2: Coding Style Guide](http://www.php-fig.org/psr/psr-2/)