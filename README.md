### 安装

```
cd /www/wwwroot/sspanel
git clone -b new_master https://github.com/PaPaPR/ss-panel-v3-mod.git tmp && mv tmp/.git . && rm -rf tmp && git reset --hard
chown -R root:root *
chmod -R 755 *
chown -R www:www storage
php composer.phar install
mv tool/alipay-f2fpay vendor/
mv -f tool/cacert.pem vendor/guzzle/guzzle/src/Guzzle/Http/Resources/
mv -f tool/autoload_classmap.php vendor/composer/
```
### 伪静态

```
location / 
{
    try_files $uri $uri/ /index.php$is_args$args;		                
}

location ~ [^/]\.php(/|$)
{
    # comment try_files $uri =404; to enable pathinfo
    try_files $uri =404;
    fastcgi_pass  unix:/tmp/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
    #include pathinfo.conf;
}
```

### 网站目录

```
/www/wwwroot/sspanel/public
```
修改防跨目录

```
chattr -i .user.ini
mv .user.ini public
cd public
chattr +i .user.ini
```

重启nginx

### 安装依赖

```
php composer.phar install
```

### 修改配置文件
```
cp config/.config.php.example config/.config.php
```
### crontab -e

```
30 22 * * * php /www/wwwroot/sspanel/xcat sendDiaryMail 
*/1 * * * * php /www/wwwroot/sspanel/xcat synclogin
*/1 * * * * php /www/wwwroot/sspanel/xcat syncvpn
0 0 * * * php /www/wwwroot/sspanel/xcat dailyjob
*/1 * * * * php /www/wwwroot/sspanel/xcat checkjob    
*/1 * * * * php /www/wwwroot/sspanel/xcat syncnas
```
```
/etc/init.d/cron restart
```
### 时区
```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
### 命令

```
php xcat createAdmin          //创建管理员
php xcat syncusers            //同步用户
php xcat initQQWry            //下载IP解析库
php xcat resetTraffic         //重置流量
php xcat initdownload         //下载ssr客户端
```