文档中关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" 的含义参见 [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)。

#1. 概述

本篇为PHP类的自动加载规范。本规范完全可互用，可作为其他自动加载说明的补充，包括PSR-0。本规范还包含了自动加载类对应文件放置位置的介绍。

#2. 说明

##1. 这里提到的“类”泛指所有 `class`, `interface`, `trait`以及其他类似的结构
##2.一个完整的类的命名格式如下:

    \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>
    \<顶级命名空间>(\<子-命名空间>)*\<类名>

1. 完整的类的命名`MUST`有一个顶级命名空间(vendor namespace)
2. 完整的类的命名`MAY`有一个或多个子命名空间
3. 完整的类的命名`MUST`有一个最终的类名
4. 在整个类的命名中下划线没有任何特殊含义
5. 字母在整个类的命名中`MAY`任意大小写混合
6. 所有的类名`MUST`大小写敏感

##3. 加载一个文件需要与完整的类的命名有关联关系...

1. 完整的类的命名中，相邻的一个或多个“父级命名空间”和“子级命名空间”，不包含“顶级命名空间”的分隔符（命名空间前缀），至少和一个文件“基目录”对应
2. 命名空间前缀相邻的子命名空间与一个“基目录”下的子目录相对应，命名空间的分隔符代表目录分隔符，子目录`MUST`与子命名空间相同。
3. 最终类名与一个`.php`文件对应。文件名`MUST`与类名相同。

##4. 自动加载的实现`MUST NOT`抛出异常，`MUST NOT`抛出任何一级的错误，并且`SHOULD NOT`有返回值。

#. Examples

下面的表单展示了文件路径和完整的类的命名、命名空间前缀、基目录的对应关系。
The table below shows the corresponding file path for a given fully qualified class name, namespace prefix, and base directory.

|*完整的类的命名*|*命名空间前缀*|*基目录*|*文件目录*|
|-|
|\Acme\Log\Writer\File_Writer|Acme\Log\Writer|./acme-log-writer/lib/|./acme-log-writer/lib/File_Writer.php|
|\Aura\Web\Response\Status|Aura\Web|/path/to/aura-web/src/|/path/to/aura-web/src/Response/Status.php|
|\Symfony\Core\Request|Symfony\Core|./vendor/Symfony/Core/|./vendor/Symfony/Core/Request.php|
|\Zend\Acl|Zend|/usr/includes/Zend/|/usr/includes/Zend/Acl.php|

关于上面的规范，这里有一个[示例文件](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader-examples.md)。示例的实现`MUST NOT`作为规范的一部分，因为后续`MAY`随时修改。

原文参考：[PSR-4: Autoloading Standard](http://www.php-fig.org/psr/psr-4/)