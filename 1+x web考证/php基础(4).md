---
typora-root-url: images
---

# 1、数组

## 1.1、创建数组的方式

1、创建数组的格式

```php
1、$变量名称=array(数组的元素);//PHP低版本中使用
2、$变量名称 = [数组的元素];//在PHP高版本中使用
3、$变量名称[]=元素内容//直接向变量中追加元素
```

2、数组中元素指定

在数组中所记录的每一份数据叫做元素。数组所记录的是field(元素下标)与value(元素的值)的映射关系

```php
$data = [
    '下标'=>值,
    '下标'=>值
]
```

在设置数组时 可以省略下标

## 1.2、数字索引与关联索引

### 1.2.1、数字索引

在设置数组时下标统一使用数字的方式指定

1、创建索引数组

```php
<?php 
	$data = [
		// 标准格式为 field=>value
		'php',
		'python',
	];
	echo '<pre>';
	print_r($data);//打印变量
?>
```

结果

![1571118606216](/1571118606216.png)

2、手动指定数字下标

```php
<?php 
	$data = [
		// 标准格式为 field=>value
		'php',
		'python',
		5=>'c++',
		'javascript'
	];
	echo '<pre>';
	print_r($data);
?>
```

结果

![1571118806838](/1571118806838.png)

3、读取数组中元素

```php
$变量名称['元素的下标'];//该格式数字与关联索引都可以使用
```

4、数组相关的操作

```php
<?php 
	$data = [
		// 标准格式为 field=>value
		'php',
		'python',
		5=>'c++',
		'javascript'
	];
	echo '<pre>';
	print_r($data);
	// 读取数组元素
	echo $data[5];
	// 修改数组元素
	$data[5]='c/c++';
	// 删除元素
	unset($data[6]);
	
?>
```

### 1.2.2、关联索引

关联索引即数组中元素下标使用字符串表示

```php
<?php 
	$user_info = [
		'username'=>'leo',
		'pwd'=>'123456',
		'role_id'=>1
	];
	echo '<pre>';
	print_r($user_info);
	// 直接向数组中追加元素
	$order['id'] = 10;
	// 追加数字索引元素
	$order['price']='100';
	print_r($order);

?>
```

![1571119457100](/1571119457100.png)



## 1.3、数组循环

### 1.3.1、使用for循环

1、PHP循环数组

```php
<?php 
	$data = [1,2,3,4,5,6,7,8];
	// count 获取数组中元素个数
	for($i = 0;$i<count($data);$i++){
		echo $data[$i];
	}

?>
```

2、使用for循环数组的问题

```php
<?php 
	$data = [1,2,3,4,5,6,7,8];
	// count 获取数组中元素个数
	for($i = 0;$i<count($data);$i++){
		echo $data[$i];
	}
	echo '<hr/>';
	$data = [
		'php',
		'javascript',
		5=>'golang',
		'c++'
	];
	// 计算数组元素个数为4
	for($i = 0;$i<count($data);$i++){
		echo $data[$i];
	}
?>
```

![1571120432373](/1571120432373.png)

### 1.3.2、使用foreach循环

1、foreach语法格式

```php
foreach($循环变量 as $key=>$value){
    //$key代表每一次循环的元素的下标的临时变量
    //$value代表每一次循环的元素的值的临时变量
    //处理程序
}
```

2、使用foreach循环

```php
<?php 
	$data = [
		'php',
		'javascript',
		5=>'golang',
		'c++'
	];

	foreach ($data as $key => $value) {
		echo '循环的下标:'.$key.'循环的元素值'.$value.'<br/>';
	}

?>
```

结果

![1571120725853](/1571120725853.png)

3、使用foreach循环增加数组元素的值

```php
<?php 

	$data = [1,2,3,4,5];
	foreach ($data as $key => $value) {
		$value+=10;
	}
	echo '<pre>';
	print_r($data);
?>
```

结果

![1571120867908](/1571120867908.png)

使用foreach循环数组其中使用的$key与$value都是临时变量 修改临时变量不能改变到数据本身

```php
	$data = [1,2,3,4,5];
	foreach ($data as $key => $value) {
		$data[$key] = $value+10;
	}
	echo '<pre>';
	print_r($data);
```

### 1.3.3、使用指针操作

| 函数名称                | 作用                                   |
| ----------------------- | -------------------------------------- |
| current(数组的变量名称) | 获取当前指针对应的数组元素值           |
| next(数组的变量名称)    | 将指针向下移动一步并且取出元素值       |
| prev(数组的变量名称)    | 将指针向上移动一步并且取出元素值       |
| end(数组的变量名称)     | 将指针移动到最后一个元素并且取出元素值 |
| reset(数组的变量名称)   | 将指针移动到第一个元素并且取出元素值   |
| key(数组的变量名称)     | 获取当前数组指针对应的元素的下标       |

```php
<?php 
	$user_info = [
		'username'=>'leo',
		'pwd'=>'123456',
		'role_id'=>1
	];

	// 获取当前指针对应的key
	echo key($user_info).'<br/>';
	// 指针向下移动
	echo next($user_info);
	reset($user_info);

	echo current($user_info);


?>
```

在数组中默认指针对应第一个元素

## 1.4、多维数组

在PHP中数组的元素可以存储任意数据类型包括元素内容设置为数组

```php
<?php 
	// 定义二维数组
	$user_info =[
		['username'=>'zhangsan'],
		['username'=>'lisi'],
		['username'=>'zhaoliu']
	];
	// 使用二维数组的元素
	echo $user_info[0]['username'];
	foreach ($user_info as $key => $value) {
		var_dump($value);//每次循环$value还是一个数组 如果需要获取到下面的元素需要继续循环
	}
?>
```

# 2、数组相关函数

## 2.1、array_keys/array_values

array_keys(数组名称)：获取数组中所有的元素的小标返回一个数组集合

array_values(数组变量名称)获取数组中所有元素的值返回一个数组集合

```php
<?php
	$user_info =[
		'username'=>'leo',
		'pwd'=>'123456',
		'role_id'=>1
	];
	echo '<pre>';
	var_dump(array_keys($user_info));
	echo '<hr/>';
	var_dump(array_values($user_info));
?>
```



## 2.2、array_key_exists

作用：检查数组中某一个key是否存在

```php
<?php
	$user_info =[
		'username'=>'leo',
		'pwd'=>'123456',
		'role_id'=>1
	];
	// 第一个参数指定要坚持的key名称 第二个参数指定寻找的数组变量名称
	if(array_key_exists('username',$user_info)){
		echo '找到了';
	}else{
		echo '没有对应的元素';
	}
```



## 2.3、array_pop/array_push

array_pop：从数组末尾（右侧）中弹出一个元素。

array_push：向数组的末尾追加元素

array_unshift 在数组开头插入一个或多个单元

array_shift:在数组的开头弹出元素

上面的四个函数都会改变数据本身。使用者四个函数可以实现队列(先进先出类似水管)与栈数据(先进后出类似水

杯)结构

```php
<?php 
	$array = ['php','c'];
	// 向数组尾部追加元素
	$array[] = 'java';
	array_push($array, 'shell');

	echo '<pre>';
	print_r($array);
	// 数组尾部弹出元素
	echo array_pop($array);
	echo '<hr/>';
	print_r($array);

	// 数组之前追加元素
	array_unshift($array, 'html');
	print_r($array);

?>
```

## 2.4、in_array

作用：判断数组中是否存在某一个元素(值的判断)

```php
	$array = ['php','c'];
	var_dump(in_array('php', $array));
```

# 3、字符串相关的函数

## 3.1、strlen

作用：获取字符串的长度(字节)

```php
<?php 

	$str = 'hello world';

	$str2 = 'php你好';
	echo strlen($str).'<hr/>';//11

	echo strlen($str2);//9 中文utf8编码一个字符等于3个字节
	// 字符串截取
	echo substr($str, 6,strlen($str));

	echo mb_substr($str2,4,6);
?>
```

## 3.2、strpos/strrpos

strpos：查找字符串第一次(从左向右数)出现的位置

strrpos：查找字符串最后一次(从右向左匹配)出现的位置

```php
	$str = 'hello world';
	echo strpos($str, 'l').'<hr/>';//2
	echo strrpos($str, 'l').'<hr/>';//9
	echo strpos($str, 'h').'<hr/>';//0
```

当查找字符串位置时 是否找到需要与false进行全等判断

## 3.3、strtolower/strtoupper

strtolower：将字符串转换为小写

strtoupper：将字符串转换为大写

```php
	$str = 'hello world';
	$str2=  strtoupper($str);//HELLO WORLD
	echo $str2.'<br/>';
	echo strtolower($str2);//hello world
```

## 3.4、trim/ltrim/rtrim

trim:去掉字符串左右空白符号或者其他符号

ltrim:去掉字符串左侧的空白或者其他符号

rtrim：去掉字符串右侧的空白或者其他符号

```php
	// 去掉左右的空白
	$str = ' leo ';
	var_dump($str);
	var_dump(trim($str));
```

## 3.5、md5

作用：将字符串进行md5方式编码

```php
	echo md5('admin');
```

在实际项目中针对用户的密码加密可以多次使用md5加密或者配合盐一起完成提示密码的复杂度



## 3.6、json_encode/json_decode

json在js语言中为对象，在PHP语言中为字符串

json_encode：将数据转换为json格式的字符串"{名称：值}"

json_decode:将json格式的字符串转换为数组或者对象

```php
<?php 
	$data = ['code'=>1,'msg'=>'请求正常'];

	echo '<pre>';
	// JSON_UNESCAPED_UNICODE 设置中文不处理
	$json = json_encode($data,JSON_UNESCAPED_UNICODE);
	echo $json.'<hr/>';

	// 将字符串转换为对象
	var_dump(json_decode($json));
	// 将字符串转换为数组
	var_dump(json_decode($json,true));
?>
```



## 3.7、explode/implode

explode：将字符串按照指定的字符进行分割 分割之后形成数组

implode：将数组按照指定的符号进行合并 最终形成字符串

```php
<?php 
	$role_ids = '1,2,3,4';
	// 将字符串转换为数组  第一个参数指定分隔符号 第二个参数指定被分隔的变量
	$array = explode(',', $role_ids);
	var_dump($array);
	// 将数组转换为字符串
	echo implode(',', $array);
?>
```

使用ajax验证注册时用户名是否可用