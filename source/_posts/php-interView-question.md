---
title: php面试题
date: 2017-09-17 11:12:02
tags:
 - php
 - 面试题
categories:
 - 后端
---

## 1、基础


### 1.1 cookie与session的区别
- 存储位置：session存储于服务器，cookie存储于客户端
- 安全性：session安全性比cookie高
- 存放的形式：Session是以对象的形式保存在服务器，Cookie以字符串的形式保存在客户端
- 用途：Cookies适合做保存用户的个人设置,爱好等,Session适合做客户的身份验证
- session为'会话服务'，在使用时需要开启服务，cookie不需要开启，可以直接用

### 1.2 禁用 cookie 后 session 还能用吗?

可以,通过URL传值或者隐藏表单传递Session ID (常用)。

**Session和cookie的工作流程：**
1. 你第一次访问网站时，
 
2. 服务端脚本中开启了session_start();，
 
3. 服务器会生成一个不重复的 SESSIONID 的文件session_id();，比如在/var/lib/php/session目录
 
4. 并将返回(Response)如下的HTTP头 Set-Cookie:PHPSESSIONID=xxxxxxx
 
5. 客户端接收到`Set-Cookie`的头，将PHPSESSIONID写入cookie
 
6. 当你第二次访问页面时，所有Cookie会附带的请求头(Request)发送给服务器端
 
7. 服务器识别PHPSESSIONID这个cookie，然后去session目录查找对应session文件，
 
8. 找到这个session文件后，检查是否过期，如果没有过期，去读取Session文件中的配置；如果已经过期，清空其中的配置

### 1.3 表单中get与post提交方法的区别
```
参数接收：
 get：通过url参数传递进行接收,
 post：是实体数据,可以通过表单提交大量信息.
```

### 1.4 数据库中的事务是什么

- 事务（transaction）是作为一个单元的一组有序的数据库操作。
如果组中的所有操作都成功，则认为事务成功，即使只有一个操作失败，事务也不成功。
如果所有操作完成，事务则提交，其修改将作用于所有其他数据库进程。
如果一个操作失败，则事务将回滚，该事务所有操作的影响都将取消


### 1.5 echo(),print(),print_r()的区别 

- echo是PHP语句, print和print_r是函数,语句没有返回值,函数可以有返回值(即便没有用)

- print() 只能打印出简单类型变量的值(如int,string)

- print_r() 可以打印出复杂类型变量的值(如数组,对象)

- echo 输出一个或者多个字符串



### 1.6 用PHP写出显示客户端IP与服务器IP的代码

- 打印客户端IP:echo $_SERVER['REMOTE_ADDR']; 或者: getenv('REMOTE_ADDR');

- 打印服务器IP:echo gethostbyname("www.bolaiwu.com")


### 1.7 include和require的区别是什么?

- require:是无条件包含也就是如果一个流程里加入require,无论条件成立与否都会先执行require

- include:有返回值，而require没有(可能因为如此require的速度比include快)

><font color='red'>注意:包含文件不存在或者语法错误的时候,require是致命的,include不是</font>



### 1.8 Trait是什么?
Trait 是为类似 PHP 的单继承语言而准备的一种代码复用机制。Trait 为了减少单继承语言的限制，使开发人员能够自由地在不同层次结构内独立的类中复用 method。Trait 和 Class 组合的语义定义了一种减少复杂性的方式，避免传统多继承和 Mixin 类相关典型问题

单个Trait使用方法：
```php
<?php
trait ezcReflectionReturnInfo {
    function getReturnType() { /*1*/ }
    function getReturnDescription() { /*2*/ }
}

class ezcReflectionMethod extends ReflectionMethod {
    use ezcReflectionReturnInfo;
    /* ... */
}

class ezcReflectionFunction extends ReflectionFunction {
    use ezcReflectionReturnInfo;
    /* ... */
}
?>
```
多个Trait使用方法
```php
<?php
trait Hello {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait World {
    public function sayWorld() {
        echo 'World';
    }
}

class MyHelloWorld {
    use Hello, World;
    public function sayExclamationMark() {
        echo '!';
    }
}

$o = new MyHelloWorld();
$o->sayHello();
$o->sayWorld();
$o->sayExclamationMark();
?>

```


### 1.9 php7和php5区别

1.PHP7.0 比PHP5.6性能提升了两倍。
1). 变量存储字节减小，减少内存占用，提升变量操作速度

2). 改善数组结构，数组元素和hash映射表被分配在同一块内存里，降低了内存占用、提升了 cpu 缓存命中率

3). 改进了函数的调用机制，通过优化参数传递的环节，减少了一些指令，提高执行效率

2.PHP7.0全面一致支持64位。

3.PHP7.0之前出现的致命错误，都改成了抛出异常。

4.增加了空结合操作符（？？）。效果相当于三元运算符。

5.PHP7.0新增了函数的返回类型声明。

6.PHP7.0新增了标量类型声明。
`PHP 7 中的函数的形参类型声明可以是标量。在 PHP 5 中只可以是类名、接口、array 或者 callable (PHP 5.4，即可以是函数，包括匿名函数)，现在也可以使用 string、int、float和 bool 了。`


7.新增加了匿名类。

`PHP 5.3 开始有了匿名函数，现在又新增了匿名类；`

8.PHP7.0之后溢移除了一些老的不再支持的SAPI(服务器端应用编程端口)和扩展。
``` 
 ereg
    
 mssql
    
 mysql
    
 sybase_ct
```
    

9.define 现在可以定义常量数组。


### 1.10 谈谈对mvc的认识
`模型(model),视图(view),控制器(controller);
由模型发出要实现的功能到控制器,控制器接收组织功能传递给视图;`

### 1.11 请说明php中传值与传引用的区别。什么时候传值什么时候传引用?

- 按值传递：函数范围内对值的任何改变在函数外部都会被忽略

- 按引用传递：函数范围内对值的任何改变在函数外部也能反映出这些修改

> 优缺点：按值传递时，php必须复制值。特别是对于大型的字符串和对象来说，这将会是一个代价很大的操作。
按引用传递则不需要复制值，对于性能提高很有好处

### 1.12 从一个标准url 里取出文件的扩展名
例如: http://www.sina.com.cn/abc/de/fg.php?id=1 需要取出php 或.php

```php
function getExt($url){
   $arr = parse_url($url);
   $file = basename($arr['path']);
   $ext = explode(".",$file);
   return $ext[1];
}
```
### 1.13 字符串反转
```php
function strrev($str){
  $len = mb_strlen($str);
  $tmp=[];
  for ($i=0;$i<$len;$i++){
      $tmp[] = mb_substr($str,$i,1);
  }
  krsort($tmp);
  return implode('',$tmp);
}
```
> strrev函数对英文很好用,直接可以实现字符串翻转,但是面对中文会出现乱码

### 1.14 在HTTP 1.0中，状态码401 的含义是? 如果返回 "找不到文件" 的提示，则可用header函数，其语句为?
```
答：401表示未授权; header("HTTP/1.0 404 Not Found");

```

### 1.15 isset、empty和is_null的区别?
- isset 判断变量是否已存在，如果变量存在则返回 TRUE，否则返回 FALSE。
- empty 判断变量是否为空，如果变量是非空或非零的值，则empty() 返回 FALSE。换句话说，"" 、0 、"0"、NULL 、FALSE 、array() 、var $var; 以及没有任何属性的对象都将被认为是空的，如果变量为空，则返回TRUE。　　
- is_null 判断变量是否为NULL

### 1.16 self、static、$this 的区别?
- self 和 \_\_CLASS__，都是对当前类的ip静态引用，取决于定义当前方法所在的类。也就是说，self写在哪个类里面,它引用的就是谁。
- $this 指向的是实际调用时的对象，也就是说，实际运行过程中，谁调用了类的属性或方法，$this 指向的就是哪个对象。但 $this 不能访问类的静态属性和常量，且 $this 不能存在于静态方法中。
- static 关键字除了可以声明类的静态成员（属性和方法）外，还有一个非常重要的作用就是后期静态绑定。
- self可以用于访问类的静态属性、静态方法和常量，但self指向的是当前定义所在的类，这是 self 的限制。
- $this 指向的对象所属的类和 static 指向的类相同。
- static 可以用于静态或非静态方法中，也可以访问类的静态属性、静态方法、常量和非静态方法，但不能访问非静态属性。
- 静态调用时，static 指向的是实际调用时的类；非静态调用时，static 指向的是实际调用时的对象所属的类。

### 1.17 单引号'与双引号"区别
  php里的单引号把内容当成纯文本，不会经过服务器翻译。而双引号则与此相反。里面的内容会经过服务器处理(process)；

```php
$foo="data";
echo '$foo'; //单引号输出$foo
echo "$foo"; //双引号输出data
```

### 1.18 如果理解OOP？
OOP(object oriented programming)，即面向对象编程，其中两个最重要的概念就是类和对象，类只是具备了某些功能和属性的抽象模型，而实际应用中需要一个一个实体，也就是需要对类进行实例化，类在实例化之后就是对象。

`OOP具有三大特点：`
- 封装性：
>将一个类的使用和实现分开,只保留部分接口和方法供外部使用，所以开发人员只需要关注这个类如何使用，而不用去关心其具体的实现过程。
- 继承性：
> 子类自动继承其父级类中的属性和方法,并可以添加新的属性和方法或者对部分属性和方法进行重写。继承增加了代码的可重用性。 php只支持单继承，也就是说一个子类只能有一个父类。
- 多态性：
> 继承了来自父级类中的属性和方法，并对其中部分方法进行重写,于是多个子类中虽然都具有同一个方法，但是这些子类实例化的对象调用这些相同的方法后却可以获得完全不同的结果，这种技术就是多态性。多态性增强了软件的灵活性。

### 1.19 PHP缓存技术有哪些 ？
- 全页面静态化缓存，也就是将页面全部生成html静态页面，用户访问时直接访问的静态页面，而不会去走php服务器解析的流程
- 页面部分缓存，将一个页面中不经常变的部分进行静态缓存，而经常变化的块不缓存，最后组装在一起显示
- 数据缓存，通过一个id进行请求的数据,将数据缓存到一个php文件中,id和文件是对应的,下次通过这个id进行请求时 直接读php文件
- 查询缓存，和数据缓存差不多,根据查询语句进行缓存;

### 1.20 接口和抽象类的区别是什么？
- 抽象类是一种不能被实例化的类，只能作为其他类的父类来使用。
- <font color="red">抽象类是通过关键字 abstract 来声明的</font>。
- 抽象类与普通类相似，都包含成员变量和成员方法，两者的区别在于，抽象类中至少要包含一个抽象方法，
- 抽象方法没有方法体，该方法天生就是要被子类重写的。
- 抽象方法的格式为：abstract function abstractMethod();
- 因为php中只支持单继承，如果想实现多重继承，就要使用接口。也就是说子类可以实现多个接口。
- <font color="red">接口是通过interface关键字来声明的，接口中的成员常量和方法都是public的，方法可以不写关键字public</font>，
- 接口中的方法也是没有方法体。接口中的方法也天生就是要被子类实现的。
- <font color="red">抽象类和接口实现的功能十分相似，最大的不同是接口能实现多继承。在应用中选择抽象类还是接口要看具体实现</font>。
- <font color="red">子类继承抽象类使用extends，子类实现接口使用implements</font>。

### 1.21 常见 HTTP 状态码，分别代表什么含义
- 200:请求成功
- 206:部分内容
> 服务器已经成功处理了部分GET请求。类似于FlashGet或者迅雷这类的HTTP 下载工具，都是使用此类响应实现断点续传，或者将一个大文档分解为多个下载段同时下载。
- 301:永久重定向
- 302:临时重定向
- 400:错误请求
- 401:未经授权
- 403:禁止访问
- 404:文件未找到
- 500:内部服务器错误
- 502:无效网关

### 1.22 计算两个日期相隔多少年，多少月，多少天，多少小时，多少分钟，多少秒

```php
<?php
/**
 * function：计算两个日期相隔多少年，多少月，多少天，多少小时，多少分钟，多少秒
 * param string $date1[格式如：2011-11-5]
 * param string $date2[格式如：2012-12-01]
 * return array array('年','月','日');
 */
function diffDate($date1,$date2)
{
    $datetime1 = new \DateTime($date1);
    $datetime2 = new \DateTime($date2);
    $interval = $datetime1->diff($datetime2);
    $time['y']         = $interval->format('%Y');
    $time['m']         = $interval->format('%m');
    $time['d']         = $interval->format('%d');
    $time['h']         = $interval->format('%H');
    $time['i']         = $interval->format('%i');
    $time['s']         = $interval->format('%s');
    return $time;
}


# 使用实例
$sss = diffDate('2018-12-25 12:30:30', '2018-12-26 15:00:00');
print_r($sss);

```
输出结果：
```
Array
(
    [y] => 00
    [m] => 0
    [d] => 1
    [h] => 02
    [i] => 29
    [s] => 30
)

```
### 1.23 长连接、短连接的区别和使用

涵义说明:
- 长连接：client方与server方先建立连接，连接建立后不断开，然后再进行报文发送和接收。这种方式下由于通讯连接一直存在。此种方式常用于P2P通信。
- 短连接：Client方与server每进行一次报文收发交易时才进行通讯连接，交易完毕后立即断开连接。此方式常用于一点对多点通讯。C/S通信。


使用时机:
- 长连接使用场景:
短连接多用于操作频繁，点对点的通讯，而且连接数不能太多的情况。每个TCP连 接的建立都需要三次握手，每个TCP连接的断开要四次握手。如果每次操作都要建立连接然后再操作的话处理速度会降低，所以每次操作下次操作时直接发送数据 就可以了，不用再建立TCP连接。例如：数据库的连接用长连接，如果用短连接频繁的通信会造成socket错误，频繁的socket创建也是对资源的浪费。

- 短连接使用场景:
web网站的http服务一般都用短连接。因为长连接对于服务器来说要耗费一定 的资源。像web网站这么频繁的成千上万甚至上亿客户端的连接用短连接更省一些资源。试想如果都用长连接，而且同时用成千上万的用户，每个用户都占有一个 连接的话，可想而知服务器的压力有多大。所以并发量大，但是每个用户又不需频繁操作的情况下需要短连接。


### 1.24 如何防止盗链？

- 不定期更名文件或者目录

- 加入水印
- 限制引用页
原理:服务器获取用户提交信息的网站地址，然后和真正的服务端的地址相比较， 如果一致则表明是站内提交，或者为自己信任的站点提交，否则视为盗链。实现时可以使用HTTP_REFERER 和htaccess 文件(需要启用mod_Rewrite)，结合正则表达式去匹配用户的每一个访问请求。

- 文件伪装
文件伪装是目前用得最多的一种反盗链技术，一般会结合服务器端动态脚本 (PHP/JSP/ASP)。实际上用户请求的文件地址，只是一个经过伪装的脚本文件，这个脚本文件会对用户的请求作认证,一般会检查 Session，Cookie 或HTTP_REFERER 作为判断是否为盗链的依据。而真实的文件实际隐藏在用户不能够访问的地方，只有用户通过验证以后才会返回给用户

- 加密认证
这种反盗链方式，先从客户端获取用户信息，然后根据这个信息和用户请求的文件名 字一起加密成字符串(Session ID)作为身份验证。只有当认证成功以后，服务端才会把用户需要的文件传送给客户。一般我们会把加密的Session ID 作为URL 参数的一部分传递给服务器，由于这个Session ID 和用户的信息挂钩，所以别人就算是盗取了链接，该Session ID 也无法通过身份认证，从而达到反盗链的目的。这种方式对于分布式盗链非常有效。

- 随机附加码
每次,在页面里生成一个附加码,并存在数据库里,和对应的图片相关,访问图片时和此附加码对比,相同则输出图片,否则输出404图片

>[查看阿里云防盗链方案](https://www.alibabacloud.com/help/zh/doc-detail/31937.htm)

## 2、进阶
### 2.1 yield 是什么，说个使用场景
- yield是生成器函数的核心关键字，
- 使用场景：协程可以用在，异步网络 IO 的时候，使其成为非阻塞的，

使用示例:
```
<?php
header("content-type:text/html;charset=utf-8");
function readTxt()
{
    # code...
    $handle = fopen("./test.txt", 'rb');

    while (feof($handle)===false) {
        # code...
        yield fgets($handle);
    }

    fclose($handle);
}

foreach (readTxt() as $key => $value) {
    # code...
    echo $value.'<br/>';
}
```

>使用生成器读取文件，第一次读取了第一行，第二次读取了第二行，以此类推，每次被加载到内存中的文字只有一行，大大的减小了内存的使用


[在PHP中使用协程实现多任务调度](http://www.laruence.com/2015/05/28/3038.html)

### 2.2 session共享方案

- 搭建redis集群或者memcached集群，用集群自带的同步方法来帮我们在不同的主机中同步session，这样就相当于把原来的一份session变成了N分session，session的同步就依赖于NoSql集群的同步了。

- 单独设置一个session服务器，负载服务器得到一个sessionid过后，去session服务器获得会话状态，然后根据状态来响应用户请求，如果会话状态为空，则在session服务器中设置一个会话状态，然后返回给用户一个sessionid。

### 2.3 php7.2 为什么弃用__autoload
`自动加载的原理，就是在我们new一个class的时候，PHP系统如果找不到你这个类，就会去自动调用本文件中的__autoload($class_name)方法，我们new的这个class_name 就成为这个方法的参数。所以我们就可以在这个方法中根据我们需要new class_name的各种判断和划分就去require对应的路径类文件，从而实现自动加载。`

**弃用原因**:因是PHP不允许函数重名，所以一个项目中仅能出现一个__autoload函数。自己写的代码保证只有一个__autoload函数虽然有点难但也能做到，要是第三方库也定义了__autoload，那就很头疼了。__autoload的后继者是[spl_autoload_register](http://php.net/manual/zh/function.spl-autoload-register.php)函数


### 2.4 Zval结构

- PHP5 Zval结构
```
struct _zval_struct {
     union {
          long lval;
          double dval;
          struct {
               char *val;
               int len;
          } str;
          HashTable *ht;
          zend_object_value obj;
          zend_ast *ast;
     } value;
     zend_uint refcount__gc;
     zend_uchar type;
     zend_uchar is_ref__gc;
};
```
** <font color='blue' >PHP5 Zval结构存在的问题:</font> **
- 整个结构体的大小是(在64位系统)24个字节, 而zval.value联合体中zend_object_value是最大的长板, 它导致整个value需要16个字节。
- 结构体中的每一个字段都有明确的定义, 没有预留任何的自定义字段,在存储一些和zval相关的信息的时候，不得不采用其他方式来扩充zval
- PHP的zval大部分都是按值传递,但有俩个例外(对象和资源),它们永远都是按引用传递，这样就造成一个问题, 对象和资源在除了zval中的引用计数以外, 还需要一个全局的引用计数, 这样才能保证内存可以回收。`在PHP5的时代, 以对象为例, 它有俩套引用计数, 一个是zval中的, 另外一个是obj自身的计数。非此之外，需要多次读取内存, 才能获取到真正的objec对象本身`
- PHP5的时代调用MAKE_STD_ZVAL会在堆内存上分配一个临时变量



- PHP7 Zval结构
``` 
struct _zval_struct {
     union {
          zend_long         lval;             /* long value */
          double            dval;             /* double value */
          zend_refcounted  *counted;
          zend_string      *str;
          zend_array       *arr;
          zend_object      *obj;
          zend_resource    *res;
          zend_reference   *ref;
          zend_ast_ref     *ast;
          zval             *zv;
          void             *ptr;
          zend_class_entry *ce;
          zend_function    *func;
          struct {
               uint32_t w1;
               uint32_t w2;
          } ww;
     } value;
    union {
        struct {
            ZEND_ENDIAN_LOHI_4(
                zend_uchar    type,         /* active type */
                zend_uchar    type_flags,
                zend_uchar    const_flags,
                zend_uchar    reserved)     /* call info for EX(This) */
        } v;
        uint32_t type_info;
    } u1;
    union {
        uint32_t     var_flags;
        uint32_t     next;                 /* hash collision chain */
        uint32_t     cache_slot;           /* literal cache slot */
        uint32_t     lineno;               /* line number (for ast nodes) */
        uint32_t     num_args;             /* arguments number for EX(This) */
        uint32_t     fe_pos;               /* foreach position */
        uint32_t     fe_iter_idx;          /* foreach iterator index */
    } u2;
};
```
** <font color='blue' >PHP7对Zval结构优化:</font> **
- 整个结构体内部都是联合体，这个新的zval在64位环境下,现在只需要16个字节(2个指针size), 它主要分为俩个部分, value和扩充字段, 而扩充字段又分为u1和u2俩个部分, 其中u1是type info, u2是各种辅助字段.
- 在PHP7中，移除了MAKE_STD_ZVAL/ALLOC_ZVAL宏, 不再支持存堆内存上申请zval. 函数内部使用的zval要么来自外面输入, 要么使用在栈上分配的临时zval.
- 抽象的来说, PHP7中的zval, 已经变成了一个值指针, 要么保存着原始值, 要么保存着指向一个保存原始值的指针. 也就是说现在的zval相当于PHP5的时候的zval*. 只不过相比于zval*, 直接存储zval, 我们可以省掉一次指针解引用, 从而提高缓存友好性.

[深入理解PHP7内核之zval](http://www.laruence.com/2018/04/08/3170.html)

## 3、数据库

### 3.1 Mysql的存储引擎,MyISAM和InnoDB的区别。 
- MyISAM类型不支持事务处理等高级处理，而InnoDB类型支持.
- MyISAM类型的表强调的是性能，其执行数度比InnoDB类型更快.
- InnoDB不支持FULLTEXT(全文索引) 类型的索引.
- InnoDB 中不保存表的具体行数.
> 也就是说执行 ：select count(*) from table时，
   InnoDB要扫描一遍整个表来计算有多少行，但是MyISAM只要简单的读出保存好的行数即可.
 
- 对于AUTO_INCREMENT(递增)类型的字段，InnoDB中必须包含只有该字段的索引，但是在MyISAM表中，可以和其他字段一起建立联合索引。
- DELETE FROM table时，InnoDB不会重新建立表，而是一行一行的删除。
- Load Table From Master 操作对InnoDB是不起作用的，解决方法是首先把InnoDB表改成MyISAM表，导入数据后再改成InnoDB表，但是对于使用的额外的InnoDB特性(例如外键)的表不适用.
- MyISAM支持表锁，InnoDB支持行锁。


### 3.2 delete drop truncate区别

- truncate 和 delete只删除数据，不删除表结构 ,drop删除表结构，并且释放所占的空间。
- 删除数据的速度，一般来说: drop> truncate > delete
- delete属于DML语言，需要事务管理，commit之后才能生效。drop和truncate属于DDL语言，操作立刻生效，不可回滚

**使用场合**：
  1.当你不再需要该表时， 用drop;
  2.当你仍要保留该表，但要删除所有记录时， 用truncate;
  3.当你要删除部分记录时（always with a where clause), 用 delete.

> 对于有主外键关系的表，不能使用truncate而应该使用不带where子句的delete语句，由于truncate不记录在日志中，不能够激活触发器

### 3.3 优化MYSQL数据库的方法
 - 选取最适用的字段属性,尽可能减少定义字段长度,尽量把字段设置NOT NULL,例如'省份,性别',最好设置为ENUM
 - 使用连接（JOIN）来代替子查询
 ``` 
  a.删除没有任何订单客户:
  DELETE FROM customerinfo WHERE customerid NOT in(SELECT customerid FROM orderinfo)
  
  b.提取所有没有订单客户:
  SELECT FROM customerinfo WHERE customerid NOT in(SELECT customerid FROM orderinfo)
   
  c.提高b的速度优化:
  SELECT FROM customerinfo LEFT JOIN orderid customerinfo.customerid=orderinfo.customerid WHERE orderinfo.customerid IS NULL
 ```
 - 使用联合(UNION)来代替手动创建的临时表
 ``` 
SELECT name FROM `nametest` UNION SELECT username FROM `nametest2`
 ```
 - 事务处理
 >保证数据完整性,例如添加和修改同时,两者成立则都执行,一者失败都失败。
 
 - 锁定表,优化事务处理
 
  `我们用一个 SELECT 语句取出初始数据，通过一些计算，用 UPDATE 语句将新值更新到表中。包含有 WRITE 关键字的 LOCK TABLE 语句可以保证在 UNLOCK TABLES 命令被执行之前，不会有其它的访问来对 inventory 进行插入、更新或者删除的操作.`
 ``` 
mysql_query("LOCK TABLE customerinfo READ, orderinfo WRITE");
mysql_query("SELECT customerid FROM `customerinfo` where id=".$id);
mysql_query("UPDATE `orderinfo` SET ordertitle='$title' where customerid=".$id);
mysql_query("UNLOCK TABLES");
 ```
 - 建立索引
 [索引类型,请看3.7](https://mrliuqh.github.io/2017/09/17/php-interView-question/#3-7-索引类型)
 - 优化查询语句
   - 避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。
   - 避免全表扫描，首先应考虑在where及order by涉及的列上建立索引
   - 避免在 where子句中对字段进行null值判断，会引起全表扫描
   ``` 
   如：select id from test where num is null
   ```
   > <font color='red'>因此字段都应设置为NOT NULL，将来查询的时候就不用去比较NULL值</font>
   
   - 避免在where子句中使用or来连接条件，会引起全表扫描
   `
   如：select id from t where num=10 or num=20
   可以这样查询：
   select id from t where num=10 union all select id from t where num=20
   `
   - in 和 not in 也要慎用，否则会导致全表扫描
   - 避免在 where 子句中对字段进行表达式操作

   `如：select id from t where num/2=100
   应改为：
   select id from t where num =100*2`

   - 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，一个表的索引数最好不要超过6个。
   
   - 任何地方都不要使用 select \* from t ，用具体的字段列表代替"\*”，不要返回用不到的任何字段。
   
   - 通过explain查询和分析SQL的执行计划
 ![image](https://mrliuqh.github.io/directionsImg/mysql/explain.png)
 
 
 ### 3.4 mysql_fetch_row() 和 mysql_fetch_array() 有什么分别？
 ```  
 mysql_fetch_row()
  返回的结果集是索引数组。
 mysql_fetch_assoc()
   返回的结果集是关联数组
 mysql_fetch_array()
  既可以返回索引数组也可以返回关联数组，
  取决于它的第二个参数 MYSQL_BOTH MYSQL_NUM MYSQL_ASSOC 默认为MYSQL_BOTH
 
 ```
 
 ### 3.5 php访问数据库有哪几步?
 
 1.连接数据库服务器：
 `mysql_connect('数据库服务器的主机名或ip','数据库服务器的用户名','数据库服务器的密码');
 ` 
 2.选择数据库：
 ` 
 mysql_select_db(数据库名);
 `
 3.设置从数据库提取数据的字符集：
 ` 
 mysql_query("set names utf8");
 `
 4.执行sql语句：
 `
 mysql_query(sql语句);
 `
 5.关闭结果集，释放资源：
 ` 
 mysql_free_result($result);
 `
 6.关闭与数据库服务器的连接：
 `
 mysql_close($link);
 `
 
 ### 3.6 表设计三大范式
 
- 1．第一范式(原子性):所有字段值都是不可分解的原子值       
- 2．第二范式(在第一范式的基础上):确保表中的每列都和主键相关，即一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中
- 3．第三范式(在第二范式的基础上):确保每列都和主键列直接关联,而不是间接相关

### 3.7 索引类型

- 普通索引(index):
`
创建:
  CREATE INDEX <索引名> ON tablename (索引字段)
修改:
  ALTER TABLE tablename ADD INDEX [索引名] (索引字段)
创表指定索引:
  CREATE TABLE tablename([...],INDEX[索引名](索引字段))
`

- 唯一索引(unique):
<font color='red'>在普通索引的基础上，会进行排除重复值</font>
`
创建:
  CREATE UNIQUE <索引名> ON tablename (索引字段)
修改:
  ALTER TABLE tablename ADD UNIQUE [索引名] (索引字段)
创表指定索引:
   CREATE TABLE tablename([...],UNIQUE[索引名](索引字段)) 
`

- 主键(primary key):
<font color='red'>和唯一索引的区别在于一个表里只能有一个主键索引，但是唯一索引可以有多个</font>
``` 
 它是唯一索引,一般在创建表是建立
 语法：
  CREATA TABLE tablename ([...],PRIMARY KEY[索引字段])
```

- 联合索引:
`
语法：
ALTER TABLE table_name ADD INDEX index_name ( column1, column2, column3 )
`

- 全文索引 (fulltext)

`普通索引／唯一索引／主键索引 哪个速度更快？`
`
速度是一样的快，因为三者都是采用btree二叉树算法进行查找。
`

### 3.8 索引算法
- BTREE算法
>Innodb和MyISAM默认的索引是BTREE索引
 采用二叉树算法，左边的树枝小于根节点关键词，右边大于根节点，两边的树的深度不大于1，从而降低时间复杂度。
 
- HASH算法
>Mermory默认的索引是Hash索引
 Hash索引只能用于HASH值比较，例如=,<> 操作符，不像BTREE索引需要从根节点到枝节点，最后才能访问到页节点这样多次IO访问，所以检索效率远高于BTREE索引。
 
- <font>为什么不默认采用HASH索引呢？</font>
> HASH只能用在=和<>上，所以功能受限，所以默认采用BTREE。


### 3.9 insert 和 replace的区别
replace into 跟 insert 功能类似，不同点在于：replace into 首先尝试插入数据到表中， 1. 如果发现表中已经有此行数据（根据主键或者唯一索引判断）则先删除此行数据，然后插入新的数据。 2. 否则，直接插入新数据。

> <font color='red'>要注意的是：插入数据的表必须有主键或者是唯一索引！否则的话，replace into 会直接插入数据，这将导致表中出现重复的数据。</font>


- **MySQL replace into 有三种形式：**

```
replace into tbl_name(col_name, ...) values(...)

replace into tbl_name(col_name, ...) select ...

replace into tbl_name set col_name=value, ...
```

>前两种形式用的多些。其中 "into” 关键字可以省略，不过最好加上 "into”，这样意思更加直观。另外，对于那些没有给予值的列，MySQL 将自动为这些列赋上默认值


### 3.10 数据库操作事务的四大特性
#### 3.10.1 事务操作数据库的四大特性(ACID)

- 原子性 (Atomicity):就是事务的所包含的所有操作，要么全部成功，要么全部失败回滚。
- 一致性 (Consistency):简单来说就是在事务执行前和执行后，必须保持数据的一致。
- 隔离性 (Isolation):一个事务执行的过程当中，不能被其他的事务干扰。
  >比如有事务A和事务B，相对于A来说，B想要执行，要么在我执行之前执行，要么在我执行完毕之后，你再开始执行.
 
- 持久性 (Durability):事务被提交之后，他就被永久的存储到了数据库当中.
  
#### 3.10.2 不考虑事务的隔离性所引发的问题
- 脏读:一个事务读取到了一个未提交的事务的数据。
- 不可重复读:
在读取数据库的某条数据的时候返回了不同的值，造成这个结果的原因是因为我们在查询了一次之后准备进行第二次查询的这个间隔之间，对我们要进行查询的这条数据进行了修改操作，从而导致两次读取的数据不一致。
  
 脏读和不可重复读的区别:脏读是一个事务读取到了一个未提交事务的脏数据，而不可重复读是一个数据读取了一个已经提交了的事务的数据。
  
- 虚读(幻读)
出现幻读不是对一条数据的操作而产生的问题，而是操作多条数据产生的问题，例如：事务A想要对一张表中的某一字段的值进行修改，假设有一个字段的值全部为1，事务A现在想要将1全部修改为2，在提交事务之后，事务B接着又进行了一个操作，在这张表中添加了一个字段，值全部为1。那么这时候操作事务A的用户在查看的时候，会发现还有一行数据没有进行修改，其实这是事务B在他查看之前添加的。
  
 `幻读和不可重复读都是读取了一个已经提交的事务，而脏读是读取了一个未提交的事务。不同的是不可重复读查询的是同一条数据，而虚读查询的是批量数据。`
 
### 3.11 事务的四种隔离级别
- Serializable (序列化)：可避免脏读、不可重读读、幻读的发生
- Repeatable-read (可重复读)：可避免脏读、不可重复读的发生。
- Read-committed (读已提交)：可避免脏读的发生。
- Read-uncommitted (读未提交)：最低级别，任何情况都无法保证。
`以上四种的隔离级别最高的Serializable，最低的是Read uncommitted，级别越高，虽然安全级别越高，但是执行的效率就越低，MySQL中默认的隔离级别是:Repeatable read(可重复读)，oracle默认的隔离级别是：Read committed(读已提交)。`
  
> <font color='red'>这里需要注意的是，mysql支持以上四种隔离级别，但是oracle只支持Serializable(串行化)和Read committed(读已提交)这两种隔离级别。</font>
  
- MySQL中查看当前的事务隔离界别
```mysql
select @@tx_isolation
```
- 设置mysql的隔离级别
```mysql
set tx_isolation='read-uncommitted'
```
><font color='red'>记住:设置数据库的隔离级别一定要是在开启事务之前！</font>

> 隔离级别的设置只对当前的链接有效。对于MySQL窗口来说，一个窗口就是一个链接，当前设置的事务隔离级别只对当前的窗口有效。
  

### 3.12 CHAR和VARCHAR的区别

#### 3.12.1 存储方式
- 当值存储在CHAR字段中时，<font color='red'>剩余的字符将用空格填充;
> 例如，一个字段是 name char(5)，并且您要存储只是"tom"，则实际值将存储为"tom  "
- 与CHAR不同，VARCHAR只占用基于存储的数据的空间
- varchar类型的实际长度是它的值的实际长度+1，这一个(也可能是两)字节用于保存实际使用了多大的长度
- char的存储方式是：英文字符占1个字节，汉字占用2个字节；varchar的存储方式是：英文和汉字都占用2个字节，两者的存储数据都非unicode的字符数据

#### 3.12.2 数据检索
- 如果CHAR字段的数据较短时，会经过空格填充，所以查询出来的结果需删除尾随空格。

#### 3.12.3 存储上限
- char(n)，n最大255。
- varchar(n)，n最大65535，另外，按照字符集，不能超过65525字节。这65535字节不能全用来存数据，因为有1-2字节要用来存占用长度，255字节以下用1字节存储长度，255字节以上用2字节存储长度。
- text，上限65535字节，再多也能存，因为还有mediumtext上限2^24-3字节大概16m，longtext上限2^32-4字节大概4G。


#### 3.12.4 性能对比
> 按照查询速度： char最快， varchar次之，text最慢
- char，定长，基本没有碎片，索引速度极快。
- varchar，不定长，索引速度没有char快。理论上可以添加全部索引，但是数据长度太大时索引也会截取数据前面的一部分。
- text，不定长，速度慢，索引只能是前缀索引。


#### 3.12.5 不同存储引擎对 CHAR 和 VARCHAR 的使用
- MyISAM 存储引擎:建议使用固定长度的数据列代替可变长度的数据列。

- MEMORY 存储引擎:目前都使用固定长度的数据行存储，因此无论使用 CHAR 或 VARCHAR 列都没有关系。两者都是作为 CHAR 类型处理。
- InnoDB 存储引擎:建议使用 VARCHAR 类型。对于 InnoDB 数据表，内部的行存储格式没有区分固定长度和可变长度列(所有数据行都使用指向数据列值的头指针)，因此在 本质上，使用固定长度的 CHAR 列不一定比使用可变长度 VARCHAR 列性能要好。因而，主要的性能因素是数据行使用的存储总量。由于 CHAR 平均占用的空间多于 VARCHAR，因此使 用 VARCHAR 来最小化需要处理的数据行的存储总量和磁盘 I/O 是比较好的。


### 3.13 如何理解超键、候选键、主键、外键

- 主键(Primary Key)：对数据库表中的每一行数据进行唯一标识。

  - 任意两行的主键值都不同
  - 包含主键值的列从不修改或更新
  - 主键值不能重用
  - 使用PRIMARY KEY进行标识

- 外键(foreign key)：是表中的一列，其值必须在另一个表的主键中。

- 超键(Super Key)：在关系中能惟一标识元组(数据库中的一条记录)的属性集称为关系模式的超键。 
>比如一张学生信息表，学生表中含有学号或者身份证号的任意组合都为此表的超键。如：（学号）、（学号，姓名）、（身份证号，性别）等。

- 候选键(Candidate Key)：不含有多余属性的超键称为候选键。也就是在候选键中，若要再删除属性，就不能唯一标识元组了。

> 如：学生表中的候选键为：（学号）、（身份证号）。



### 3.14  BLOB和TEXT有什么区别
- 二者之间的主要差别是 BLOB 能用来保存二进制数据（比如照片），而TEXT只能保存字符数据

- TEXT值是大小写不敏感的
- BLOB值进行排序和比较时区分大小写


### 3.15 NOW() 和CURRENT_DATE() 有什么区别

- NOW()  命令用于显示当前年份，月份，日期，小时，分钟和秒。

- CURRENT_DATE() 仅显示当前年份，月份和日期。






## 4、缓存

### 4.1 Memcache和Redis区别

- **数据类型**：都是k/v数据库，但memcache只支持string，redis除了string，还支持list，set，hash等数据
- **持久化**：memcache不支持内存持久化，redis支持。
- **内存管理**：memcache内存用完时，会删除用得最少的缓存；redis内存用完时，会把最少的缓存交换到磁盘里。

### 4.2 如何提高memcache的缓存命中率
- 合理组合缓存 Key，保证Key最大复用率。
- 合理设置过期时间，减少因为缓存数据过期后被穿透


## 5、服务器


### 5.1 Apache与Nginx的优缺点比较
1、nginx相对于apache的优点：
轻量级，比apache 占用更少的内存及资源。高度模块化的设计，编写模块相对简单抗并发，nginx处理请求是异步非阻塞，多个连接（万级别）可以对应一个进程，而apache 则是阻塞型的，是同步多进程模型，一个连接对应一个进程，在高并发下nginx 能保持低资源低消耗高性能。nginx处理静态文件好，Nginx 静态处理性能比 Apache 高 3倍以上
2、apache 相对于nginx 的优点：
apache 的rewrite 比nginx 的rewrite 强大 ，模块非常多，基本想到的都可以找到 ，比较稳定，少bug ，nginx的bug相对较多
3：Nginx比Apache快的原因：这得益于Nginx使用了最新的epoll（Linux 2.6内核）和kqueue（freebsd）网络I/O模型，而Apache则使用的是传统的select模型。

>目前Linux下能够承受高并发访问的 Squid、Memcached都采用的是epoll网络I/O模型。 处理大量的连接的读写，Apache所采用的select网络I/O模型非常低效。
### 5.2 fastcgi、cgi、php-fpm

- fastcgi和cgi的区别
`在web服务器方面在对数据进行处理的进程方面`:
a. cgi fork一个新的进程进行处理读取参数，处理数据，然后就结束生命期。
b. fastcgi 用tcp方式跟远程机子上的进程或本地进程建立连接要开启tcp端口，进入循环，等待数据的到来，处理数据。

- php-fpm的作用
`那PHP-FPM又是什么呢？它是一个实现了Fastcgi协议的程序,用来管理Fastcgi起的进程的,即能够调度php-cgi进程的程序。现已在PHP内核中就集成了PHP-FPM，使用--enalbe-fpm这个编译参数即可。另外，修改了php.ini配置文件后，没办法平滑重启，需要重启php-fpm才可。此时新fork的worker会用新的配置，已经存在的worker继续处理完手上的活`

> 举个例子: 服务端现在有个10万个字单词， 客户每次会发来一个字符串，问以这个字符串为前缀的单词有多少个。 那么可以写一个程序，这个程序会建一棵trie树，然后每次用户请求过来时可以直接到这个trie去查找。 但是如果以cgi的方式的话，这次请求结束后这课trie也就没了，等下次再启动该进程时，又要新建一棵trie树，这样的效率就太低下了。 而用fastcgi的方式的话，这课trie树在进程启动时建立，以后就可以直接在trie树上查询指定的前缀了


### 5.3 为什么使用独立文件服务器？

- 从服务器本身来说，单台的话会加大机器IO负载,多台(负载均衡)的话涉及到文件同步的问题
- 浏览器对一个域名下的并发是有数量限制的，独立域名的文件服务器会加快响应
- 防止域名污染，请求图片的时候是不用带上cookie
    
    
## 6、算法

### 6.1 写一个函数，算出两个文件的相对路径
```php
 function countOppose(){
   $arr1 = explode('/', $pathA);
   $arr2 = explode('/', $pathB);
   // 获取相同路径的部分
   $intersection = array_intersect_assoc($arr1, $arr2);
   $depth =count($intersection);
   
   // 将path2的/ 转为 ../，path1获取后面的部分，然后合拼
   // 计算前缀
   if (count($arr2) - $depth - 1 > 0) {
       $prefix = array_fill(0, count($arr2) - $depth - 1, '..');
   } else {
       $prefix = array('.');
   }

   $tmp = array_merge($prefix, array_slice($arr1, $depth));
   $relativePath = implode('/', $tmp);
   return $relativePath;
 }
```

### 6.2 [php排序算法汇总](https://mrliuqh.github.io/2018/01/12/php-sort/)

### 6.3 遍历一个文件夹下的所有文件和子文件夹

```php

function childForDir($dir)
{
    $files = [];
    if (!is_dir($dir)) {
        return $dir;
    }
    $handle = opendir($dir);
    if (!$handle) {
        return false;
    }

    //取出.和..
    readdir($handle);
    readdir($handle);
   
   //遍历剩余的文件和目录
    while ($file = readdir($handle)) {
        if (is_dir($file)) {
            $files[$file] = $this->childForDir($file);
        } else {
            $files[] = $dir . '/' . $file;
        }
    }
    closedir($handle);
    return $files;
}
```

### 6.4 猴子选大王
 一群猴子排成一圈，按1，2，…，n依次编号。然后从第1只开始数，数到第m只,把它踢出圈，从它后面再开始数，再数到第m只，在把它踢出去…，如此不停 的进行下去，直到最后只剩下一只猴子为止，那只猴子就叫做大王。要求编程模拟此过程，输入m、n, 输出最后那个大王的编号。

```php
<?php
function monkeyKingNum($allNum, $m){
  $arr = range(1,$allNum);
  $num = 1;
  while(count($arr) > 1){
      foreach ($arr as $key => $value) {
          if($num == $m){
              unset($arr[$key]);
              $num = 1;
          }else{
              $num++;
          }
      }
  }
  $monkeyKingNum = array_values($arr)[0];
  return $monkeyKingNum;
}
monkeyKingNum(10,10);
```
### 6.5 二分查找
`二分查找又称折半查找，优点是比较次数少，查找速度快，平均性能好;其缺点是要求待查表为有序表，且插入删除困难。
 因此，折半查找方法适用于不经常变动而查找频繁的有序列表。`

```php

<?php
/**
 * @param $data //待查找的元素数组
 * @param $min //开始元素的下标
 * @param $max //结束元素的下标
 * @param $k //待查找的元素
 * @return bool
 */
function binarySearch($data,$min,$max,$k){
    if ($min <= $max){
        //计算中间的元素下标
        $mid = intval(($min +$max)/2);
        if ($data[$mid] == $k){
            //如果相等,则找到
            return $mid;
        } else if ($k < $data[$mid]){
            //元素下标在前面一部分
            return binarySearch($data, $min, $mid-1, $k);
        } else {
            //元素下标在后面一部分
            return binarySearch($data, $mid+1, $max, $k);
        }
    }
    return false;
}

```


## 7、设计模式

### 7.1单例模式（三私一公）
>单例模式的用途,是对系统资源的节省, 可以避免重复实例化,而PHP每次执行完都会从内存中清理掉所有的资源. 因而PHP中的单例实际每次运行都是需要重新实例化的, 这样就失去了单例重复实例化的意义了. 单单从这个方面来说, PHP的单例的确有点让各位失望. 
 
 
>但是php的应用主要在于数据库应用, 所以一个应用中会存在大量的数据库操作, 在使用面向对象的方式开发时(废话), 如果使用单例模式, 则可以避免大量的new 操作消耗的资源。



```php
 class Test{
     //私有化后在类内部保存对象并且防止外部访问到
     private static $obj=null;
     
     //私有化后防止在外部创建新的对象
     private function __construct() {
     }
     
     //公有并且静态方法在类外面可以通过类名直接访问
     public static function getInstance(){
         if(self::$obj==null)
             self::$obj=new self();
         return self::$obj;
     }
     
     //私有化克隆执行的方法,防止在外部被克隆
     private function __clone(){
     }
 }
```

### 7.2工厂模式
工厂模式具体可分为四类：简单工厂，工厂方法，抽象工厂、静态工厂；


>简单工厂模式:静态工厂方法(Static Factory Method)模式，它属于类创建型模式。在简单工厂模式中，可以根据参数的不同,返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。。


示例代码：
```php
<?php
//简单工厂模式
class Cat
{
  function __construct()
  {
      echo "I am Cat class <br>";
  }
}
class Dog
{
  function __construct()
  {
      echo "I am Dog class <br>";
  }
}
class Factory
{
  public static function CreateAnimal($name){
      if ($name == 'cat') {
          return new Cat();
      } elseif ($name == 'dog') {
          return new Dog();
      }
  }
}

$cat = Factory::CreateAnimal('cat');
$dog = Factory::CreateAnimal('dog');
?>
```
> <font color=red>IUser 接口定义用户对象应执行什么操作。IUser 的实现称为 User，UserFactory 工厂类则创建 IUser 对象 </font>


- [工厂方法，查看详情](https://designpatternsphp.readthedocs.io/zh_CN/latest/Creational/FactoryMethod/README.html)

- [抽象工厂，查看详情](https://designpatternsphp.readthedocs.io/zh_CN/latest/Creational/AbstractFactory/README.html)

- [静态工厂，查看详情](https://designpatternsphp.readthedocs.io/zh_CN/latest/Creational/StaticFactory/README.html)

### 7.3 建造者模式（生成器模式）
又名：生成器模式，是一种对象构建模式。它可以将复杂对象的建造过程抽象出来（抽象类别），使这个抽象过程的不同实现方法可以构造出不同表现（属性）的对象。

>建造者模式是一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。例如，一辆汽车由轮子，发动机以及其他零件组成，对于普通人而言，我们使用的只是一辆完整的车，这时，我们需要加入一个构造者，让他帮我们把这些组件按序组装成为一辆完整的车

- Builder：抽象构造者类，为创建一个Product对象的各个部件指定抽象接口。
- ConcreteBuilder：具体构造者类，实现Builder的接口以构造和装配该产品的各个部件。定义并明确它所创建的表示。提供一个检索产品的接口
- Director：指挥者，构造一个使用Builder接口的对象。
- Product：表示被构造的复杂对象。ConcreateBuilder创建该产品的内部表示并定义它的装配过程。


 **示例代码**：
```php

<?php 
abstract class Builder
{
  protected $car;
  abstract public function buildPartA();
  abstract public function buildPartB();
  abstract public function buildPartC();
  abstract public function getResult();
}
class CarBuilder extends Builder
{
  function __construct()
  {
      $this->car = new Car();
  }
  public function buildPartA(){
      $this->car->setPartA('发动机');
  }
  public function buildPartB(){
      $this->car->setPartB('轮子');
  }
  public function buildPartC(){
      $this->car->setPartC('其他零件');
  }
  public function getResult(){
      return $this->car;
  }
}
class Car
{
  protected $partA;
  protected $partB;
  protected $partC;
  public function setPartA($str){
      $this->partA = $str;
  }
  public function setPartB($str){
      $this->partB = $str;
  }
  public function setPartC($str){
      $this->partC = $str;
  }
  public function show()
  {
      echo "这辆车由：".$this->partA.','.$this->partB.',和'.$this->partC.'组成';
  }
}
class Director
{
  public $myBuilder;

  public function startBuild()
  {
      $this->myBuilder->buildPartA();
      $this->myBuilder->buildPartB();
      $this->myBuilder->buildPartC();
      return $this->myBuilder->getResult();
  }

  public function setBuilder(Builder $builder)
  {
      $this->myBuilder = $builder;
  }
}
$carBuilder = new CarBuilder();
$director = new Director();
$director->setBuilder($carBuilder);
$newCar = $director->startBuild();
$newCar->show();
```


## 8、数据结构
### 8.1 堆、栈、队列的区别
- 堆
**堆中主要存放用new构造的对象和数组**
优势：可以动态的分配内存的大小，生存期也不必事先告诉编译器，因为它是在运行时动态分配内存的。
缺点：由于要在运行时动态分配内存，存取速度比较慢


- 栈
**栈中主要存放一些基本类型的变量和对象引用类型。**
优势：存取速度比较快，仅次于寄存器，栈数据可以共享。
缺点：栈中的数据大小和生存周期必须是确定的，缺乏灵活性。


- 队列
**设计程序中常用的一种数据结构，采用"先进先出”的存储结构，类似于队列。**
数据元素只能从队尾进入，从队首取出。在此队列中，数据元素可以随意增减，
但是数据元素的次序不会更改。每次都是取出队首的元素，后面的元素会整体向前移动一位。队列遍历数据的速度要快的多



### 8.2 什么是哈希表？
哈希表（Hash table，也叫散列表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

哈希表hashtable(key，value) 的做法其实很简单，就是把Key通过一个固定的算法函数既所谓的哈希函数转换成一个整型数字，然后就将该数字对数组长度进行取余，取余结果就当作数组的下标，将value存储在以该数字为下标的数组空间里。
     而当使用哈希表进行查询的时候，就是再次使用哈希函数将key转换为对应的数组下标，并定位到该空间获取value，如此一来，就可以充分利用到数组的定位性能进行数据定位




## 附加1、扩展

### 1. 写代码来解决多进程/线程同时读写一个文件的问题
```php
function write(){
    //打开文件
	$file = fopen('flock.text','w+');
	if(!$file){
	  return 'the file not exist!';
	}
	//获取锁
	if (flock(file,LOCK_EX)){
       //todo 
       fwrite(file,'do some things');
       //释放锁
       flock(file,LOCK_UN);
	} else {
      return 'the file is write...';
	}
	//关闭文件
	fclose(file);
}
```


### 2. 什么是写时复制
>  **写时复制（Copy on Write，也缩写为COW)的应用场景非常多， 比如Linux中对进程复制中内存使用的优化，在各种编程语言中，如C++的STL等等中均有类似的应用。 COW是常用的优化手段，可以归类于：资源延迟分配。只有在真正需要使用资源时才占用资源， 写时复制通常能减少资源的占用。**

在开始之前，我们可以先看一段简单的代码：

```php
<?php   //例一
    $foo = 1;
    $bar = $foo;
    echo $foo + $bar;
?>
```
> 执行这段代码，会打印出数字2。从内存的角度来分析一下这段代码"可能”是这样执行的： 分配一块内存给foo变量，里面存储一个1； 再分配一块内存给bar变量，也存一个1，最后计算出结果输出。 事实上，我们发现foo和bar变量因为值相同，完全可以使用同一块内存，这样，内存的使用就节省了一个1， 并且，还省去了分配内存和管理内存地址的计算开销。 没错，很多涉及到内存管理的系统，都实现了这种相同值共享内存的策略：写时复制

[详情参考](http://www.php-internals.com/book/?p=chapt06/06-06-copy-on-write)

### 3. echo (int) ( (0.1+0.7) * 10 ); 输出是多少？为什么?
``` 
输出的结果为：7 
```
>关于浮点数精度的警告
 显然简单的十进制分数如同 0.1 或 0.7 不能在不丢失一点点精度的情况下转换为内部二进制的格式，这就会造成混乱的结果。例如，floor((0.1+0.7)*10) 通常会返回 7 而不是预期中的 8，因为该结果内部的表示其实是类似 7.9。
 
>注意：永远不要相信浮点数结果精确到了最后一位，也永远不要比较两个浮点数是否相等




## 附加2、面试题链接
- https://github.com/hookover/php-engineer-interview-questions
- https://www.kancloud.cn/pingfan_/php_interview/916716
- https://www.kancloud.cn/i281151/php_questions/174233
- https://www.kancloud.cn/tp5girl/interview/329075
- https://github.com/wudi/PHP-Interview-Best-Practices-in-China
- https://my.oschina.net/anyeshe/blog/1550238
- https://www.jianshu.com/p/ac5cad6d64a8
- https://www.zhaoyafei.cn/content.html?id=150846575347
- https://www.cnblogs.com/zyf-zhaoyafei/p/4828358.html
- https://segmentfault.com/a/1190000010262869#articleHeader9