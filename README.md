# wisdom

# Deployment Instructions
* #### [Login](#login-1)
* #### [Update](#update-1)
* #### [Apache](#apache-1)
* #### [Node](#node-1)
* #### [Php](#php-1)
* #### [Mysql](#mysql-1)
* #### [PhpMyAdmin](#phpmyadmin-1)
* #### [Secure PhpMyAdmin](#secure-phpmyadmin-1)
* #### [RVM](#rvm-1)
* #### [Ruby](#ruby-1)
* #### [Rails](#rails-1)
* #### [Swap Space](#swap-space-1)
* #### [Git](#git-1)
* #### [Passenger](#passenger-1)
* #### [Dbf](#dbf-1)
* #### [Store Dbf Folder](#store-dbf-folder-1)
* #### [Mysql Gem](#mysql-gem-1)
* #### [Mysql Permission](#mysql-permission-1)
* #### [Virtual Host](#virtual-host-1)
* #### [Capistrano](#capistrano-1)
* #### [Whenever](#whenever-1)
* #### [Timezone](#timezone-1)
* #### [Deployment](#deployment-1)
* #### [Deployment Problems](#deployment-problems-1)
* #### [Folders to be created](#folders-to-be-created-1)
* #### [SSL Certificates](#ssl-certificates-1)
* #### [PdfTk](#pdftk-1)
* #### [DNS Problem](#dns-problem-1)
* #### [Backup](#backup-1)
* #### [Restore](#restore-1)
* #### [Sitemap](#sitemap-1)
* #### [Important Locations](#important-locations-1)

## Login
`ssh user@ip_address`
## Update
`sudo apt-get update`
## Apache
`sudo apt-get install apache2`
## Node
* `sudo apt-get install nodejs`
* `sudo apt-get install npm`
## Php
`sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql`
## Mysql
`sudo apt-get install mysql-server`
* Enter password and confirm password
## PhpMyAdmin
`sudo apt-get install phpmyadmin php-mbstring php-gettext`
* select **apache**
* select **"yes"** when asked whether to use dbconfig-common to set up the database
* enter password and confirm password

* `sudo phpenmod mcrypt`
* `sudo phpenmod mbstring`
* `sudo systemctl restart apache2`
* `sudo ln -s /usr/share/phpmyadmin /var/www`
## Secure PhpMyAdmin
`sudo nano /etc/apache2/conf-available/phpmyadmin.conf` if no lines are there in first file try second `sudo nano /etc/phpmyadmin/apache.conf`
* Add `AllowOverride All` after `<Directory /usr/share/phpmyadmin> Options 							FollowSymLinks DirectoryIndex index.php ....`

`sudo systemctl restart apache2`

#### create htaccess file
`sudo nano /usr/share/phpmyadmin/.htaccess`
* add the below code
	`AuthType Basic`
	`AuthName "Restricted Files"`
	`AuthUserFile /etc/phpmyadmin/.htpasswd`
	`Require valid-user`

`sudo apt-get install apache2-utils`
`sudo htpasswd -c /etc/phpmyadmin/.htpasswd username`
* enter password and confirm password

[Reference url](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04)
## RVM
* `sudo apt-add-repository -y ppa:rael-gc/rvm`
* `sudo apt-get update`
* `sudo apt-get install rvm`
* `source /etc/profile.d/rvm.sh` or source the file shown in the terminal
## Ruby
`rvm install 2.4.1`
## Rails
`gem install rails`
## Swap Space
* `sudo dd if=/dev/zero of=/swap bs=1M count=1024`
* `sudo mkswap /swap`
* `sudo swapon /swap`
## Git
`sudo apt-get install git`
## Passenger
* `gem install passenger`
* `passenger-install-apache2-module`
* if any problem install things shown in terminal 
* **add specified lines** shown in terminal to specified files
## Dbf
* `sudo apt-get install php7.0-dev`
* `sudo pecl install dbase-7.0.0beta1`
* add `extension=dbase.so` in **/etc/php/7.0/apache2/php.ini**
[Reference url](https://www.yinfor.com/2017/04/simple-way-install-dbase-extension-php7-0.html)
## Store Dbf Folder
* from the folder where dbf.zip is available do 
`scp dbf.zip root@ip_address:/var/www`
* goto /var/www/ and do `unzip dbf.zip`
* change permissions from **root** to **www-data**
* change **dbf.yml** file to appropriate address
## Mysql Gem
* `sudo apt-get install libmysqlclient-dev`
* `gem install mysql2 -v '0.4.10'`
## Mysql Permission
Mysql Permission for other servers to access
* edit **sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf**
`#bind-address           = 127.0.0.1`
`#skip-networking`
* `service mysql restart`
* `GRANT ALL PRIVILEGES ON *.* TO 'user'@'ip_address' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;`
`FLUSH PRIVILEGES;`
* `service mysql restart`
* [Reference url](https://easyengine.io/tutorials/mysql/remote-access/)
## Virtual Host
* copy contents of **/etc/apache2/sites-available/000.cnf** from old server and paste in new server
* `sudo service apache2 restart`
## Capistrano
`gem install capistrano`
## Whenever
`gem install whenever`
## Timezone
`sudo dpkg-reconfigure tzdata`
## Deployment Problems
1. **Rvm problems**
-try to **change path** of rvm in **deploy.rb** file
-set rvm path to **:rvm_type** or **:rvm_ruby_version** or **:rvm_custom_path**

2. **Unknown db error**
-**manually create a db** after login inside mysql
`CREATE DATABASE database_name;`
## Folders to be created
Folders to be created in **shared** and change **root to www-data**
* Feeds
* Uploads
## SSL Certificates
* Copy all necessary certificates into the `apache/certs`
* `sudo a2enmod ssl`
* `sudo a2enmod rewrite && sudo service apache2 restart`
## Pdftk
`sudo apt-get install pdftk`
## DNS Problem
* `sudo nano /etc/resolv.conf `
* add `nameserver 8.8.4.4`
* add `nameserver 8.8.8.8`
## Backup
`mysqldump -u root -ppassword database_name > /var/www/inn/current/tmp/my_database_dump.sql`
## Restore
`mysql -h ip_address -u root -ppassword database_name < /var/www/inn/current/tmp/my_database_dump.sql`
## Sitemap
`rake sitemap:refresh:no_ping`
## Important Locations
* `/etc/apache2/apache2.conf` - Apache config file
* `/etc/apache2/sites-available/000-default.conf` - Virtual host file
* `/etc/environment` - Environment file
* `/etc/php/7.0/apache2/php.ini` - Php config file
* `/etc/mysql/mysql.conf.d/mysqld.cnf` - Mysql config file
* `/var/log/apache2/access.log` - Apache log files
* `/var/log/apache2/error.log` - Apache log files
