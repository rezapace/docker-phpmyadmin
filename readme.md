Berikut adalah script `.sh` yang menggabungkan semua perintah yang Anda sebutkan:

```bash
#!/bin/bash

# Update and upgrade system packages
apt-get update && apt-get upgrade -y

# Install necessary packages
apt install -y nano
apt-get install -y apache2
apt-get install -y mysql-server

# Set timezone and install PHP with necessary modules
echo "Asia/Jakarta" > /etc/timezone
DEBIAN_FRONTEND=noninteractive apt-get install -y php libapache2-mod-php php-mysql tzdata
dpkg-reconfigure -f noninteractive tzdata

# Install additional PHP modules
apt-get install -y php-cli php-curl php-gd php-mbstring php-xml php-zip

# Start Apache and MySQL services
service apache2 start
service mysql start

# Install phpMyAdmin with non-interactive configuration
echo "phpmyadmin phpmyadmin/dbconfig-install boolean true" | debconf-set-selections && \
echo "phpmyadmin phpmyadmin/app-password-confirm password p" | debconf-set-selections && \
echo "phpmyadmin phpmyadmin/mysql/admin-pass password p" | debconf-set-selections && \
echo "phpmyadmin phpmyadmin/mysql/app-pass password p" | debconf-set-selections && \
echo "phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2" | debconf-set-selections && \
DEBIAN_FRONTEND=noninteractive apt-get install -y phpmyadmin

# Create symbolic link for phpMyAdmin
ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

# Adjust permissions for MySQL
chmod 755 /var/run/mysqld/
chmod 660 /var/run/mysqld/mysqld.sock

# Configure phpMyAdmin
echo "Configuring phpMyAdmin..."
sed -i "/$cfg\['Servers'\]\[\$i\]\['host'\] = 'localhost';/c\$cfg['Servers'][\$i]['host'] = 'localhost';" /etc/phpmyadmin/config.inc.php
sed -i "/$cfg\['Servers'\]\[\$i\]\['socket'\] = '';/c\$cfg['Servers'][\$i]['socket'] = '/var/run/mysqld/mysqld.sock';" /etc/phpmyadmin/config.inc.php
sed -i "/$cfg\['Servers'\]\[\$i\]\['user'\] = '';/c\$cfg['Servers'][\$i]['user'] = 'root';" /etc/phpmyadmin/config.inc.php
sed -i "/$cfg\['Servers'\]\[\$i\]\['password'\] = '';/c\$cfg['Servers'][\$i]['password'] = 'p';" /etc/phpmyadmin/config.inc.php

# Secure MySQL installation
mysql -u root -p <<EOF
SELECT user, host, plugin FROM mysql.user WHERE user = 'root';
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'p';
FLUSH PRIVILEGES;
EOF

# Restart Apache and MySQL services
service apache2 restart
service mysql restart

echo "Installation and configuration completed."
```

**Cara menggunakan:**

1. Simpan script ini sebagai file `.sh`, misalnya `install_phpmyadmin.sh`.
2. Berikan izin eksekusi pada file tersebut:
   ```bash
   chmod +x install_phpmyadmin.sh
   ```
3. Jalankan script:
   ```bash
   ./install_phpmyadmin.sh
   ```

Script ini akan mengotomatisasi proses instalasi dan konfigurasi server Apache, MySQL, PHP, dan phpMyAdmin dengan pengaturan yang Anda sebutkan.
