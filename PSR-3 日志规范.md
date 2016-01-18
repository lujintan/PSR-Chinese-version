本篇描述日志库的一个通用的接口规范。

核心目标是能够让基础库实现一个`Psr\Log\LoggerInterface`接口，以便能够以最简单和统一的方式打日志。有自己实现日志功能需求的框架和内容发布系统`MAY`根据自己的需求扩展这个接口，但是`SHOULD`与本规范兼容。这样可以保证一个应用使用的第三方库日志可以集中管理。

文档中关键词 "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" 的含义参见 [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)。

该文档提到的关键词`实现者`是指在日志相关的库或者框架中实现`LoggerInterface`的人。日志工具的使用者也被称为用户。

#. 说明

##1.1 基础

`LoggerInterface`暴露8个写日志的方法，对应[RFC 5424](http://tools.ietf.org/html/rfc5424)的8个级别(debug, info, notice, warning, error, critical, alert, emergency)

第9个方法:log, 会在第一个参数接收一个log级别。调用这个方法。通过传入log级别常量调用这个方法`MUST`和直接调用特定级别的方法效果一样。调用时传入一个未知的log级别`MUST`抛出一个`Psr\Log\InvalidArgumentException`。`SHOULD NOT`使用一个该规范为提及的log级别。

##1.2 消息

* 每个方法接收一个字符串或者有`__toString()`方法的对象作为消息记录。`实现者``MAY`对传过来的对象做一些特殊的处理。如果对象不满足要求，`实现者``MUST`将其转换成字符串。
* 消息`MAY`包含一个占位符，`实现者``MAY`用一个`上下文数组`进行替换。（注：说的其实主要是实现一个最基本的字符串模板功能）
    
    占位符名称`MUST`能对应一个`上下文数组`中的key。
    占位符名称`MUST`使用`{`、`}`作为`定界符`。定界符与占位符名称之间`MUST NOT`有空格。
    占位符名称`SHOULD`只能由A-Z, a-z, 0-9, 下划线 “_”, 以及英文句号“.”构成。其他符号作为预留以备未来可能对占位符规范的扩展。
    `实现者``MAY`使用占位符去实现各种策略去实现转义或转换日志的展示，用户`SHOULD NOT`在不了解代码上下文的情况下预先转义占位符内容。

下面是一个实现了上面提及的在占位符中插入内容的示例：

```php
  /**
   * Interpolates context values into the message placeholders.
   */
  function interpolate($message, array $context = array())
  {
      // build a replacement array with braces around the context keys
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // interpolate replacement values into the message and return
      return strtr($message, $replace);
  }

  // a message with brace-delimited placeholder names
  $message = "User {username} created";

  // a context array of placeholder names => replacement values
  $context = array('username' => 'bolivar');

  // echoes "User bolivar created"
  echo interpolate($message, $context);
```

##1.3 上下文

每个方法接收一个数组参数作为上下文数据。主要为了携带一些不合适用字符串表示的数据。上下文数组可以包含任何数据。`实现者``MUST`保证对上下文数组有足够的容忍度。
对于上线文数组`MUST NOT`抛出任何异常、产生任何php error, warning或者notice。
如果一个上下文数组传递了一个`Exception`对象，它`MUST`使用`excption`作为`key`。记录异常是有非常通用的模式，如果日志底层支持它`实现者`就能够提取堆栈信息。`实现者`在使用'exception'之前`MUST`验证'exception'key对应的值是否确实是一个`Exception`类型。因为它`MAY`是任何类型的。

##1.4 辅助类和接口

通过继承和实现`Psr\Log\AbstractLogger`会使你实现接口`LoggerInterface`变的非常简单。另外8个方法能够给它传递信息和上下文数组。
相似的，使用`Psr\Log\LoggerTrait`只需要你去实现通用log方法。注意因为`trait`不能实现接口，所以你仍然需要实现`LoggerInterface`。

`Psr\Log\NullLogger`和接口一同提供出来。那些没有日志记录器的`MAY`使用`Psr\Log\NullLogger`，一个备用的日志“黑洞”。当上下文的构造非常复杂时，按条件记录日志或许是更好的做法。

`Psr\Log\LoggerAwareInterface` 仅包含一个 `setLogger`(LoggerInterface $logger)方法，框架可以用一个日志记录器的示例来自动装填。

使用`Psr\Log\LoggerAwareTrait`可以很容易的实现一个和通过实现接口同样的效果的类。通过`$this->logger`去访问日志记录器。

`Psr\Log\LogLevel`类包含了8个日志级别的常量。

#2. 包

接口和类的描述以及相关异常类和一个适合检验实现的的测试用例在包含在[psr/log](https://packagist.org/packages/psr/log)包中。

#3. Psr\Log\LoggerInterface

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger instance
 *
 * The message MUST be a string or object implementing __toString().
 *
 * The message MAY contain placeholders in the form: {foo} where foo
 * will be replaced by the context data in key "foo".
 *
 * The context array can contain arbitrary data, the only assumption that
 * can be made by implementors is that if an Exception instance is given
 * to produce a stack trace, it MUST be in a key named "exception".
 *
 * See https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * for the full interface specification.
 */
interface LoggerInterface
{
    /**
     * System is unusable.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * Action must be taken immediately.
     *
     * Example: Entire website down, database unavailable, etc. This should
     * trigger the SMS alerts and wake you up.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * Critical conditions.
     *
     * Example: Application component unavailable, unexpected exception.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * Runtime errors that do not require immediate action but should typically
     * be logged and monitored.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * Exceptional occurrences that are not errors.
     *
     * Example: Use of deprecated APIs, poor use of an API, undesirable things
     * that are not necessarily wrong.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * Normal but significant events.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * Interesting events.
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * Detailed debug information.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * Logs with an arbitrary level.
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```
#4. Psr\Log\LoggerAwareInterface

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger-aware instance
 */
interface LoggerAwareInterface
{
    /**
     * Sets a logger instance on the object
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

#5. Psr\Log\LogLevel

```php
<?php

namespace Psr\Log;

/**
 * Describes log levels
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```