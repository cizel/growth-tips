# 符合PSR2 的 PHP 编码规范

## 编码规范

- PHP代码文件必须以 <?php 标签开始。

```php
<?php //开头

// 不结尾
```

- PHP代码文件必须以不带BOM的UTF-8编码。

```php
//例sublime, setting增加，
{
  "show_encoding" : true
}
```

- 每行的字符数不超过 80 个字符

```php
//例，sublime
{
  "word_wrap": "true",
  "wrap_width": 80,
}
```

- tab键4个空格

```php
//例，sublime
{
  "tab_size": 4,
}
```

- PHP代码中应该只定义类(trait)/函数/常量/其他会产生副作用的操作（如：生成文件输出以及修改 .ini 配置文件等），只能选其一。

```php
//a.php
class A
{

}

//b.php
function demo()
{

}

//c.php
define('A', value);

//d.php
ini_set('some_vars', value);
```

- 类/trait/Interface的命名必须遵循 StudlyCaps 大写开头的驼峰命名规范。

```php
class StudlyCaps
{

}

trait StudlyCaps
{

}

Interface StudlyCaps
{

}
```

- 类中的常量所有字母都必须大写，单词间用下划线分隔。

```php
define('FOO_BAR', 'something more');

const FOO_BAR = value;
```

- 方法(类/trait中)名称必须符合 camelCase 式的小写开头驼峰命名规范。

```php
class StudlyCaps
{
    public function studlyCaps()
    {
        // coding...
    }
}
```

- 函数名称必须符合 snake_case 式的下划线式命名规范。

```php
function snake_case()
{
    // coding...
}
```

- 私有的(private)方法(类/trait中)名称必须符合 小写开头驼峰命名规范。

```php
class StudlyCaps
{
    private function studlyCaps()
    {
        // coding...
    }
}
```

- 方法名称 第一个单词 为动词。

```php
class StudlyCaps
{
    public function doSomething()
    {
        // coding...
    }
}
```

- 变量 必须符合 camelCase 式的小写开头驼峰命名规范。

```php
class StudlyCaps
{
    public function doSomething()
    {
        $someVariable = 'demo';
        // coding...
    }
}
```

- 方法/函数 多参数时，之间要有1个空格

```php

class StudlyCaps
{
    public function doSomething($variableOne, $variableTwo)
    {
        // coding...
    }
}
```

- 运算符/表达式 要有一个空格

```php
$a = $b + $c;
$a = $b . $c;
```

- 每个 namespace 命名空间声明语句块 和 use 声明语句块后面，必须 插入一个空白行。

```php
namespace Standard;
// 空一行
use Test\TestClass;//use引入类
// 空一行
```

- 类的开始花括号 "{ "必须 写在函数声明后自成一行，结束花括号"}"也必须写在函数主体后自成一行。

```php
class StudlyCaps
{

}
```

- 方法/函数的开始花括号 { 必须 写在函数声明后自成一行，结束花括号 }也 必须 写在函数主体后自成一行。

```php
class StudlyCaps
{
    public function studlyCaps()
    {
        // coding...
    }
}

function snakeCase()
{
    // coding...
}
```

- 类的属性和方法 必须 添加访问修饰符（private、protected 以及 public），abstract 以及 final 必须 声明在访问修饰符之前，而 static 必须 声明在访问修饰符之后。

```php
abstract class StudlyCaps
{
    abstract public function studlyCaps();

    final public static function studlyCapsOne()
    {

    }
}
```

- 控制结构的关键字后 必须 要有一个空格符，而调用方法或函数时则 一定不可 有。

```php
if ($valueOne === $valueTwo) {
  // code...
}

switch ($valueThree) {
  case 'value':
    // code...
    break;

  default:
    // code...
    break;
}

do {
  // code...
} while ($valueFour <= 10);

while ($valueFive <= 10) {
  // code...
}

for ($i = 0; $i < $valueSix; $i++) {
  // code...
}

$demo = new Demo()
$demo->doSomething();

doSomething();
```

- 控制结构的开始花括号 { 必须 写在声明的同一行，而结束花括号 } 必须 写在主体后自成一行。

```php
if ($valueOne === $valueTwo) {
    // code...
}

switch ($valueThree) {
case 'value':
    // code...
    break;

default:
    // code...
    break;
}

do {
    // code...
} while ($valueFour <= 10);

while ($valueFive <= 10) {
    // code...
}

for ($i = 0; $i < $valueSix; $i++) {
    // code...
}
```

- 控制结构的开始左括号后和结束右括号前，都一定不可有空格符。

```php
if ($valueOne === $valueTwo) {// 控制结构（右边和)左边不加空格
  // code...
}
```

## 编码建议

- sql过长

```php
// heredoc语法
$sql = <<<SQL
SELECT delivery_id
FROM d_test
WHERE delivery_id
IN (123,234)
GROUP BY delivery_id
HAVING SUM(send_number) <= 0;
SQL;
```

- if等控制结构条件过长

```php
if ($a > 0
    && $b > 0
    && $c > 0
    && $d > 0
    && $e > 0) {

}
```

- 方法或函数参数大于三个换行

```php
public function tooLangFunction(
      $valueOne   = '',
      $valueTwo   = '',
      $valueThree = '',
      $valueFour  = '',
      $valueFive  = '',
      $valueSix   = '')
{
    //coding...
}
```

- 链式操作超过两个

```php
$this->nameTest->functionOne()
               ->functionTwo()
               ->functionThree();
```

- 数组php5.4以后，使用[]

```php
$a = [
    'aaa' => 'aaa',
    'bbb' => 'bbb'
];
```

- 单引号多引号

```php
//字符串中无变量，单引号
$str = 'str';
//字符串中有变量，双引号
$arg = "$str";
```

- 声明类或者方法或函数添加描述&属性描述&作者

```php
/**
 * 类描述
 *
 * desc
 */
class StandardExample
{
    /**
    *  常量描述.
    *
    * @var string
    */
    const THIS_IS_A_CONST = '';

    /**
    * 属性描述.
    *
    * @var string
    */
    public $nameTest = '';

    /**
    * 构造函数.
    *
    * 构造函数描述
    * @author name <email>
    * @param  string $value 形参名称/描述
    * @return 返回值类型        返回值描述
    * 返回值类型：string，array，object，mixed（多种，不确定的），void（无返回值）
    */
    public function __construct($value = '')
    {
        // coding...
    }
```

- api方法提供测试样例example

```php
/**
 * 成员方法名称.
 *
 * 成员方法描述
 *
 * @param  string $value 形参名称/描述
 *
 * @example domain/api/controller/action?argu1=111&argu2=222
 */
public function testFunction($value = '')
{
    // code...
}
```

- 使用try...catch...

```php
try {

    // coding...

} catch (\Exception $e) {
  // coding...
}

```

- 连续调用多个方法(大于3个)使用foreach

```php
// 改写doSome为doSomething
class StandardExample
{
    /**
     * 方法列表
     *
     * @var array
     */
    private $functionList = [];

    public function __construct($functionList = array())
    {
        $this->functionList = $value;
    }

    public function doSome()
    {
        $this->functionOne();
        $this->functionTwo();
        $this->functionThree();
        $this->functionFour();
    }

    public function doSomething()
    {
        foreach($this->functionList as $function) {
            $this->$function();
        }
    }

    ...
}
```

- 文件顶部进行版权声明

```php
// +----------------------------------------------------------------------
// | Company Name  xx服务
// +----------------------------------------------------------------------
// | Copyright (c) 2021 http://domain All rights reserved.
// +----------------------------------------------------------------------
// | Author: name <email>
// +----------------------------------------------------------------------
```
## 编码实例

附编码实例文件: [standard.php](./standard.php)

```php
<?php
/**
 * 符合psr-1,2的编程实例
 *
 * @author cizel
 */

namespace Standard; // 顶部命名空间
// 空一行
use Test\TestClass;//use引入类

/**
 * 类描述
 *
 * 类名必须大写开头驼峰.
 */
abstract class StandardExample // {}必须换行
{
    /**
     *  常量描述.
     *
     * @var string
     */
    const THIS_IS_A_CONST = ''; // 常量全部大写下划线分割

    /**
     * 属性描述.
     *
     * @var string
     */
    public $nameTest = ''; // 属性名称建议开头小写驼峰
    // 成员属性必须添加public（不能省略）， private, protected修饰符

    /**
     * 属性描述.
     *
     * @var string
     */
    private $privateNameTest = ''; // 类私有成员属性

    /**
     * 构造函数.
     *
     * 构造函数描述
     *
     * @param  string $value 形参名称/描述
     */
    public function __construct($value = '') //成员方法必须添加public（不能省略）， private, protected修饰符
    {// {}必须换行

        $this->nameTest = new TestClass();

        // 链式操作
        $this->nameTest->functionOne()
                       ->functionTwo()
                       ->functionThree();

        // 一段代码逻辑执行完毕 换行
        // code...
    }

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $value 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     * 返回值类型：string，array，object，mixed（多种，不确定的），void（无返回值）
     */
    public function testFunction($value = '') //成员方法必须小写开头驼峰
    {
        // code...
    }

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $value 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     */
    private function privateTestFunction($value = '') //私有成员方法
    {
        // code...
    }

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $value 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     */
    public static function staticFunction($value = '') //static位于修饰符之后
    {
        // code...
    }

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $value 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     */
    abstract public function abstractFunction($value = ''); //abstract位于修饰符之前

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $value 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     */
    final public function finalFunction($value = '')// final位于修饰符之前
    {
        // code...
    }

    /**
     * 成员方法名称.
     *
     * 成员方法描述
     *
     * @param  string $valueOne 形参名称/描述
     * @param  string $valueTwo 形参名称/描述
     * @param  string $valueThree 形参名称/描述
     * @param  string $valueFour 形参名称/描述
     * @param  string $valueFive 形参名称/描述
     * @param  string $valueSix 形参名称/描述
     *
     * @return 返回值类型        返回值描述
     */
    public function tooLangFunction(
        $valueOne   = '', // 变量命名小写开头驼峰
        $valueTwo   = '',
        $valueThree = '',
        $valueFour  = '',
        $valueFive  = '',
        $valueSix   = '') { // 参数过多换行
        if ($valueOne === $valueTwo) {// 控制结构=>后加空格,同{一行，（右边和)左边不加空格
            // code...
        }

        switch ($valueThree) {
        case 'value':
            // code...
            break;

        default:
            // code...
            break;
        }

        do {
            // code...
        } while ($valueFour <= 10);

        while ($valueFive <= 10) {
            // code...
        }

        for ($i=0; $i < $valueSix; $i++) {
            // code...
        }
    }
}
```
