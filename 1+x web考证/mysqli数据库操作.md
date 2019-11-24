---
typora-root-url: images
---

# 1、PHP交互MySQL的扩展方式

1、mysql扩展。该扩展完全采用面向过程方式使用。并且还存在版本支持的问题

2、mysqli扩展。可以按照面向过程方式使用也可以按照面向对象方式使用

3、pdo扩展。完全采用面向对象的方式使用。pdo方式是操作数据库的主流方式

# 2、使用mysqli扩展操作数据库

## 2.1、开启PHP的mysqli扩展

1、修改配置文件

![1572396020508](/1572396020508.png)

2、创建查看环境信息文件

```php
<?php 
	phpinfo();
?>
```

3、重启后查看

![1572396103016](/1572396103016.png)

## 2.2、mysqli使用步骤

1、打开数据库的连接 使用mysqli_connect函数

2、设置数据库操作相关的信息。需要选择数据库，设置编码

3、实现数据的增删改查 使用mysqli_query实现

4、关闭连接

## 2.3、数据写入

数据写入是包含了数据添加(INSERT)、修改(UPDATE)、删除(DELETE)。这三种类型的SQL语句会改变数据本身统

一叫做写入操作

1、创建数据库

```sql
create database test;
```

2、创建数据表

```sql
CREATE TABLE php_user(
	id INT auto_increment,
	`name` char(20) not NULL DEFAULT '',
	pwd char(32) not NULL DEFAULT '',
	PRIMARY KEY(id)
)ENGINE = INNODB DEFAULT charset='utf8'
```

3、打开数据库的连接

a)使用语法格式

```php
//1、面向对象使用方式 参数分别制定MySQL主机地址，登录的账号、密码、数据库名称、端口号、socket信息
new mysqli($host,$user,$pwd,$dbname,$port,$socket)
//2、面向过程使用方式
mysqli_connect($host,$user,$pwd,$dbname,$port,$socket)
```

b)使用面向对象的方式打开连接

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	$mysqli = new mysqli('127.0.0.1','root','root');
	var_dump($mysqli);

?>
```

结果

![1572397448181](/1572397448181.png)

c)使用面向过程方式打开连接

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	// $mysqli = new mysqli('127.0.0.1','root','root');
	// 使用面向过程方式打开连接
	$mysqli = mysqli_connect('127.0.0.1','root','root');
	var_dump($mysqli);

?>
```

![1572397567492](/1572397567492.png)

在使用mysqli扩展进行数据库操作时  两种方式任意一种都可以使用

4、设置数据库相关的信息

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	// $mysqli = new mysqli('127.0.0.1','root','root');
	// 使用面向过程方式打开连接
	$mysqli = mysqli_connect('127.0.0.1','root','root');
	// 选择数据库
	mysqli_select_db($mysqli,'test');//即执行原生SQL语句中的use test语句
?>
```

5、设置编码

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	// $mysqli = new mysqli('127.0.0.1','root','root');
	// 使用面向过程方式打开连接
	$mysqli = mysqli_connect('127.0.0.1','root','root');
	// 选择数据库
	mysqli_select_db($mysqli,'test');
	// 设置编码 具体为哪一个编码与当前使用的方式有关系
	mysqli_query($mysqli,'set names utf8');

?>
```

6、执行数据写入操作

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	// $mysqli = new mysqli('127.0.0.1','root','root');
	// 使用面向过程方式打开连接
	$mysqli = mysqli_connect('127.0.0.1','root','root');
	// 选择数据库
	mysqli_select_db($mysqli,'test');
	// 设置编码 具体为哪一个编码与当前使用的方式有关系
	mysqli_query($mysqli,'set names utf8');
	//执行添加操作
	$pwd = md5('admin');

	$sql = "INSERT INTO php_user VALUES(NULL,'admin','$pwd')";
	// 执行写入操作返回是否成功
	$res = mysqli_query($mysqli,$sql);
	// 获取主键id
	$user_id = mysqli_insert_id($mysqli);
	echo $user_id;
?>
```

7、关闭数据库的连接

```php
<?php 
	// 1、打开数据库的连接
	// 使用面向对象方式打开连接
	// $mysqli = new mysqli('127.0.0.1','root','root');
	// 使用面向过程方式打开连接
	$mysqli = mysqli_connect('127.0.0.1','root','root');
	// 选择数据库
	mysqli_select_db($mysqli,'test');
	// 设置编码 具体为哪一个编码与当前使用的方式有关系
	mysqli_query($mysqli,'set names utf8');
	//执行添加操作
	$pwd = md5('admin');

	$sql = "INSERT INTO php_user VALUES(NULL,'admin','$pwd')";
	// 执行写入操作返回是否成功
	$res = mysqli_query($mysqli,$sql);
	// 获取主键id
	$user_id = mysqli_insert_id($mysqli);
	echo $user_id;
	// 关闭连接
	mysqli_close($mysqli);
?>

```

## 2.4、数据查询

数据查询与数据写入操作基本类似只是需要从执行查询操作的结果集中获取到完整的数据

### 2.4.1、实现数据查询

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);

	var_dump($result);

	// 关闭连接
	mysqli_close($mysqli);
?>
```

结果

![1572399065213](/1572399065213.png)

### 2.4.2、结果集的解析

1、使用方法

| 方法名称               | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| mysqli_fetch_array     | 返回结果同一条数据存在两份，一份以数字为下标另外一份以字段名称为下标 |
| **mysqli_fetch_assoc** | 返回数据以字段名称为下标                                     |
| mysqli_fetch_row       | 返回结果以数字为下标                                         |

2、使用mysqli_fetch_array方法获取数据

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);
	// 使用mysqli_fetch_array方法获取数据
	var_dump(mysqli_fetch_array($result));

	// 关闭连接
	mysqli_close($mysqli);
?>
```

结果

![1572399417331](/1572399417331.png)

3、使用mysqli_fetch_assoc解析数据

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);
	// 使用mysqli_fetch_array方法获取数据
	// var_dump(mysqli_fetch_array($result));
		
	// 使用mysqli_fetch_assoc方法获取数据
	var_dump(mysqli_fetch_assoc($result)
	// 关闭连接
	mysqli_close($mysqli);
?>
```

结果

![1572399514969](/1572399514969.png)

4、使用mysqli_fetch_row获取数据

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);
	// 使用mysqli_fetch_array方法获取数据
	// var_dump(mysqli_fetch_array($result));
		
	// 使用mysqli_fetch_assoc方法获取数据
	// var_dump(mysqli_fetch_assoc($result));
	// 使用mysqli_fetch_row获取数据
	var_dump(mysqli_fetch_row($result));
	// 关闭连接
	mysqli_close($mysqli);
?>
```

![1572399574425](/1572399574425.png)

5、多条数据的解析

在上面所提供的三个方法中只能得到一条数据。如果涉及多条数据也只能得到一条

a)手动写入数据

![1572399669643](/1572399669643.png)

b)再次执行获取数据操作

![1572399703126](/1572399703126.png)

c)修改代码

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);
	// 使用mysqli_fetch_array方法获取数据
	// var_dump(mysqli_fetch_array($result));
		
	// 使用mysqli_fetch_assoc方法获取数据
	// var_dump(mysqli_fetch_assoc($result));
	
	// 使用mysqli_fetch_row获取数据
	// var_dump(mysqli_fetch_row($result));
	var_dump(mysqli_fetch_assoc($result));
	var_dump(mysqli_fetch_assoc($result));
	var_dump(mysqli_fetch_assoc($result));
	// 关闭连接
	mysqli_close($mysqli);
?>
```

结果

![1572399775996](/1572399775996.png)

d)使用循环解析结果集

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$sql = "SELECT * FROM php_user";

	$result = mysqli_query($mysqli,$sql);
	// 使用mysqli_fetch_array方法获取数据
	// var_dump(mysqli_fetch_array($result));
		
	// 使用mysqli_fetch_assoc方法获取数据
	// var_dump(mysqli_fetch_assoc($result));
	
	// 使用mysqli_fetch_row获取数据
	// var_dump(mysqli_fetch_row($result));
	
	// var_dump(mysqli_fetch_assoc($result));
	// var_dump(mysqli_fetch_assoc($result));
	// var_dump(mysqli_fetch_assoc($result));
	
	// 使用while循环解析结果集
	echo '<pre>';
	while ($row = mysqli_fetch_assoc($result)) {
		var_dump($row);
	}
	// 关闭连接
	mysqli_close($mysqli);
?>
```

结果

![1572399893087](/1572399893087.png)

# 3、MySQL事务

## 3.1、为什么要使用事务

例如当前用户在商家购买商品，当前状态用户有1000元购买了0个商品，商家库存5个收益为0元。如果用户购买

了1个商品此时金额为900元商品数量为1个，商家存款4个商品收益100。上面的步骤实施需要将用户的金额减少

100元并且商品数量加1而商家收益增加100元库存数量减少1。在上面的过程中涉及到多次数据的变更。每一次数

据变更都有可能存在异常。如果某一步骤出错可能导致后续的 步骤无法实施最终导致数据错乱。为了解决上面的

问题 可以将所有涉及到数据的过程视为一个整体。要么全部操作成功要么全部操作失败。此方式就是事务

## 3.2、事务的ACID原则

凡是支持事务功能的数据库必须会准守ACID原则。acid原则即描述从一个状态切换到另外一个状态的特性。特性

分别为原子性、一致性、隔离性、持久性

### 3.2.1、原子性

原子性即整个事情为一个单独的个体。原子性即整体不可分隔。实现状态切换时要么全部成功，要么全部失败

### 3.2.1、一致性

在上面所描述的场景中。不论用户与商家发生多少次交易。最终整体上永远金额为1000元商品数量为5个

### 3.2.3、隔离性

每个事务之间相互无影响。隔离一般与并发存在关系。例如100个人同时去购买商品但是商品数量只有5个。需要

控制到最终只能卖出5个不能超卖

### 3.2.4、持久性

即完成状态切换之后。最终结果固定不会受到其他的影响。即持久化保存最终结果

## 3.3、MySQL原生事务使用

备注：在MySQL中只有innodb引擎支持事务(事务为数据库所提供的功能)。并且默认会自动的提交事务

1、关闭事务的自动提交

![1572402778483](/1572402778483.png)

2、执行SQL语句

![1572402994805](/1572402994805.png)

结果

![1572403010105](/1572403010105.png)

3、手动提交事务

![1572403071369](/1572403071369.png)

结果

![1572403083844](/1572403083844.png)

对于上面的例子需要控制要么全部执行成要么全部执行失败。因此可以使用rollback控制回滚(还原到事务之前的状

态)

## 3.4、使用mysqli扩展控制事务

```php
<?php 
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 手动关闭自动提交事务
	mysqli_autocommit($mysqli,false);
	// 该SQL语句正常
	$sql = "INSERT INTO php_user VALUES(null,'a','a')";

	$res = mysqli_query($mysqli,$sql);
	// SQL语句存在字段缺少的问题最终无法正常执行
	$sql = "INSERT INTO php_user VALUES('a','a')";

	$res2 = mysqli_query($mysqli,$sql);

	if($res && $res2){
		// 两条SQL语句全部正常执行
		mysqli_commit($mysqli);
	}else{
		// 执行的两条SQL语句有异常 需要回滚
		mysqli_rollback($mysqli);
	}

?>
```

# 4、对登录案例使用数据库

```php
<?php 
	$user = isset($_POST['user'])?$_POST['user']:'';
	$pwd = isset($_POST['pwd'])?$_POST['pwd']:'';
	$captcha = isset($_POST['captcha'])?$_POST['captcha']:'';
	session_start();
	// 比对验证码
	if($captcha !=$_SESSION['captcha']){
		die('验证码不匹配');
	}
	// 与数据库交互匹配用户
	// 建立连接
	$mysqli = mysqli_connect('127.0.0.1','root','root','test',3306);
	// 设置编码
	mysqli_query($mysqli,'set names utf8');
	// 执行查询
	$pwd = md5($pwd);
	$sql = "SELECT * FROM php_user WHERE name='$user' AND pwd = '$pwd'";
	$result = mysqli_query($mysqli,$sql);
	// 提取结果集中的数据
	$user_info = mysqli_fetch_assoc($result);

	if(!$user_info){
		// 账号或者密码存在问题
		die('账号或者密码存在问题');
	}
	// 保存cookie
	$expire = 0;
	if(isset($_POST['box'])){
		$expire = 3600*24*7;
	}
	// 载入函数加密
	include_once 'function.php';

	setcookie('user',authcode($user,'ENCODE'),time()+$expire);
	// 完成登录后跳转到index.php
	header('location:index.php');
?>
```









