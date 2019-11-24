---
typora-root-url: images
---

# 1、session

## 1.1、为什么要使用session

在前面使用cookie的过程中使用受到一些影响。

1、cookie存储的数据条目数受到浏览器的影响

2、cookie是明文方式存储。并且在请求与响应的头信息中。可以被模拟

为了解决上面的问题可以使用session完成。session是基于cookie的基础之上

## 1.2、session介绍

session与cookie的作用类似都是用于存储部分数据保存用户相关信息。session的数据默认存储文件并且存储在服

务端下

## 1.3、cookie与session之间的区别

1、cookie数量受到限制

2、session存储数据无限制

3、cookie数据存储客户端 session数据存储服务端

4、cookie数据不安全 session的安全系统更高

# 2、session的使用

## 2.1、session相关的配置

```php
//指定session的存储数据方式 files表示文件方式存储
session.save_handler = files
//指定session的存储数据的目录地址
session.save_path = "/tmp"
//指定session标识符号的名称
session.name = PHPSESSID
//session是否自动启动
session.auto_start = 0
//指定session最大有效时间 单位为秒
session.gc_maxlifetime = 1440
//session的gc回收机制触发的概率 默认1/1000   
session.gc_probability = 1
session.gc_divisor = 1000    
```

## 2.2、session的使用步骤

1、开启session机制 即执行session_start函数

2、实际使用session完成操作。使用即对$_SESSION预定义变量进行数据操作

## 2.3、设置session内容

1、创建文件

```php
<?php 

	echo 'use session';
?>
```

2、抓包查看请求信息

![1572328505034](/1572328505034.png)

3、修改代码使用session

```php
<?php 
	// 1、打开session机制
	session_start();
	// 2、操作session中的数据 即$_SESSION变量进行操作，$_SESSION变量为数组。数组中元素名称需要存储数据的名称，元素值即保存的数据
	// 创建一个session的数据
	$_SESSION['php'] = 'lanuage';
	
	echo 'use session';
?>
```

4、再次抓包查看头信息

![1572328839645](/1572328839645.png)

5、再次抓包查看

![1572328945489](/1572328945489.png)

通过上面的结果可以证明session是在cookie的基础之上实现的

6、查看session的数据文件

![1572329188820](/1572329188820.png)

## 2.3、session的其他操作

```php
<?php 
	
	session_start();
	// 读取session中的内容
	echo '<pre>';
	print_r($_SESSION);
	// 修改session的内容
	$_SESSION['php'] = ['role'=>'管理员','addtime'=>time()];
	echo '<hr/>修改session之后的结果'; 
	print_r($_SESSION);
	// 删除session的内容
	unset($_SESSION['php']);
	echo '<hr/>删除session之后的结果'; 
	print_r($_SESSION);

?>
```

# 3、session机制

在PHP中已经完成整个session功能的封装。对外使用时直接开启机制使用$_SESSION变量即可。但是默认为文件

存储数据可能会导致session使用异常

## 3.1、session异常的场景

当项目使用用户比较庞大。项目想要正常运行。需要使用到负载均衡技术。负载均衡的情况下session就异常

![1572330482719](/1572330482719.png)

解决上面的问题需要修改到session的整套机制

## 3.2、session机制

在session的整套机制中存在6个操作分别 使用打开连接、读取数据、写入数据、关闭连接、销毁数据、gc回收

![1572331539441](/1572331539441.png)

1、当用户第一次访问项目时。cookie头信息中没有携带任何数据

2、服务端接受请求，当执行到代码session_start函数时，正式打开了session机制

3、服务端立马读取cookie头信息中是否存在session的标识。有则直接后续使用，没有根据自己的规则计算一个

唯一不重复的标识。此步骤之后永远可以得到一个标识

4、PHP服务端自动根据标识打开对应的存储介质(受到了配置项的影响)的连接(默认即打开文件)

5、PHP服务端自动读取对应存储介质中数据，再将序列化后的字符串反序列化操作转换为数组格式存储到

$_SESSION变量中

6、PHP按照自己的代码对$_SESSION变量进行操作

7、当代码执行完毕PHP自动的将$_SESSION变量中的内容序列化之后保存到对应的存储介质

8、自动的关闭对应存储介质的连接

9、如果在程序代码中手动执行session_destroy函数会触发执行销毁数据操作

10、在session_start时有一定的几率触发执行gc回收

11、服务端将结果响应给客户端再头信息中增加Set-Cookie

## 3.3、修改session机制

修改session机制即设置session操作数据的存储介质。修改整套机制只能使用session_set_save_handler函数完

成。该函数需要传递6个函数分别实现session机制中6个操作

1、修改机制

```php
<?php 
	// session的机制修改必须要在 session_start之前完成
	// 修改session机制
	session_set_save_handler('open', 'close', 'read', 'write', 'destroy', 'gc');
	// 打开连接
	function open(){
		echo 'open<hr/>';
		return true;
	}
	// 关闭连接
	function close(){
		echo 'close<hr/>';
		return true;
	}
	// 从存储介质中读取数据，然后反序列化存储到$_SESSION变量中
	function read(){
		echo 'read<hr/>';
		return '';
	}
	// 将$_SESSION变量的数据序列化之后写入存储介质
	function write(){
		echo 'write<hr/>';
		return true;
	}
	// 手动销毁数据
	function destroy(){
		echo 'destroy<hr/>';
		return true;
	}
	// 垃圾回收
	function gc(){
		echo 'gc<hr/>';
		return true;
	}
	// 开启机制
	session_start();

?>
```

2、结果

![1572332045737](/1572332045737.png)

3、修改代码

![1572332080322](/1572332080322.png)

结果

![1572332191521](/1572332191521.png)

4、手动销毁数据

![1572332249844](/1572332249844.png)

结果

![1572332271704](/1572332271704.png)

# 4、案例使用session

## 4.1、解决验证码的问题

由于验证码字符采用明文方式存储。如果恶意入侵者直接根据cookie的数据读取验证码提交。最终导致项目存在

漏洞。为了安全考虑可以将验证码的字符存储到session中

1、将验证码字符存储使用session

![1572333979855](/1572333979855.png)



2、验证码比对使用session

![1572334045630](/1572334045630.png)

## 4.2、解决首页的校验问题

当前登录完成之后使用cookie存储用户的状态。后期需要登录的以cookie能否取到内容验证是否登录。为了安全

起见可以把保存状态的方式使用session存储。但是使用session存储又不方便实现七天免登录，最终中和考虑可以

使用cookie存储状态但是不能直接明文存储需要进行加密处理

1、创建加密解密的函数文件

```php
<?php 
/**

 * @param $string    要加密/解密的字符串

 * @param string $operation   类型，ENCODE 加密；DECODE 解密

 * @param string $key    密匙

 * @param int $expiry    有效期

 * @return string

 */

function authcode($string, $operation = 'DECODE', $key = 'encrypt', $expiry = 0)

{

    // 动态密匙长度，相同的明文会生成不同密文就是依靠动态密匙

    $ckey_length = 4;

    // 密匙

    $key = md5($key ? $key : $GLOBALS['discuz_auth_key']);

    // 密匙a会参与加解密

    $keya = md5(substr($key, 0, 16));

    // 密匙b会用来做数据完整性验证

    $keyb = md5(substr($key, 16, 16));

    // 密匙c用于变化生成的密文

    $keyc = $ckey_length ? ($operation == 'DECODE' ? substr($string, 0, $ckey_length) :

        substr(md5(microtime()), -$ckey_length)) : '';

    // 参与运算的密匙

    $cryptkey = $keya . md5($keya . $keyc);

    $key_length = strlen($cryptkey);

    // 明文，前10位用来保存时间戳，解密时验证数据有效性，10到26位用来保存$keyb(密匙b)，

    //解密时会通过这个密匙验证数据完整性

    // 如果是解码的话，会从第$ckey_length位开始，因为密文前$ckey_length位保存 动态密匙，以保证解密正确

    $string = $operation == 'DECODE' ? base64_decode(substr($string, $ckey_length)) :

        sprintf('%010d', $expiry ? $expiry + time() : 0) . substr(md5($string . $keyb), 0, 16) . $string;

    $string_length = strlen($string);

    $result = '';

    $box = range(0, 255);

    $rndkey = array();

    // 产生密匙簿

    for ($i = 0; $i <= 255; $i++) {

        $rndkey[$i] = ord($cryptkey[$i % $key_length]);

    }

    // 用固定的算法，打乱密匙簿，增加随机性，好像很复杂，实际上对并不会增加密文的强度

    for ($j = $i = 0; $i < 256; $i++) {

        $j = ($j + $box[$i] + $rndkey[$i]) % 256;

        $tmp = $box[$i];

        $box[$i] = $box[$j];

        $box[$j] = $tmp;

    }

    // 核心加解密部分

    for ($a = $j = $i = 0; $i < $string_length; $i++) {

        $a = ($a + 1) % 256;

        $j = ($j + $box[$a]) % 256;

        $tmp = $box[$a];
        $box[$a] = $box[$j];
        $box[$j] = $tmp;
        // 从密匙簿得出密匙进行异或，再转成字符
        $result .= chr(ord($string[$i]) ^ ($box[($box[$a] + $box[$j]) % 256]));

    }

    if ($operation == 'DECODE') {

        // 验证数据有效性，请看未加密明文的格式

        if ((substr($result, 0, 10) == 0 || substr($result, 0, 10) - time() > 0) &&
            substr($result, 10, 16) == substr(md5(substr($result, 26) . $keyb), 0, 16)
        ) {
            return substr($result, 26);
        } else {

            return '';
        }

    } else {

        // 把动态密匙保存在密文里，这也是为什么同样的明文，生产不同密文后能解密的原因

        // 因为加密后的密文可能是一些特殊字符，复制过程可能会丢失，所以用base64编码

        return $keyc . str_replace('=', '', base64_encode($result));

    }

}

?>
```



2、针对登录之后进行cookie内容加密

![1572334842705](/1572334842705.png)

3、对校验cookie内容进行解密

![1572334858608](/1572334858608.png)

