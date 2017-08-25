##### The ./configure command
```
./configure
    --prefix=/usr/local/php5.6.3
    --enable-opcache
    --enable-fpm
    --with-gd
    --with-zlib
    --with-jpeg-dir=/usr
    --with-png-dir=/usr
    --with-pdo-mysql=mysqlnd
    --enable-mbstring
    --enable-sockets
    --with-curl
    --with-mcrypt
    --with-openssl;
./configure --help   //for help
```

##### Build essentials
We’ll need these fundamental software binaries to build PHP on your operating
system. These binaries include gcc, automake, and other fundamental develop‐
ment software:

```
# Ubuntu
sudo apt-get install build-essential;
# CentOS
sudo yum groupinstall "Development Tools";
```

##### libxml2
We’ll need the libxml2 library. This is used by PHP’s XML-related functions:
```
# Ubuntu
sudo apt-get install libxml2-dev;
# CentOS
sudo yum install libxml2-devel;
```
##### OpenSSL
We’ll need the openssl library. This is required to enable HTTPS stream wrappers in PHP, which is kind of important, right?
```
# Ubuntu
sudo apt-get install libssl-dev;
# CentOS
sudo yum install openssl-devel;
```
Curl
We’ll need the libcurl library. This is required by PHP’s Curl functions:
```
# Ubuntu
sudo apt-get install libcurl4-dev;
# CentOS
sudo yum install libcurl-devel;
```
##### Image manipulation
We’ll need the GD, JPEG, PNG, and other image-related system libraries. Fortunately, all of these are bundled into a single package. These are required to
manipulate images with PHP:
```
# Ubuntu
sudo apt-get install libgd-dev;
# CentOS
sudo yum install gd-devel;
```
##### Mcrypt
We’ll need the mcrypt system library to enable PHP’s Mcrypt encryption and
decryption functions. For whatever reason, there is no default CentOS Mcrypt
package. We’ll need to supplement the default CentOS packages with the thirdparty EPEL package repository to install Mcrypt:
```
# Ubuntu
sudo apt-get install libmcrypt-dev;
# CentOS
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm;
sudo rpm -Uvh epel-release-6*.rpm;
sudo yum install libmcrypt-devel;
```
