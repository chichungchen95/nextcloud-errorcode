在Ubuntu安裝Nextcloud
前行提要
本次使用:
網頁伺服器為apache2

1.安裝PHP擴充跟模組和資料庫
LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
apt -y install libapache2-mod-php tar unzip git php8.3 php8.3-{mysql,gd,mbstring,curl,zip,xml,fpm,intl,gmp,bcmath,exif,imagick,bz2} 

2.設置數據庫 用戶名後面的localhost可以自行判斷是否改為其他(localhost也可以運行)
mysql -u root -p
CREATE DATABASE 數據庫名稱;
CREATE USER '帳戶名稱'@'localhost' IDENTIFIED BY '密碼';
GRANT ALL PRIVILEGES ON 數據庫名稱.* TO  '帳戶名稱'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit;
 
3.下載nextcloud檔案
cd /var/www/
rm -rf html
mkdir nextcloud
cd nextcloud
wget https://download.nextcloud.com/server/releases/nextcloud-30.0.4.zip
unzip nextcloud-30.0.4.zip 
rm nextcloud-30.0.4.zip
mv nextcloud/* .
rm -r nextcloud

4.導入其他存儲硬碟(沒有需求，可以跳過)
fdisk -l <-查詢硬碟路徑
注意! 由於我的硬碟是新的 未初始化，所以這裡沒需求也可以跳過

mkfs.ext4 /dev/sdb <- /dev/sdb這裡可以改成自己的硬碟路徑

blkid /dev/sdb
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


9.安裝完成 查看錯誤問題文件

(以上為參考，如運行後出現問題與本人無關)
