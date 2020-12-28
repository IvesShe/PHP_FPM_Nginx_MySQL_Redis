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

https://gitlab.com/ivesshe/nginxstudy

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

https://museum.php.net/php5/

![image](./images/20201228213139.png)


## 在/usr/local新增php資料夾

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
```bash
/usr/local/php-5.3.2    # -> 進行設定、組譯並安裝
/usr/local/php          # -> 最後安裝的目錄
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
```bash
./sbin/php-fpm
```

## 查看進程
```bash
ps aux|grep /php
```

![image](./images/20201228232555.png)

## 若安裝失敗的話
```bash
# php若要移除的話只需要移除/usr/local/php這個資料夾即可
# 在 /usr/local的目錄時，下指令
rm -rf php/
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


# 其它資料(暫放未整理)
## corntab

https://www.google.com/search?q=crontab&rlz=1C1CHBF_zh-TWTW911TW911&oq=crontab&aqs=chrome..69i57j0l3j0i395l4.2320j1j7&sourceid=chrome&ie=UTF-8

http://linux.vbird.org/linux_basic/0430cron.php

# 參考文檔

https://segmentfault.com/a/1190000019361535

https://zhuanlan.zhihu.com/p/156936431

https://blog.gtwang.org/linux/nginx-php-fpm-configuration-optimization/