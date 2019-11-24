---
typora-root-url: images
---

# 1、函数

## 1.1、函数作用

函数用于实现对功能进行封装。避免相同的代码反复的编写

1、不使用函数输出table表格

```php
<?php 
	$table ='<table>';
	$table .='<tr><td>不使用函数输出表格</td></tr>';
	$table .='</table>';

	echo $table;
?>
```

2、还需要在输出一个table

```php
<?php 
	$table ='<table>';
	$table .='<tr><td>不使用函数输出表格</td></tr>';
	$table .='</table>';

	echo $table;


	$table ='<table>';
	$table .='<tr><td>再次输出表格</td></tr>';
	$table .='</table>';

	echo $table;
?>
```

当多次需要输出table表格时每次都会将相同的代码进行复制

## 1.2、函数的使用

1、定义函数

```php
function 函数名称(参数列表){
    //函数体
    //返回数据
}
```

2、调用函数

```php
函数名称(参数列表);
```

3、使用函数简化table输出

```php
<?php 
	// 定义输出table的函数
	function show_table(){
		$table ='<table>';
		$table .='<tr><td>不使用函数输出表格</td></tr>';
		$table .='</table>';
		return $table;
	}
	echo show_table();
	echo show_table();
?>
```

## 1.3、函数的参数使用

关于函数的参数区分形参与实参

形式参数：就是在定义函数中所指定的参数列表。形参只是数据的别名

实际使用参数：即在调用函数时所传递的参数

一般而言形参的个数与实参个数一致

1、使用参数修改table输出的函数

```php
<?php 
	// 定义输出table的函数
	function show_table($strings){
		$table ='<table>';
		$table .='<tr><td>'.$strings.'</td></tr>';
		$table .='</table>';
		return $table;
	}
	$string = '使用参数进行函数的控制';
	echo show_table($string);

	echo show_table('php');
?>
```

## 1.4、可选参数

在调用函数时可以不传递参数。函数体在执行的过程中会使用参数的默认值。可选的参数是在定义函数时指定并且

可选参数会设置默认值。可选参数必须在参数列表后面

```php
<?php 
	// 计算器的函数 可选参数在最后并且具备默认值
	function sum($number1,$number2,$op='+'){
		switch ($op) {
			case '+':
				return $number1 + $number2;
				break;
			case '-':
				return $number1 - $number2;
				break;
			case '*':
				return $number1 * $number2;
				break;
			case '/':
				return $number1 / $number2;
				break;
			default:
				echo '操作符号错误';
				break;
		}
	}
	echo sum(1,2).'<br/>';
	echo sum(2,3,'*');


?>
```

## 1.5、返回值

return可以终止函数代码的执行并且将结果返回给调度者。函数只能返回一个变量。如果在函数体没有手动的

return最终函数也返回null

```php
<?php 
	// 计算器的函数 可选参数在最后并且具备默认值
	function sum($number1,$number2,$op='+'){
		switch ($op) {
			case '+':
				return $number1 + $number2;
				break;
			case '-':
				return $number1 - $number2;
				break;
			case '*':
				return $number1 * $number2;
				break;
			case '/':
				if($number2 ==0){
					// 返回的结果不限制
					return FALSE;
				}
				return $number1 / $number2;
				break;
			default:
				echo '操作符号错误';
				break;
		}
	}
	$res = sum(1,0,'/');
	if($res ===FALSE){
		echo '操作数据异常';
	}else{
		var_dump($res);
	}
	echo '<hr/>';
	var_dump(sum(2,3,'%'));


?>
```

# 2、作用域

## 2.1、作用域的分类

作用域用于限制在指定区域中能否使用够一个变量。作用域区分全局与局部

局部作用域：在函数体所包裹的部分就是局部作用域。局部作用域下只能在该内使用

全局作用域：除了函数体所包裹的之外其他的全部是全局作用域

![1571045793544](1571045793544.png)

## 2.2、作用域的案例

1、全局与局部作用域

```php
<?php
	//全局作用域下的变量 
	$number = 30;

	function fn(){
		// 局部作用域下的变量
		$number = 20;
		echo $number;//20
	}

	fn();
	// 输出全局作用域下的变量
	echo $number;//30

?>
```

2、参数的作用域

参数的作用域一般为局部作用域。PHP中调用函数传递的全局作用域的参数等价于将参数拷贝一份

```php
<?php 
	$number = 10;
	function test($a){
		
		$a = 20;
		echo $a;
	}
	// 调用函数传递全局作用域下的变量。函数传参默认采用传值方式。即将数字10拷贝一份赋予函数运行过程中的$a变量
	test($number);

	echo $number;
?>
```

3、引用传递参数

```php
<?php 
	$number = 10;

	// 引用方式传递参数是在函数定义时设置
	function test(&$a){
		$a = 20;//将$a变量修改为20。$number值也是20
		echo $a;
	}
	// 将$number变量的地址赋值给函数中的$a变量。即$a与$number 对应着同一个变量
	test($number);

	echo $number;//20
?>
```

## 2.3、$GLOBALS

$GLOBALS变量中存储着全局作用域下的所有的变量信息。可以在任何位置使用该变量。即在局部作用域中也可以使用全局作用域下的变量

1、创建测试的代码

```php
<?php 
	$number = 10;

	function fn(){
		echo '<pre>';
		var_dump($GLOBALS);
	}

	fn();
?>
```

2、结果

![1571051491876](/1571051491876.png)

3、修改代码

```php
<?php 
	$number = 10;

	function fn(){
		echo '<pre>';
		// var_dump($GLOBALS);
		// 使用数组格式使用元素内容
		$GLOBALS['number'] = 20;//修改全局作用域下的number

		$number = 30; //在局部作用域下创建number变量并且值为20

		echo $number;//输出局部作用域下的number
		echo '<br/>';
	}

	fn();
	echo $number;//20
?>
```

## 2.4、global

1、创建测试代码

```php
<?php 

	$number = 10;

	function fn(){
		// 引用全局作用域下变量$number;
		global $number;
		$number = 20;
		echo $number;//20
	}

	fn();
	echo $number;//20
?>
```

2、释放变量

```php
<?php 
	$number = 10;
	function fn(){
		// 引用全局作用域下变量$number;
		global $number;
		$number = 20;
		echo $number;//20
		unset($number);

	}

	fn();
	echo $number;//20
?>
```

总结：global关键字对全局作用域下的变量进行引用。在函数体中使用global关键字等价于在函数中创建一个局部

作用域的同名变量并且两个变量指向同一个内存地址

# 3、静态变量

1、普通变量的使用

```php
<?php 
	
	function fn(){
		$i = 0;
		$i++;
		return $i;
	}

	echo fn();//1

	echo fn();//1

?>
```

函数调用，局部作用域下的变量使用完毕之后就会销毁

2、使用静态变量

```php
<?php 
	
	function fn(){
		// 静态变量只会被申明一次
		static $i = 0;
		$i++;
		return $i;
	}

	echo fn();//1
	// 在程序没有执行完毕静态变量不会被销毁
	echo fn();//2

?>
```

>总结：静态变量初始化只执行一次。并且在程序没有执行完毕之前不会被销毁。函数中普通变量一旦函数执行完毕全部回收



# 4、文件载入

## 4.1、为什么要使用文件载入功能

php文件的执行入口永远是php后缀的文件。但是经常需要显示html内容。由于PHP与html可以混合编写。实现时可以直接使用PHP指定html代码。但是形成了混合方式不利于开发。因此需要拆分html代码出去。拆分之后就需要使用文件载入与PHP配合

## 4.2、演示文件载入使用

1、常规的混编方式

```php
<?php 
	$title = '文件载入的使用';
?>
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h1><?php echo $title; ?></h1>
</body>
</html>
```

2、创建html文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<h1><?php echo $title; ?></h1>
</body>
</html>
```

3、PHP中载入文件

```php
<?php 
	$title = '文件载入的使用';
	include 'demo13.html';
?>
```

可以理解载入操作就是将代码拷贝到载入的位置

## 4.3、include与require区别

require载入文件错误(文件地址错误)直接终止代码执行

include载入文件错误会存在警告。即代码还可以继续向下执行

1、使用require载入文件

```php
<?php

	require 'common.php';//该文件是不存在

	echo 'hello';
?>
```

结果

![1571054114031](/1571054114031.png)

2、使用include载入文件

```php
<?php

	include 'common.php';//该文件是不存在

	echo 'hello';
?>
```

![1571054239312](/1571054239312.png)

## 4.4、include_once与include区别

include：可以多次载入相同的文件

include_once：相同文件只载入一次

1、创建公共函数文件

```php
<?php 

	function fn(){
		
	}
?>
```

2、多次使用include载入文件

```php
<?php 
	include 'func.php';
	include 'func.php';
?>
```

![1571054450736](/1571054450736.png)

3、使用include_once载入

```php
<?php 
	include_once 'func.php';
	include_once 'func.php';
?>
```

