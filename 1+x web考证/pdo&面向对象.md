---
typora-root-url: images
---

# 1、pdo数据库操作

在PHP中可以使用mysqli整套扩展实现MySQL数据库的操作。但是mysqli扩展存在一定约束，只能使用MySQL数

据库。因此可以使用pdo方式完成数据库操作。对于pdo的使用形式完全采用面向对象的方式使用

## 1.1、开启pdo扩展

1、开启扩展

![1572857398638](/1572857398638.png)

2、查看环境信息

```php
<?php 
	phpinfo();
?>
```

3、重启Apache访问

![1572857503939](/1572857503939.png)

## 1.2、使用pdo的步骤

1、建立连接(实例化对象)

2、配置信息(可选)

3、执行SQL语句

4、关闭连接

## 1.3、pdo建立连接

1、语法格式

```php
//实例化对象即完成数据库的连接
//第一个参数指定数据库服务器相关的信息 格式为“mysql:host=服务器地址;dbname=数据库的名称;port=端口号”
//第二个参数指定账户名称
//第三个参数指定密码
$pdo = new PDO($dsn,$user,$pwd);
```

2、具体连接数据

```php
<?php 
	// 数据库的连接信息
	$dsn = 'mysql:host=127.0.0.1;dbname=test;port=3306';
	$pdo = new PDO($dsn,'root','root');
	var_dump($pdo);
?>
```

## 1.4、操作数据

数据库操作区分增删改查四种类型，但是实际经常将四种类型分为两类 分别为写入操作(INSERT语句、update语

句、delete语句)与查询操作(select语句)。在pdo中查询数据使用query方法、写入数据使用exec方法

### 1.4.1、实现数据写入

```php
<?php 
	// 数据库的连接信息
	$dsn = 'mysql:host=127.0.0.1;dbname=test;port=3306';
	$pdo = new PDO($dsn,'root','root');
	// 定义要执行写入操作的SQL语句
	$sql = "INSERT INTO php_user VALUES(NULL,'leo','leo')";
	// 调用写入方法查询数据 返回结果为受影响的行数
	$result = $pdo->exec($sql);
	var_dump($result);
?>
```

结果

![1572858369219](/1572858369219.png)

### 1.4.2、查询数据

1、查询数据

```php
<?php 
	$dsn = 'mysql:host=127.0.0.1;dbname=test;port=3306';

	//建立连接
	$pdo = new PDO($dsn,'root','root');

	// 定义查询的SQL语句
	$sql = "SELECT * FROM php_user";

	// 执行查询操作
	$result = $pdo->query($sql);

	echo '<pre>';
	var_dump($result);
?>
```

2、结果

![1572858561196](/1572858561196.png)

3、修改代码获取数据库中的数据

```php
<?php 
	$dsn = 'mysql:host=127.0.0.1;dbname=test;port=3306';

	//建立连接
	$pdo = new PDO($dsn,'root','root');

	// 定义查询的SQL语句
	$sql = "SELECT * FROM php_user";

	// 执行查询操作
	$result = $pdo->query($sql);

	echo '<pre>';
	foreach ($result as $key => $value) {
		var_dump($value);
	}
?>
```

4、结果

![1572858732817](/1572858732817.png)

## 1.5、关闭连接

![1572858799743](/1572858799743.png)

## 1.6、pdo与mysqli区别

1、pdo支持多种数据库类型

2、mysqli采用对象与过程方式都可以使用。pdo纯面向对象的使用方式

3、pdo支持预处理功能

# 2、面向对象

## 2.1、面向对象概述

### 2.1.1、面向过程

面向过程是最为直接的一种解决方式。

所谓的面向过程就是将所要解决的问题视之为一个过程。分析问题中的步骤，之后将步骤封装成函数，再按顺序一

次调用。

例如实现用户注册需要校验信息合法性、需要入库、再实现发送激活邮件等操作。安装面向过程的方式将每一个步骤封装为函数依次调用即完成功能

### 2.1.2、面向对象

Object Oriented Programing

现实对东西的描述(xxx的xxx)逻辑，移植到计算的程序语言中，形成了一种定义的语法与调用的语法。

面向对象即将所有的事务全部安装对象方式使用。某一个对象个体又能够实现某些功能

## 2.2、类与对象的使用

在面向对象编程中存在两种关键“角色”为类与对象

类为对事物的一种抽象化

对象为具体的一个事物

例如现实生活中对各种事物进行归类。类即为相同的事物个体的一种集合。对象就类集合中的具体个体

在程序代码中需要先定义类，在实例化对象 最终使用的为对象

1、定义类

```php
<?php
// class为申明类的关键字 Student为类名称 
class Student{
	// 类中可以使用成员属性与方法对"类"进行描述
}
```

2、实例化对象

```php
<?php
// class为申明类的关键字 Student为类名称 
class Student{
	// 类中可以使用成员属性与方法对"类"进行描述
}
// 实例化对象 后面的()在构造方法中没有参数时可以省略
$obj = new Student();

$obj2 = new Student;
// var_dump使用逗号分隔可以打印多个变量
var_dump($obj,$obj2);
```

## 2.3、属性使用

属性的作用是用于针对 对象的描述

1、定义属性的语法格式

```php
//属性的定义是在类上完成
class 类名称{
    //权限修饰符在PHP中存在public、private、protected三种 还有一种var等价于 public，public代表公开
    权限修饰符 $属性名称=值;
}
```

2、实际定义属性

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
	}

	$obj = new Student;
	var_dump($obj);

?>
```

3、类外对象对属性的使用

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
	}

	$obj = new Student;
	// 对象调用公开的属性 在调用属性时"对象->属性名称" 名称中不要使用$符号
	echo $obj->subject;

?>
```

## 2.4、方法使用

方法即对象个体能够实现的事情

1、定义方法语法格式

```php
class  类名称{
    权限修饰符 function 名称(参数列表){
        //方法体
        //return
    }
}
```

2、定义方法

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
		// 定义方法即定义对象的行为
		public function say(){
			echo 'say';
		}
	}
```

3、对象调用方法

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
		// 定义方法即定义对象的行为
		public function say(){
			echo 'say';
		}
	}

	$obj = new Student;
	// 对象调用公开的属性 在调用属性时"对象->属性名称" 名称中不要使用$符号
	echo $obj->subject.'<hr/>';
	// 对象调用方法
	$obj->say();

?>
```

## 2.5、$this使用

在类的定义的方法中经常需要调用自己的属性或者方法此时可以使用$this代表当前对象

### 2.5.1、调用自身属性

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
		// 定义方法即定义对象的行为
		public function say(){
			// $this代表当前对象本身。哪一个对象调用say方法 $this就代表哪一个对象
			echo '本人属于的学科为：'.$this->subject;
		}
	}

	$obj = new Student;
	$obj->say();

?>
```

### 2.5.2、调用自身的方法

```php
<?php 
	class Student{
		// 定义属性保存学科信息
		public $subject = '前端开发';
		// 定义方法即定义对象的行为
		public function say(){
			// 调用当前方法
			$this->sayhello();
			// $this代表当前对象本身。哪一个对象调用say方法 $this就代表哪一个对象
			echo '本人属于的学科为：'.$this->subject;
		}
		public function sayhello(){
			echo 'hello';
		}
	}

	$obj = new Student;
	$obj->say();

?>
```

在class里面包裹的位置叫做类内，否则叫做类外。在类内调用属性方法需要使用$this(代表着对象)关键字在类外

调用属性或者方法时需要使用对象

## 2.6、类常量

类常量与普通的常量一致都是使用const关键字修饰。

1、定义语法

```php
class 类名称{
    //常量名称中没有$符号
    权限修饰符 常量名称 = 值；
}
```

2、定义常量

```php
<?php 
	class Car{
		// 定义常量
		public const number = 4;
	}

?>
```

3、类外使用常量

```php
<?php 
	class Car{
		// 定义常量
		public const number = 4;
	}
	//::表示使用类名称调用个常量、静态属性、静态方法 Car代表类名称
	echo Car::number;

?>
```

4、类内使用常量

```php
<?php 
	class Car{
		// 定义常量
		public const number = 4;
		public function fn(){
			echo Car::number;
		}
	}
	// 类外使用常量
	// echo Car::number;
	// 类内使用常量
	$car = new Car;
	$car->fn();
?>
```

在类内使用常量时每次都使用类名称调用，在大量使用类名称调用常量的情况下。如果修改了类名称，导致最终需要将所有使用常量的情况修改。因此可以在**类内**使用self关键字代表当前类

5、使用self关键字

```php
<?php 
	class Car{
		// 定义常量
		public const number = 4;
		public function fn(){
			// self代表当前Car类
			echo self::number;
		}
	}
	// 类外使用常量
	// echo Car::number;
	// 类内使用常量
	$car = new Car;
	$car->fn();
?>
```

## 2.7、静态属性

静态属性与静态变量一致都是使用static关键字修饰。在脚本没有执行结束，静态属性不会伴随对象销毁而销毁

1、静态属性语法格式

```php
class 类名称{
    权限修饰符 static $属性名称 = 值;
}
```

2、静态属性的定义

```php
<?php 
	class Stu{
		public static $name = 'leo';
	}

?>
```

3、类外使用静态属性

```php
<?php 
	class Stu{
		public static $name = 'leo';
	}
	// 使用对象调用普通属性->符号后指定名称没有$ 但是使用静态变量需要指定$符号
	echo Stu::$name;
?>
```

4、类内使用静态属性

```php
<?php 
	class Stu{
		public static $name = 'leo';
		public function fn(){
			echo self::$name;
		}
	}
	// 使用对象调用普通属性->符号后指定名称没有$ 但是使用静态变量需要指定$符号
	// echo Stu::$name;
	$obj = new Stu;
	$obj->fn();
?>
```

5、手动销毁对象查看静态属性

```php
<?php 
	class Stu{
		public static $name = 'leo';
		public function fn(){
			echo self::$name;
		}
	}
	// 使用对象调用普通属性->符号后指定名称没有$ 但是使用静态变量需要指定$符号
	// echo Stu::$name;
	$obj = new Stu;
	// $obj->fn();
	// 手动修改静态属性的值
	Stu::$name = 'zhangsan';
	$obj->fn();
	unset($obj);//销毁对象
	echo Stu::$name;//静态属性依旧有值说明 静态属性不属于对象属于类
	
?>
```

## 2.8、静态方法

1、静态方法的定义语法

```php
class 类名称{
    权限修饰符 static function 方法名称(){
        //方法体
    }
}
```

2、定义静态方法

```php
<?php 
class Stu{
	public static $name = 'leo';
	// 定义静态方法
	public static function say(){
		echo 'hello';
	}
}

?>
```

3、类外调用静态方法

```php
<?php 
class Stu{
	public static $name = 'leo';
	// 定义静态方法
	public static function say(){
		echo 'hello';
	}
}

Stu::say();
?>
```

4、类内部调用静态方法

```php
<?php 
class Stu{
	public static $name = 'leo';
	// 定义静态方法
	public static function say(){
		echo 'hello';
		self::fn();
	}
	public static function fn()
	{
		echo 'fn';
	}
}

Stu::say();
?>
```

5、静态方法调用普通属性

```php
<?php 
class Stu{
	public static $name = 'leo';
	public $age =20;
	// 定义静态方法
	public static function say(){
		echo 'hello';
		self::fn();
	}
	public static function fn(){
		// 注意 在静态方法中没有对象时 不能使用$this调用其他的属性与方法
		echo $this->age;//由于没有对象报错
		echo 'fn';
	}
}

Stu::fn();
?>
```

>总结：类常量、静态属性、静态方法都是属于类而不是对象，在类内部调用使用self((self::常量/静态属性/静态方法())关键字，在类外调用直接使用类名称(类名称::常量/静态属性/静态方法())

# 3、魔术方法

在PHP中存在较多的魔术方法，每一个魔术方法都以__开头的名称。并且有一个规律所有的魔术方法为自动执行(就是当满足某种情况下自动执行)。在PHP中魔术方法存在  __construct()， __destruct()， __call()， __callStatic()， __get()， __set()， __isset()， __unset()， __sleep()， __wakeup()， __toString()， __invoke()， __set_state()， __clone() 和 __debugInfo() 等

## 3.1、常用的魔术方法

| 方法名称     | 作用                                   |
| ------------ | -------------------------------------- |
| __construct  | 实例化对象时自动触发执行               |
| __destruct   | 销毁对象时自动触发执行                 |
| __call       | 调用一个不存在的普通方法时自动触发执行 |
| __callStatic | 调用一个不存在的静态方法时自动触发执行 |
| __clone      | 当克隆对象时会自动触发                 |
| __get        | 当获取一个不存在的属性时触发执行       |
| __set        | 当设置一个不存在的属性时触发执行       |

## 3.2、构造方法(__construct)

构造方法当每次使用new关键字实例化对象时会自动触发执行

1、实例化对象

```php
<?php 
	class Stu{
		public $name ='zs';
	}
	$zs = new Stu;
	var_dump($zs);
	$ls = new Stu;
	var_dump($ls);
?>
```

2、结果

![1572867712742](/1572867712742.png)

3、使用构造方法修改对象属性

```php
<?php 
	class Stu{
		public $name ='zs';
		public function __construct($name){
			// 将$name变量的值赋予对象下的name属性的值使用
			$this->name = $name;
		}
	}
	$zs = new Stu('zhangsan');
	var_dump($zs);
	$ls = new Stu('lisi');
	var_dump($ls);
?>
```

4、结果

![1572867910438](/1572867910438.png)

构造方法可以借助于实现对象的初始化

## 3.3、析构方法(__destruct)

析构方法是当对象被销毁时自动执行的方法

### 3.3.1、销毁对象的场景

1、脚本结束自动销毁

2、当将保存对象的变量的值更改为其他数据时。

3、当前unset一个保存对象的变量时

### 3.3.2、使用析构方法

```php
<?php 
	class Stu{
		public $name ='zs';
		public function __construct($name){
			// 将$name变量的值赋予对象下的name属性的值使用
			$this->name = $name;
		}
		public function __destruct(){
			echo '对象被销毁';
		}
	}
	$zs = new Stu('zhangsan');
	var_dump($zs);
	unset($zs);
	echo '<hr/>';
	// $ls = new Stu('lisi');
	// var_dump($ls);
?>
```

![1572868148933](/1572868148933.png)

析构方法可以使用在一些有连接建立的场景。最后销毁对象关闭连接

## 3.4、克隆方法(__clone)

当使用PHP内置函数clone复制对象时会被自动触发执行

```php
<?php 
	class Stu{
		public $name ='zs';
		public function __construct($name){
			// 将$name变量的值赋予对象下的name属性的值使用
			$this->name = $name;
		}
		public function __destruct(){
			echo '对象被销毁<hr/>';
		}
		public function __clone(){
			echo '对象呗克隆<hr/>';
		}
	}
	// $zs = new Stu('zhangsan');
	// var_dump($zs);
	// unset($zs);
	// echo '<hr/>';
	// $ls = new Stu('lisi');
	// var_dump($ls);
	$obj = new Stu('zhaoliu');
	// clone()为内置函数复制对象 __clone为类中魔术方法
	clone($obj);
?>
```

结果

![1572868460610](/1572868460610.png)