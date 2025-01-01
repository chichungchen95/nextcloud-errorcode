nano /etc/fstab
UUID= uuid填寫這裡  /var/www/html/data ext4 defaults 0 2
mount -a
reboot

5.給予權限
chown -R www-data:www-data /var/www/html
chmod -R 755 /var/www/html

6.啟動套件
systemctl start apache2 && systemctl enable apache2
systemctl start php8.1-fpm && systemctl enable php8.1-fpm

7.進入後台網頁
Chrome輸入 http://網頁ip/nextcloud  <-假如使用內網登入 之後用網域就需要去
/var/www/html/config/config.php修改

設定帳號名稱
數據庫帳戶名稱

設定存放位置/var/www/html/data(預設)

數據庫帳戶密碼
數據庫名稱
數據庫IP localhost(預設)

8.等待安裝+配置基本設定
等太久或是網頁卡住可以試試重新整理
nano /etc/php/8.1/apache2/php.ini
memory_limit = 512M
upload_max_filesize = 16G <-上傳大小
opcache.enable = 1
opcache.interned_strings_buffer = 8
opcache.max_accelerated_files = 10000
opcache.memory_consumption = 128
opcache.save_comments = 1
opcache.revalidate_freq = 1


9.安裝完成 如有在管理頁面看到報錯可以查看question文件
