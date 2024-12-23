# Configure Ubuntu server to serve a Laravel application with the domain

1. Update and Upgrade the Server
   ```
    sudo apt update
   ```
2. Install Nginx, PHP, and MySQL:
   ```
    sudo apt install nginx mysql-server php-fpm php-mysql php-xml php-mbstring php-curl php-zip unzip -y
    sudo apt install php8.3-{cli,pdo,mysql,zip,gd,mbstring,curl,xml,bcmath,common}
   ```
3. Configure MySQL, secure your MySQL installation and create a database for your Laravel application:
   ```
    sudo mysql_secure_installation

    # Log in to MySQL
    sudo mysql -u root -p

    # Create database and assign user
    CREATE DATABASE <laravel_db>;
    CREATE USER '<laravel_user>'@'<host>' IDENTIFIED BY '<your_password>';
    GRANT ALL PRIVILEGES ON <laravel_db>.* TO '<laravel_user>'@'<host>';
    FLUSH PRIVILEGES;
    // GRANT ALL ON `amirhamzan_site`.* TO 'amirhamzan_user'@'localhost';
   ```
   ```
    # Verify user and host created
    SELECT user, host FROM mysql.user;
   ```
   ```
    # Update host and database for user in MySQL
    UPDATE mysql.user SET Host='<new_host>' WHERE Host='<old_host>' AND User='<user>';
    UPDATE mysql.db SET Host='<new_host>' WHERE Host='<old_host>' AND User='<user>';
    FLUSH PRIVILEGES;
   ```
   Save the file and restart MySQL:
   ```
    sudo systemctl restart mysql
   ```
4. Install Composer
   ```
    cd ~
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer
   ```
5. Deploy Laravel Application
   ```
    cd /var/www
    sudo git clone https://github.com/amirhamzan/real-estate-site.git amirhamzan.site
    cd amirhamzan.site
    cp .env.example .env
    composer install
    php artisan key:generate
    <next_step_configure_laravel_app ...>
   ```
6. Configure NGINX
7. Configure DNS - set A and CNAME to point to server
8. Enable HTTPS using certbot
9. Install, configure phpmyadmin

---
   ```
    ##################### TO CHECK AGAIN #####################
    # To activate ufw (Uncomplicated Firewall)
    sudo ufw enable
    # Allows incoming connections on port 3306(MySQL)
    sudo ufw allow 3306
    # Check ufw status
    sudo ufw status
   ```
   Ensure MySQL is configured to listen on all IP addresses
   ```
    # Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf or /etc/my.cnf):
    sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
   Look for the bind-address directive and set it to 0.0.0.0 or 194.238.23.69:
   ```
    bind-address = 0.0.0.0
   ```
