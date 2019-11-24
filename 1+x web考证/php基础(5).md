---
typora-root-url: images
---

# 1、ajax实现用户注册案例

1、创建HTML代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<form method="post" action="">
		用户名<input type="text" name="username" id="username">
		<input type="submit" value="submit" name="">
	</form>
	<script type="text/javascript" src="jquery-1.12.4.js"></script>
	<script type="text/javascript">
		// jquery页面载入事件
		$(function(){
			$('#username').blur(function(){
				// 获取用户输入的用户名
				var username = $(this).val();
				$.ajax({
					url:'checkName.php',
					data:{username:username},
					type:'post',
					dataType:'json',
					success:function(res){
						if(res.code == 0){
							console.log('用户名不可用')
						}else{
							console.log('ok');
						}
					}
				})
			})
		})
	</script>
</body>
</html>
```

2、服务端校验

```php
<?php 

	// 接受传递的用户名信息
	// isset 判断变量是否存在
	$username = isset($_POST['username'])?$_POST['username']:'';
	if(!$username){
		echo json_encode(['code'=>0,'msg'=>'参数错误']);
		exit();
	}
	
	// 设置已经被使用的用户名
	$deny = ['admin','leo','admin1'];

	if(in_array($username, $deny)){
		echo json_encode(['code'=>0,'msg'=>'用户名被占用']);
		exit();
	}

	echo json_encode(['code'=>1,'msg'=>'ok']);

?>
```

# 2、时间函数

时间戳为一个整型数字，表示1970年到当前的秒数

## 2.1、设置时区

![1571187841014](/1571187841014.png)

## 2.2、时间操作函数

time函数：获取当前时间的时间戳

date函数：将时间戳转换为日期格式

```php
<?php 
	// 获取到当前的时间戳
	$time = time();
	echo $time.'<hr/>';
	// 将时间戳转换为日期 第一个参数指定格式 第二个参数指定时间戳秒数。不指定使用当前时间
	echo date('Y-m-d H:i:s',$time).'<hr/>';
	// strtotime计算指定格式的时间戳
	echo date('Y-m-d H:i:s' ,strtotime('1 day',$time));

?>
```

# 3、数学函数

## 3.1、max/min

max计算最大值

min计算最小值

```php
<?php 
	// 计算最大值
	echo max(90,56,66,89,788);
	// 计算最小值
	echo min(90,56,66,89,788);
?>
```

## 3.2、ceil/floor

ceil向上取整，丢弃小数点后面的数据整数加1

floor向下区整，直接丢弃小数点后面的数据

```php
	echo '<hr/>';
	echo ceil(1.3).'<br>';//2
	echo floor(1.3);//1
```

## 3.3、round

作用：对小数进行四舍五入

```php
	echo '<hr/>';
	echo round(1.4).'<hr/>';//1
	echo round(1.7).'<hr/>';//2
```

## 3.4、rand

作用：产生一个随机的数字需要指定开始与结束

```php
	echo '<hr/>';
	//第一个参数为起始数字 第二个参数为介绍数字
	echo rand(100,999);
```

# 4、文件操作

当执行PHP代码文件时所有的数据全部是在内存中执行。一旦程序执行完毕数据全部销毁。在执行程序的过程中部

分重要数据需要持久化的存储。如果实现持久化存储可以使用数据库或者文件存储。如果数据比较简单可以使用文

件保存

## 4.1、打开文件

1、打开文件的语法

```php
$handle = fopen(打开文件地址,打开的模式)
//fopen打开文件返回的资源类型
 //打开文件地址一定要指定正确地址 可以使用相对于绝对地址表示
```

2、打开文件模式

![1571189545482](/1571189545482.png)

3、打开文件

a)创建文件

![1571190025134](/1571190025134.png)

b)打开文件

```php
<?php 
	// 打开文件
	$fp = fopen('text.txt', 'a+');
	var_dump($fp);

?>
```

注意：打开文件模式对文件的内容影响非常大。如果使用w模式会导致数据清空

c）结果

![1571190095965](/1571190095965.png)



## 4.2、读取文件内容

### 4.2.1、readfile

注意：readfile函数执行不需要先打开文件

```php
<?php 
	// 直接读取整个文件内容 
	$content = readfile('text.txt');
	var_dump($content);
	// 读取整个文件内容每一行作为数组中的一个元素
	var_dump(file('text.txt'));

?>
```

### 4.2.2、file_get_contengts

不需要先打开文件

```php
<?php 
	// 读取本地文件内容
	$content = file_get_contents('text.txt');
	var_dump($content);
	// 读取网络地址 本质是给指定的地址发送get请求 
	$content = file_get_contents('http://www.baidu.com');
	echo $content;

?>
```

### 4.2.3、fgets

作用：按照行读取内容

1、语法格式

```php
fgets($handle)
//$handle表示打开文件的连接 即fopen返回的结果
```

2、读取内容

```php
<?php 
	// 打开文件
	$fp = fopen('text.txt','a+');
	// 读取内容 根据指针当前的位置读取内容
	echo fgets($fp);
?>
```

### 4.2.4、fread

```php
<?php 
	$fp = fopen('text.txt','a+');
	// 第二个参数指定字节数
	$content = fread($fp, 4);
	echo $content;
?>
```

## 4.3、写入文件

### 4.3.1、file_put_contents

作用向指定的文件写入数据

```php
<?php 

	$data = 'file_put_contents文件写入不需要先打开文件';
	// 写入内容不要求文件必须存在，如果文件没有则创建后再写入 如果没有创建文件的权限执行报错
	file_put_contents('test.txt',$data);
?>
```

### 4.3.2、fwrite/fputs

1、语法格式

```php
fwrite($handle,写入内容,长度)
```

2、写入文件

```php
<?php 
	$fp = fopen('test.txt','a+');
	fwrite($fp, 'nginx高可用',8);

?>
```

![1571192489507](/1571192489507.png)

## 4.4、关闭文件

1、语法格式

```php
fclose($handle)
```

2、使用关闭文件

```php
<?php 
	$fp = fopen('test.txt','a+');
	fwrite($fp, 'nginx高可用',8);
	fclose($fp);

?>
```



## 4.5、文件操作的函数

### 4.5.1、copy

作用拷贝文件

```php
<?php 
	// 计算拷贝之后的文件名称
	$name =rand(100,999).'.ttx';
	copy('test.txt',$name);
?>
```

在拷贝时一般会对文件名称重命名。重命名的规则自己设置

### 4.5.2、unlink

作用删除文件。是否可以删除受到操作系统的权限控制

```php
<?php 
	unlink('805.ttx');

?>
```

### 4.5.3、is_file

作用：判断是否为正常文件

```php
var_dump(is_file('demo9.php'));
```

与is_file存在一个类似函数判断文件是否存在file_exists（在自动加载时使用）

作业：实现from表单文件上传。异步文件上传(借助于前端框架实现)