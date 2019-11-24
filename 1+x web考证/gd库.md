---
typora-root-url: images
---

# 1、gd概述

## 1.1、gd的作用

在平常使用web项目时，经常会使用到注册功能。注册功能一般都会使用到验证码。验证码即gd所提供的一项功

能。目的是为了防止接口被非法请求

gd除了可以绘制验证码之外。只要根图片相关的都可以实现，例如实现图形统计、实现添加水印

## 1.2、开启扩展

1、创建查看PHP环境的文件

```php
<?php 
	// 列出当前Php的环境信息
	phpinfo();
?>
```

2、查看环境信息

![1571791310842](/1571791310842.png)

3、根据php的信息下载对应的dll文件

根据PHP的版本、线程是否安全、编译器的版本、PHP的位数下载相关的扩展文件(在Windows中为dll后缀的文件)

4、查看配置项中的扩展目录

![1571791482426](/1571791482426.png)

5、修改配置项引入

![1571791567261](/1571791567261.png)

6、重启Apache后查看环境信息

![1571791614962](/1571791614962.png)

下载扩展地址：<http://pecl.php.net/>

# 2、gd的使用

## 2.1、使用步骤

1、创建画布

2、绘制形状

3、图形的输出

4、销毁图片

## 2.2、创建画布

### 2.2.1、直接创建画布

1、语法格式

```php
//都是返回的资源类型 并且在调用时需要指定画布的宽度、高度
imagecreatetruecolor($width,$height)//创建真彩画布
imagecreate($width,$height)//创建普通画布 
    
```

2、创建画布

```php
<?php 
	$image = imagecreatetruecolor(300, 300);
	var_dump($image);
?>
```

### 2.2.2、在已有图片上创建画布

在已有的图片的基础上创建画布等价于使用某一种图片作为背景图片

1、语法

```php
//参数指定文件地址
imagecreatefrompng($filename)//使用png格式的图片创建画布
imagecreatefromjpeg($filename)//使用jpeg格式的图片创建画布
imagecreatefromgif($filename)//使用gif格式的图片创建画布
```

2、使用图片创建画布

```php
<?php 
	// $image = imagecreatetruecolor(300, 300);
	$image = imagecreatefromjpeg('1.jpg');
	var_dump($image);
?>
```

3、结果

![1571792647801](/1571792647801.png)

## 2.3、图像输出

1、语法

```php
//第一个参数为资源类型 第二个参数不指定表示对图片直接输出 第二个参数指定文件名称 如果使用表示将图片保存文件
imagepng($image,[$filename])//图片输出到png格式
imagejpeg($image,[$filename])//图片输出到jpg格式
imagegif($image,[$filename])//图片输出到gif格式
    
```

2、输出图片

```php
<?php 
	// $image = imagecreatetruecolor(300, 300);
	$image = imagecreatefromjpeg('1.jpg');
	// var_dump($image);
	// 输出图片
	imagejpeg($image);
?>
```

3、结果

![1571792952125](/1571792952125.png)

4、解决问题

```php
<?php 
	// $image = imagecreatetruecolor(300, 300);
	$image = imagecreatefromjpeg('1.jpg');
	// var_dump($image);
	// 输出图片
	// header函数用于设置响应头信息 参数指定完整的头信息
	// content-type指定浏览器按照什么样的格式对内容进行输出
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
?>
```

5、结果

![1571793645122](/1571793645122.png)

## 2.4、销毁图片

1、语法格式

```php
imagedestroy($image)//销毁图片
```

2、销毁

```php
<?php 
	// $image = imagecreatetruecolor(300, 300);
	$image = imagecreatefromjpeg('1.jpg');
	// var_dump($image);
	// 输出图片
	// header函数用于设置响应头信息 参数指定完整的头信息
	// content-type指定浏览器按照什么样的格式对内容进行输出
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

# 3、绘制图像

## 3.1、填充背景色

1、语法

```php
//1、为图片分配颜色 最后返回int的标识
int imagecolorallocate ( resource $image , int $red , int $green , int $blue )
//第一个参数为创建画布的资源类型
//第二个第三个第四个参数分别要指定RGB三原色的数字 数字取值为0-255
//2、填充颜色  返回布尔值是否完成填充 
bool imagefill ( resource $image , int $x , int $y , int $color )
//第一个参数为创建画布的资源类型
//第二个与第三个参数指定填充点的x与y轴的坐标。最终将填充点的附近全部颜色修改即整个画布颜色修改
//第四个参数指定创建颜色的标识 即使用imagecolorallocate函数所返回的数字
```

2、分配颜色

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 255, 3, 3);
	var_dump($bg_color);
?>
```

3、填充颜色

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 182, 182, 182);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

4、结果

![1571794951971](/1571794951971.png)

## 3.2、绘制干扰点

干扰电其实就是在画布的一个"单元格"上打上一个点（特殊的颜色）

1、语法

```php
//第一个参数为资源类型
//第二个第三个参数指定绘制点的坐标
//第四个参数指定颜色
bool imagesetpixel ( resource $image , int $x , int $y , int $color )
```

2、绘制一个点

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 232, 232, 232);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 4、绘制点 需要先设置点的颜色 然后在绘制
	$color = imagecolorallocate($image,25,16,222);//设置干扰点的颜色
	imagesetpixel($image, 250, 250, $color);
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

3、绘制多个干扰点

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 232, 232, 232);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 4、绘制点 需要先设置点的颜色 然后在绘制
	$color = imagecolorallocate($image,255,0,0);//设置干扰点的颜色
	imagesetpixel($image, 250, 250, $color);
	// 绘制多个干扰点
	for($i=1;$i<=300;$i++){
		// 随机的设置干扰点的位置
		$x = rand(0,$width);
		$y = rand(0,$height);
		imagesetpixel($image, $x, $y, $color);
	}
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

4、结果

![1571795749306](/1571795749306.png)

## 3.3、绘制干扰线条

1、语法

```php
//第一个参数为创建画布的资源
//第二个与第三个参数表示第一个点的x与y轴坐标
//第四个与第五个参数表示第二个点的x与y轴坐标
//第六个参数指定线条的颜色
bool imageline ( resource $image , int $x1 , int $y1 , int $x2 , int $y2 , int $color )
```

2、绘制线条

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 232, 232, 232);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 4、绘制点 需要先设置点的颜色 然后在绘制
	$color = imagecolorallocate($image,255,0,0);//设置干扰点的颜色
	imagesetpixel($image, 250, 250, $color);
	// 绘制多个干扰点
	for($i=1;$i<=300;$i++){
		// 随机的设置干扰点的位置
		$x = rand(0,$width);
		$y = rand(0,$height);
		imagesetpixel($image, $x, $y, $color);
	}
	// 绘制干扰线条
	$line_color = imagecolorallocate($image,1,1,1);
	imageline($image, 50, 50, 400, 400, $line_color);
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

3、结果

![1571797024961](/1571797024961.png)

## 3.4、绘制内容

1、语法

```php
//水平绘制一个字符
bool imagechar ( resource $image , int $font , int $x , int $y , string $c , int $color )
//垂直的绘制一个字符
bool imagecharup ( resource $image , int $font , int $x , int $y , string $c , int $color )
//水平绘制字符串
bool imagestring ( resource $image , int $font , int $x , int $y , string $s , int $col )
//绘制中文字符
array imagettftext ( resource $image , float $size , float $angle , int $x , int $y , int $color , string $fontfile , string $text )  
    
```

2、绘制字符

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 232, 232, 232);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 4、绘制点 需要先设置点的颜色 然后在绘制
	$color = imagecolorallocate($image,255,0,0);//设置干扰点的颜色
	imagesetpixel($image, 250, 250, $color);
	// 绘制多个干扰点
	for($i=1;$i<=300;$i++){
		// 随机的设置干扰点的位置
		$x = rand(0,$width);
		$y = rand(0,$height);
		imagesetpixel($image, $x, $y, $color);
	}
	// 绘制干扰线条
	$line_color = imagecolorallocate($image,1,1,1);
	imageline($image, 50, 50, 400, 400, $line_color);
	// 绘制字符
	$font_color = imagecolorallocate($image,255,8,8);
	imagechar($image, 5, 100, 120, 'C', $font_color);
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

3、绘制多个字符

```php
<?php 
	// 画布的宽高
	$width = 500;
	$height = 500;
	// 1、创建画布
	$image = imagecreatetruecolor($width,$height);
	// 2、分配颜色
	$bg_color = imagecolorallocate($image, 232, 232, 232);
	// 3、填充颜色
	imagefill($image, 0, 0, $bg_color);
	// 4、绘制点 需要先设置点的颜色 然后在绘制
	$color = imagecolorallocate($image,255,0,0);//设置干扰点的颜色
	imagesetpixel($image, 250, 250, $color);
	// 绘制多个干扰点
	for($i=1;$i<=300;$i++){
		// 随机的设置干扰点的位置
		$x = rand(0,$width);
		$y = rand(0,$height);
		imagesetpixel($image, $x, $y, $color);
	}
	// 绘制干扰线条
	$line_color = imagecolorallocate($image,1,1,1);
	imageline($image, 50, 50, 400, 400, $line_color);
	// 绘制字符
	$font_color = imagecolorallocate($image,255,8,8);
	$string ='ABCDEFGHIJKLMN0123456789';//规定可以使用的字符
	for($i=0;$i<4;$i++){
		// 随机挑选字符作为验证码
		$number = rand(0,strlen($string)-1);
		$str = $string[$number];
		$x = rand(0,$width);
		$y = rand(0,$height);
		// var_dump($str);
		imagechar($image, 5, $x ,$y, $str, $font_color);
	}
	// exit();
	
	// 输出图片
	header('Content-Type: image/jpeg; charset=UTF-8');
	imagejpeg($image);
	// 销毁图片
	imagedestroy($image);
?>
```

作业：实现绘制一个4个字符的验证码 验证码图片的宽高 设置为200，36像素 并且验证码字符顺序出现

考虑如果在表单数据提交过程中使用验证码并且比对(需要使用session会话机制)