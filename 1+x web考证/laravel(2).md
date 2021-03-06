---
typora-root-url: images
---

# 1、控制器

## 1.1、创建控制器

### 1.1.1、控制器的创建规则

1、存储目录：控制器文件必须保存在app\Http\Controllers目录下

2、文件命名：完整格式为"控制器名称Controller.php" 控制器名称自定义但是需要确保名称为首字母大写的驼峰

式命名

3、代码的编写

​	a)申明命名空间

​	b)根据需要使用的类进行use

​	c)创建类以及方法的代码 类名称需要与控制器名称统一(如果不统一导致自动加载机制无法找到类文件)

### 1.1.2、创建自己的控制器

1、创建控制器文件

![1573537277960](/1573537277960.png)

2、代码编写

![1573537583915](/1573537583915.png)

### 1.1.3、使用artisan工具创建

![1573537917950](/1573537917950.png)

结果

![1573537926801](/1573537926801.png)

### 1.1.4、控制器分组存储

当前创建控制器都是保存在Controllers目录下，但是一般的web项目很多都区分前台与后台。如果所有控制器全

部存储到一个目录下不方便维护。可以对控制器安装目录进行分组

1、创建组

![1573538102484](/1573538102484.png)

2、命令行在admin分组下创建控制器

![1573538202884](/1573538202884.png)

3、手动在admin分组下创建控制器

![1573538395878](/1573538395878.png)

## 1.2、控制器路由

1、设置普通控制器路由

![1573538560001](/1573538560001.png)

2、访问执行

![1573538572911](/1573538572911.png)

3、设置分组控制器的路由

![1573538736754](/1573538736754.png)

![1573538743649](/1573538743649.png)

# 2、请求

请求即获取http协议中相关的一些信息。在框架中将所有的请求相关的信息全部保存到了一个Request类的对象中。如果需要得到请求的信息必须要获取到对象

## 2.1、获取请求对象

### 2.1.1、使用辅助函数获取

![1573539025830](/1573539025830.png)

结果

![1573539414303](/1573539414303.png)

### 2.1.2、使用依赖注入(推荐)

依赖注入为一种设计模式用于对程序进行解耦。依赖注入也可以称之为控制反转

1、使用依赖注入获取到对象

![1573539810900](/1573539810900.png)

2、结果

![1573539835081](/1573539835081.png)

3、设置使用use

![1573539898680](/1573539898680.png)

结果

![1573539906939](/1573539906939.png)

## 2.2、获取请求相关的信息

1、语法格式

```php
$request->方法()；
```

2、增加路由

![1573540055724](/1573540055724.png)

3、创建方法演示常用方法

![1573540397849](/1573540397849.png)

结果

![1573540408053](/1573540408053.png)



## 2.3、请求参数获取

### 2.3.1、使用request对象获取

1、请求参数获取的方法



| 方法名称              | 作用                                   |
| --------------------- | -------------------------------------- |
| all()                 | 获取到所有的参数数据                   |
| input('名称')         | 获取指定名称的参数                     |
| input('名称',默认值)  | 获取指定名称的参数，如果没有使用默认值 |
| only([名称1，名称2])  | 获取指定名称的参数数据                 |
| except([名称1,名称2]) | 除了指定名称的参数其他的全部获取       |

2、创建路由

![1573540798535](/1573540798535.png)

3、演示方法的使用

![1573541081094](/1573541081094.png)

### 2.3.2、使用input获取

1、input下的使用方法

| 方法名称                     | 作用                                   |
| ---------------------------- | -------------------------------------- |
| Input::all()                 | 获取到所有的参数数据                   |
| input::get('名称')           | 获取指定名称的参数                     |
| Input::get('名称',默认值)    | 获取指定名称的参数，如果没有使用默认值 |
| Input::only([名称1，名称2])  | 获取指定名称的参数数据                 |
| Input::except([名称1,名称2]) | 除了指定名称的参数其他的全部获取       |
| Input::has('名称') |判断指定名称的参数是否存在       |

2、创建路由

![1573541307400](/1573541307400.png)
3、使用input类获取数据

![1573541346407](/1573541346407.png)

4、结果

![1573541358145](/1573541358145.png)

5、使用use引入类

![1573541443016](/1573541443016.png)

6、简化use的写法

a)修改app.php配置文件

![1573541581390](/1573541581390.png)

b)修改控制器代码

![1573541628350](/1573541628350.png)