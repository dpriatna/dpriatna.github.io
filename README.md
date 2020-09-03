### Install PHP

sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo add-apt-repository ppa:ondrej/apache2

sudo apt install -y php7.4 php7.4-gd php7.4-mbstring php7.4-xml php7.4-zip php7.4-tokenizer php7.4-bcmath php7.4-json

### Apache2

sudo apt install apache2 libapache2-mod-php7.4 

### Install MySQL

sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'

sudo add-apt-repository 'deb [arch=amd64] http://mariadb.mirror.globo.tech/repo/10.5/ubuntu focal main'

sudo apt install -y mariadb-server mariadb-client php7.4-mysql

sudo mysql_secure_installation

### Install JAVA 8

sudo tar -zxvf jdk-8u256-linux-x64.tar.gz -C /usr/lib/jvm/
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-14.0.1/bin/java 1
sudo update-alternatives --config java
java -version

#### Setup Environmental Variables
sudo nano /etc/profile.d/java.sh <br>
export PATH=$PATH:/usr/lib/jvm/java-11-openjdk-amd64/bin/ <br>
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/ <br>
export J2SDKDIR=/usr/lib/jvm/java-11-openjdk-amd64/ <br>
source /etc/profile.d/java.sh <br>
<strong>place on </strong><br>
~/.bash_profile

### dbeaver
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
echo "deb https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt -y  install dbeaver-ce

### Composer

curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer

curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer

### Git Laravel
cd /var/www
git clone https://github.com/laravel/laravel.git

cd /var/www/laravel
sudo composer install

chown -R www-data.www-data /var/www/laravel
chmod -R 755 /var/www/laravel
chmod -R 777 /var/www/laravel/storage

or

cd /var/www/laravel
chmod -R 777 storage bootstrap/cache

mv .env.example .env

php artisan key:generate

Create MySQL User and Database
CREATE DATABASE laravel;
CREATE USER 'laravel'@'localhost' IDENTIFIED BY 'secret';
GRANT ALL ON laravel.* to 'laravel'@'localhost';
FLUSH PRIVILEGES;
quit

edit the .env file and update database settings.

DB_CONNECTION=mysql <br>
DB_HOST=127.0.0.1 <br>
DB_PORT=3306 <br>
DB_DATABASE=laravel <br>
DB_USERNAME=laravel <br>
DB_PASSWORD=secret

### Apache Configuration

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/example.com.conf

<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/laravel/public

        <Directory />
                Options FollowSymLinks
                AllowOverride None
        </Directory>
        <Directory /var/www/laravel>
                AllowOverride All
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

### Enable V-Host
sudo a2ensite example.com.conf <br>
sudo a2ensite test.com.conf <br>
sudo a2enmod write / rewrite <br>
sudo a2dissite 000-default.conf

sudo systemctl restart apache2

sudo service apache2 restart

sudo nano /etc/hosts

127.0.0.1   localhost <br>
127.0.1.1   guest-desktop <br>
111.111.111.111 example.com <br>
111.111.111.111 test.com 

