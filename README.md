# drupal-installation-guide
# Installing Drupal on Ubuntu Linux

This documentation provides a step-by-step guide on installing Drupal 10 with PHP 8.2 on an Ubuntu Linux system and setting up a second website once the first one is successfully configured.

---

## Prerequisites
Before you begin, ensure you have the following:
- An Ubuntu Linux machine
- Sudo privileges
- Apache, MariaDB, and PHP 8.2 installed

### Step 1: Update System Packages
```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Required Packages
```bash
sudo apt install apache2 mariadb-server php8.2 libapache2-mod-php8.2 php8.2-mysql php8.2-xml php8.2-gd php8.2-curl php8.2-mbstring php8.2-zip unzip composer -y
```

### Step 3: Secure the Database Server
```bash
sudo mysql_secure_installation
```
Follow the prompts to enhance security.

### Step 4: Create a Database for Drupal
```bash
sudo mariadb -u root -p
```
Inside the MariaDB prompt, run:
```sql
CREATE DATABASE drupal;
CREATE USER 'drupaluser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON drupal.* TO 'drupaluser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 5: Download and Set Up Drupal Using Composer
```bash
cd /var/www/html
sudo composer create-project drupal/recommended-project:10 drupal
sudo chown -R www-data:www-data /var/www/html/drupal
sudo chmod -R 755 /var/www/html/drupal
```

### Step 6: Configure Apache for Drupal
Create a new virtual host configuration file:
```bash
sudo nano /etc/apache2/sites-available/drupal.conf
```
Add the following configuration:
```apache
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    DocumentRoot /var/www/html/drupal/web
    ServerName localhost

    <Directory /var/www/html/drupal/web>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Save and exit.

### Step 7: Enable Configuration and Restart Apache
```bash
sudo a2ensite drupal.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### Step 8: Complete the Installation via Web Browser
Open a browser and visit:
```
http://localhost
```
Follow the on-screen instructions to configure Drupal.

---

# Creating a Second Drupal Website

### Step 1: Create a New Drupal Directory Using Composer
```bash
cd /var/www/html
sudo composer create-project drupal/recommended-project:10 mall_drupal
sudo chown -R www-data:www-data /var/www/html/mall_drupal
```

### Step 2: Create a New Database
```bash
sudo mariadb -u root -p
```
Inside the MariaDB prompt, run:
```sql
CREATE DATABASE mall_drupal;
CREATE USER 'malluser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON mall_drupal.* TO 'malluser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 3: Configure Apache for the Second Website
```bash
sudo nano /etc/apache2/sites-available/mall.conf
```
Add the following configuration:
```apache
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    DocumentRoot /var/www/html/mall_drupal/web
    ServerName mall.localhost

    <Directory /var/www/html/mall_drupal/web>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/mall_error.log
    CustomLog ${APACHE_LOG_DIR}/mall_access.log combined
</VirtualHost>
```
Save and exit.

### Step 4: Enable the New Site and Restart Apache
```bash
sudo a2ensite mall.conf
sudo systemctl restart apache2
```

### Step 5: Complete the Installation via Web Browser
Open a browser and visit:
```
http://mall.localhost
```
Follow the on-screen instructions to configure the second Drupal site.

---

This guide ensures you have a fully functional Drupal 10 installation with PHP 8.2 and the ability to add additional sites as needed. Let me know if you need any modifications!

