# PHP環境架設(搭配FPM、Nginx、MySQL、Redis)

# 目錄規劃


下載或要安裝的資源會放在
```bash
/usr/local/src
```

![image](./images/20201228224530.png)

nginx會安裝在(也是預設的安裝目錄)
```bash
/usr/local/nginx
```

![image](./images/20201228225938.png)

php會安裝在
```bash
/usr/local/php

or 

/usr/local/webserver/php
```

![image](./images/20201228232710.png)

# Nginx下載

## 使用版本1.12.2

http://nginx.org/en/download.html

![image](./images/20201228212241.png)

## 下載並安裝pcre依賴

### 在linux輸入指令

```bash
wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
```

### 或直接下載下來，直接在網址輸入

http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz

![image](./images/20201228212644.png)

# 安裝Nginx

## 安裝過程，請參考之前的文檔

https://github.com/IvesShe/NginxStudy

## 安裝完要打開80端口

```bash
# 查看開放的端口號
firewall-cmd --list-all

# 打開80端口(nginx預設端口)
sudo firewall-cmd --add-port=80/tcp --permanent

# 重啟防火牆
firewall-cmd --reload
```

## 安裝完成，測試畫面

![image](./images/20201228225823.png)

![image](./images/20201228225839.png)

### 常用指令
```bash
# 使用nginx操作命令前提條件，必須進入nginx的目錄
cd /usr/local/nginx/sbin

# 查看nginx的版本號
./nginx -v

# 啟動nginx
./nginx

# 查看nginx進程狀況
ps -ef | grep nginx

# 關閉nginx的
./nginx -s stop

# 配置文件位置
cd /usr/local/nginx/conf

# 查看有無nginx資料
find -name nginx 

# 強制關閉nginx
pkill -9 nginx 
```

![image](./images/20201228212644.png)
# PHP下載

## 安裝版本5.3.27
### 下面文檔的部分變7.2.34是因為更換過版本，但都通用，只需更改對應的版號資料夾

https://museum.php.net/php5/

![image](./images/20201228213139.png)

7.2.34

https://www.php.net/distributions/php-7.2.34.tar.gz

## 新增php資料夾

在/usr/local 

or 

/usr/local/webserver 

```bash
mkdir php
```

![image](./images/20201228230135.png)

## 解壓縮php-5.3.2.tar.gz

```bash
tar -xvf php-5.3.27.tar.gz
```

![image](./images/20201228230419.png)

![image](./images/20201228230504.png)

## 進入/usr/local/php-5.3.2資料夾

依自己的版本更改名稱

```bash
/usr/local/php-5.3.2    # -> 進行設定、組譯並安裝

/usr/local/php          # -> 最後安裝的目錄

or

/usr/local/webserver/php          # -> 最後安裝的目錄
```
```

## 設定要編譯的php相關套件
```bash
# 這個過程如果有出現報錯的話，請依照報錯的信息提示
# 上google查詢，將所需要的套件或依賴包安裝完成
# 完全無報錯時，才能順利完成configure
# 注意:換行的\前面必須要有空格
./configure --prefix=/usr/local/php \
--with-gd \
--enable-gd-native-ttf \
--enable-gd-jis-conv \
--enable-fpm
```

![image](./images/20201228230555.png)

有報錯，再安裝對應的依賴包

![image](./images/20201228230636.png)

![image](./images/20201228230741.png)

![image](./images/20201228230809.png)

再重新configure

![image](./images/20201228230945.png)

configure成功了!!

![image](./images/20201228231031.png)

### 所需要的依賴包可能有(參考用)

```bash
yum install gd
yum install gd-devel
yum install libxml2
yum install libxml2-devel -y
yum install libpng
yum install libpng-devel
```


## 編譯並進行安裝
```bash
make && make install
```

![image](./images/20201228231248.png)

![image](./images/20201228231710.png)

## 拷貝php設定檔
在/usr/local/php(configure設定的安裝的目錄)的目錄下

or 

/usr/local/webserver/php
```bash
# cp /usr/local/src/php-5.3.27/php.ini-development ./lib/php.ini
# cp /usr/local/src/php-7.2.18/php.ini-development ./lib/php.ini
# cp /usr/local/src/php-7.2.34/php.ini-development ./lib/php.ini
cp /usr/local/src/php-7.2.34/php.ini-development /usr/local/php/etc/php.ini

or

cp /usr/local/src/php-7.2.34/php.ini-development /usr/local/webserver/php/etc/php.ini
```

![image](./images/20201228231919.png)

## 拷貝php-fpm設定檔
```bash
# 在./configure時有加--enable-fpm，才會有php-fpm的檔案
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf

or

cp /usr/local/webserver/php/etc/php-fpm.conf.default /usr/local/webserver/php/etc/php-fpm.conf
```

![image](./images/20201228231942.png)

# PHP7.2.18(適用php7以上)不同處(多拷貝2個檔案)
```bash
#cp php-fpm.conf.default /usr/local/webserver/php/etc/php-fpm.conf

cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf

or

cp /usr/local/webserver/php/etc/php-fpm.d/www.conf.default /usr/local/webserver/php/etc/php-fpm.d/www.conf

```
## 執行php-fpm

/usr/local/php/sbin/

or

/usr/local/webserver/php/sbin/
```bash
#./sbin/php-fpm

/usr/local/php/sbin/php-fpm

or

/usr/local/webserver/php/sbin/php-fpm
```

## 查看進程
```bash
ps aux|grep /php
ps -ef|grep php
```

![image](./images/20201228232555.png)

## 若安裝失敗的話
```bash
# php若要移除的話只需要移除/usr/local/php這個資料夾即可
# 在 /usr/local的目錄時，下指令
rm -rf php/*
```

# 若php -v看不到版本的話

```bash
cp /usr/local/php/bin/php /usr/bin/php

or

cp /usr/local/webserver/php/bin/php /usr/bin/php
```

# 修改nginx設定檔


```bash
# 在設定檔中搜尋php，找到這段並將註解打開，修改如下
# /usr/local/nginx/html/這目錄應為可變，將對應的index.php放入其中
location ~ \.php$ {
           #root           html;
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME  /usr/local/nginx/html/$fastcgi_script_name;
           include        fastcgi_params;
        }
```

![image](./images/20201228233026.png)

要記得重啟

![image](./images/20201228233246.png)

## index.php內容

將php放至/usr/local/nginx/html/目錄(對應nginx.conf的設定)
```php
<?php
phpinfo();
```

![image](./images/20201228233536.png)

![image](./images/20201228233502.png)

## 執行畫面

http://192.168.160.128/index.php

![image](./images/20201228233620.png)

# 搭配MYSQL

## 下載mysql

```bash
 wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

或直接下載

![image](./images/20201230110135.png)

再傳到/usr/local/src內

![image](./images/20201230110334.png)

## 安装所需要的Yum Repository

```bash
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

![image](./images/20201230110032.png)

![image](./images/20201230110053.png)


## 開始安裝MYSQL服務器

```bash
yum -y install mysql-community-server
```

![image](./images/20201230110846.png)

![image](./images/20201230111112.png)

## 啟動MYSQL服務器

```bash
systemctl start  mysqld.service
```

或

```bash
service mysqld start
```

查看MYSQL運行狀態

```bash
systemctl status mysqld.service
```

![image](./images/20201230111427.png)

## 查看版本

```bash
mysqladmin --version
```

![image](./images/20201230114955.png)
## 查看MYSQL密碼

```bash
grep "password" /var/log/mysqld.log
```

![image](./images/20201230111847.png)

## 首次進入MYSQL

```bash
mysql -uroot -p
```

![image](./images/20201230112227.png)

修改默認的密碼之後，才能操作數據庫

```bash
# new password 處填入您自己的新密碼
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```


```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

設置失敗了

![image](./images/20201230112817.png)

MYSQL有密碼設置的規範，具體是與validate_password_policy的值有關

可以通過以下命令設置

```bash
set global validate_password_policy=0;
set global validate_password_length=1;
```

![image](./images/20201230112559.png)

再設定一次，成功通過了

![image](./images/20201230112634.png)

刷新權限，使新的密碼生效

```bash
flush privileges;
```

![image](./images/20201230113311.png)


## 設置允許外部ip通過root帳號訪問數據庫

```bash
use mysql;
UPDATE user SET `Host` = '%' WHERE `User` = 'root';
```

![image](./images/20201230113819.png)

## 查看MYSQL運行狀態

```bash
ps -ef|grep mysql
```

![image](./images/20201230113943.png)

## 查看所使用的網路端口

```bash
# 查看當前所有tcp端口
netstat -ntlp   

# 查看所有3306端口使用情況
netstat -ntulp | grep 3306

# 查看當前所有tcp端口哪些進程佔用
netstat -antp
```

![image](./images/20201230115250.png)

## 開啟防火牆對應端口

```bash
# 查看開放的端口號
firewall-cmd --list-all

# 打開80端口(nginx預設端口)
sudo firewall-cmd --add-port=3306/tcp --permanent

# 重啟防火牆
firewall-cmd --reload
```

![image](./images/20201230114826.png)


# 安裝Redis

可參考之前的文檔，直接往下拉，到安裝5.0.8版本的部分

https://github.com/IvesShe/RedisStudy


## 移動檔案到/usr/local/src

![image](./images/20201230122405.png)

## 解壓縮

```bash
tar -xvf redis-5.0.8.tar.gz 
```
![image](./images/20201230122352.png)

## 檢查gcc版本


```bash
gcc -v
```
![image](./images/20201230122541.png)

若無安裝gcc的話需先安裝

```bash
yum install gcc-c++
```

## 安裝redis

在解壓的redis目錄下

```bash
make
```

![image](./images/20201230122816.png)

![image](./images/20201230122948.png)

## 指定安裝目錄

會用預設目錄安裝
```bash
make install
```

重新下載及安裝，安裝時下參數，指定安裝資料夾
```bash
make install PREFIX=/usr/local/server/redis

or

make install PREFIX=/usr/local/webserver/redis
```

![image](./images/20201230123555.png)


## 設置redis服務後台啟動

![image](./images/20201230132336.png)

設置daemonize yes(先本的conf可以先備份起來)

![image](./images/20201230132319.png)


## 啟動服務

在redis(/usr/local/src/redis-5.0.8/帶版號的那個)目錄下

```bash
src/redis-server redis.conf
```

![image](./images/20201230132551.png)


# 用新的configure重新安裝php

## 若要查看舊系統的configure

```bash
# 可以查看該目錄的原始設定
php/bin/php-config
```

## configure或make失敗的話

可以通過以下指令，刪除編譯過的文件，然後重新用 ./configure配置
```bash
make clean
rm -f Makefile
```

![image](./images/20201230142844.png)


另外可透過參數先make，再make install

```bash
make ZEND_EXTRA_LIBS='-liconv'
```
## 移除掉/usr/local/php的所有檔案

在/usr/local/
```bash
rm -rf php/*
```

舊的參考用
```bash
./configure --prefix=/usr/local/php \
--with-gd \
--enable-gd-native-ttf \
--enable-gd-jis-conv \
--enable-fpm
```

新的版本1
```bash
./configure \
--prefix=/usr/local/php \
--with-config-file-path=/usr/local/php/etc \
--with-zlib-dir \
--with-freetype-dir \
--enable-mbstring \
--with-libxml-dir=/usr \
--enable-xmlreader \
--enable-xmlwriter \
--enable-soap \
--enable-calendar \
--with-curl \
--with-zlib \
--with-gd \
--with-pdo-sqlite \
--with-pdo-mysql \
--with-mysqli \
--with-mysql-sock \
--enable-mysqlnd \
--disable-rpath \
--enable-inline-optimization \
--with-bz2 \
--with-zlib \
--enable-sockets \
--enable-sysvsem \
--enable-sysvshm \
--enable-pcntl \
--enable-mbregex \
--enable-exif \
--enable-bcmath \
--with-mhash \
--enable-zip \
--with-pcre-regex \
```

![image](./images/20201230133956.png)

報錯，補安裝相關依賴

![image](./images/20201230134016.png)

```bash
yum install bzip2-devel

```
![image](./images/20201230134157.png)

報錯，補安裝相關依賴

![image](./images/20201230134313.png)

```bash
yum install curl curl-devel
```

![image](./images/20201230134407.png)

報錯，補安裝相關依賴

![image](./images/20201230134637.png)

```bash
yum install freetype-devel
```

![image](./images/20201230134800.png)

報錯，補安裝相關依賴

![image](./images/20201230134936.png)

```bash
yum install mysql-devel
```

![image](./images/20201230140854.png)

其它報錯安裝依賴參考
```bash
yum -y install libjpeg-devel
```

## configure: error: Please reinstall libedit - I cannot find readline.h 報錯特殊解法

```bash
# 先試
yum install -y readline-devel

# 沒用的話直接捉源碼 可從網頁輸入捉去
wget http://thrysoee.dk/editline/libedit-20170329-3.1.tar.gz

# 在/usr/local/src解壓
tar -xvf libedit-20170329-3.1.tar.gz

# 編譯
./configure

# 安裝
make && make install
```

![image](./images/20201230153124.png)

## 註：新的版本一直試不成功，最後改用 新的版本2 先安裝通過了
## 終於configure成功了

![image](./images/20201230141342.png)

## 安裝

```bash
make && make install
```

![image](./images/20201230141550.png)

![image](./images/20201230143828.png)


## 拷貝php設定檔
在/usr/local/php(configure設定的安裝的目錄)的目錄下
```bash
cp /usr/local/src/php-5.3.27/php.ini-development ./lib/php.ini
```

![image](./images/20201228231919.png)

## 拷貝php-fpm設定檔
```bash
# 在./configure時有加--enable-fpm，才會有php-fpm的檔案
cp etc/php-fpm.conf.default etc/php-fpm.conf
```

![image](./images/20201228231942.png)
## 執行php-fpm

要記得先新增user
```bash
adduser www
```

/usr/local/php/sbin/

or

/usr/local/webserver/php/sbin/
```bash
./sbin/php-fpm
```

![image](./images/20201230144110.png)

## 查看進程
```bash
ps aux|grep /php
```

## 新版測試畫面

![image](./images/20201230144423.png)

# 安裝Composer

參考之前的文檔

https://github.com/IvesShe/PHP_Linux_Study


# Nginx第三方模塊編譯

查看nginx編譯設定

```bash
/usr/local/nginx/sbin/nginx -V
```
