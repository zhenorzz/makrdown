### c扩展

```
#vim hello.c
```  
```
#include <stdio.h>
int hello(char *name)  
{  
    printf("hello %s\n", name); 
}  
```
```
#gcc -O -c -fPIC -o hello.o hello.c                      // -fPIC：是指生成的动态库与位置无关  
#gcc -shared -o libhello.so hello.o                     // -shared：是指明生成动态链接库  
#cp libhello.so /usr/local/lib      // 把生成的链接库放到指定的地址  
#ldconfig                // 用此命令，使刚才写的配置文件生效 
```

### 测试so库

```
#include <stdio.h>  
int main()  
{  
    char *name;
    name = "Zane";  
    hello(name);
    return 0;  
}  
```
```
#gcc -o hellotest -lhello hellotest.c                // 编译测试文件，生成测试程序  
#./hellotest           // 运行测试程序
``` 

php源码编译 （yum或者apt安装不知道行不行）   cd YourPath to php
```
#./buildconf --force //重新生成配置脚本
#./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-apxs2=/alidata/server/httpd/bin/apxs --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-static --enable-maintainer-zts --enable-inline-optimization --enable-sockets --enable-wddx --enable-zip --enable-calendar --enable-bcmath --enable-soap --with-zlib --with-iconv --with-gd --with-xmlrpc --enable-mbstring --with-curl --enable-ftp --with-mcrypt --with-freetype-dir=/usr/local/freetype.2.1.10 --with-jpeg-dir=/usr/local/jpeg.6 --with-png-dir=/usr/local/libpng.1.2.50 --disable-ipv6 --disable-debug --disable-fileinfo --with-openssl    
```
####注意：
  1. 提示什么错误就百度一下
  2. apxs是你的路径，apache是编译安装的话，通常都会有apxs，如果是包管理方式安装，则需要另外安装apache2-dev 
  3. openssl 如果安装不成功 是php版本不对，有些linux不向低兼容，只能装高版本php7

```
#make && make install //安装php
```
### php调用c扩展

```
#cd php/ext
#vim hello.proto //定义原型
```
```
string hello(string name)
```
```
#./ext_skel --extname=hello  --proto=hello.proto
#cd ./hello
#vim config.m4   去掉两行dnl注释
```
```
PHP_ARG_ENABLE(hello, whether to enable hello support,
dnl Make sure that the comment is aligned:
[  --enable-hello           Enable hello support])
```
```
#phpize
#./configure  --with-php-config=/usr/local/php/bin/php-config
#vim Makefile //在LDFLAGS 追加你的库  libhello.so  就 添加 -lhello
```

```
#vim hello.c     编写需要的c语言程序
```
```
/* {{{ proto string hello(string name)
 */
PHP_FUNCTION(hello)
{
char *name = NULL;
int argc = ZEND_NUM_ARGS();
size_t name_len;
if (zend_parse_parameters(argc, "s", &name, &name_len) == FAILURE)
 		return;
hello(name);
}
/* }}} */
```
```
#make 
#./libtool --finish /usr/local/lib/
#make install
#vim path/php.ini
```
 **二选一**  
1.允许dl()动态加载so扩展功能enable_dl = On
```
<?php  
    dl("hello.so");  
    hello('Zane');  
?>
```  
2.静态加载enable_dl = Off
需要添加 extension=hello.so
```
<?php  
    hello('Zane'); 
?> 
```
 **重启apache2,完成。** 
 
 **备注**  
 ```
    zval *arr, **data;
    HashTable *arr_hash;
    HashPosition pointer;
    if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "a", &arr) == FAILURE)
        RETURN_FALSE;
    //循环数组
    arr_hash = Z_ARRVAL_P(arr);
    for(zend_hash_internal_pointer_reset_ex(arr_hash, &pointer); 
    zend_hash_get_current_data_ex(arr_hash, (void**) &data, &pointer) == SUCCESS; 
    zend_hash_move_forward_ex(arr_hash, &pointer)) {
            convert_to_string_ex(data);
            dynamic_password[i++] =atoi(Z_STRVAL_PP(data)); 
    }
    //初始化输出数组
    array_init(return_value);
    //赋值
    add_next_index_long(return_value, 10);
    //打印
    php_printf("%d", var);
```

***参考链接*：**

[php调用C/C++动态链接库](http://www.jianshu.com/p/9a64df6bb7af)  

[用C/C++扩展你的PHP](http://www.laruence.com/2009/04/28/719.html)  

[php调用c语言编写的so动态库](http://blog.csdn.net/feisy/article/details/17927713)
