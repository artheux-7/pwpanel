# pwpanel
## Perfect World Web

**Note from the original author:** This project is no longer supported, however I will still look at pull requests if you'd like to attempt a fix.

### THIS BUILD HAS THE PAYPAL-IPN ISSUE FIXED

### Requirements
1. Composer & Git - [Complete steps 1 & 2 on this tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-14-04)
2. PHP 5.5.9 or higher
3. Install the following Extensions `curl php5-cli git php5.6-gd php5.6-curl php5.6-mcrypt php5.6-intl php5.6-xsl`
4. execute the following command `a2enmod rewrite`

### Setup
in your apache2 setup (This was used on ubuntu, find something similar in your own distro), In `/etc/apache2/apache2.conf` enable rewrite, change none to all

```
    <Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
    </Directory>
```

if you can't get it just text: nano `/etc/httpd/conf.d/laravel.conf`
```
<VirtualHost *:80>
    ServerAdmin root@localhost
    ServerName YOUR_IP_OR_DOMAIN
    DocumentRoot /var/www/html/pwpanel/public

    <Directory /var/www/html/pwpanel/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/pwpanel_error.log
    CustomLog /var/log/httpd/pwpanel_access.log combined
</VirtualHost>

```
save, run: `sed -i 's/^#LoadModule rewrite_module/LoadModule rewrite_module/' /etc/httpd/conf/httpd.conf`

Download the latest release and upload the files.

First you need to rename `.env.example` to `.env` 
or `cp .env.example .env`

Then set the permissions to 777 for the following directories/files:
`sudo chmod -R 775 /var/www/html/pwpanel/`

- storage/app/
- storage/framework/
- storage/logs/
- bootstrap/cache/
- .env

Next, edit the `.env` file and change the database credentials.

**Note:** Make sure your inside the `pw-web` directory when you run the commands.

Run the following command to install all the required packages:
````
composer install
````

**Note:** If you have **ANY** of the following columns in the `users` table, **REMOVE** them!
- money
- role
- language
- remember_token
- created_at
- updated_at

**Note:** If you have **ANY** of the following tables in your database, **REMOVE** them!
- migrations
- password_resets
- pweb_apps
- pweb_articles
- pweb_payments
- pweb_ranking_factions
- pweb_ranking_players
- pweb_ranking_territories
- pweb_services
- pweb_settings
- pweb_shop_items
- pweb_transfer
- pweb_vote_logs
- pweb_voucher_logs
- pweb_vouchers

The next step is to create all the database tables and default records, run the following command:
````
php artisan migrate --seed
````

Finally, run this last command to generate an application key:
````
php artisan key:generate
````
Don't forget
```
systemctl restart httpd
```

**Note:** If you receive a 500 error after installation, redo the permissions again.

If you receive any other errors please create an [issue](https://github.com/FuryPW/pw-web/issues).

**Note:** This project is no longer supported, however I will still look at pull requests if you'd like to attempt a fix.

