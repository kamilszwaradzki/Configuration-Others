# Laravel PHP Framework with Nginx
## Step 1: Install Nginx Server on Ubuntu
To install the Nginx server inside your Ubuntu machine, 
you can use the following apt command-line in your terminal shell.

`sudo apt update && sudo apt upgrade`<br>
`sudo apt install nginx -y`

Once the Nginx server is installed, you can verify the Nginx server by checking it’s version and configuration settings.

`nginx -v`<br>
`sudo nginx -t`

Now run the following terminal command lines to enable and start the Nginx server on your Ubuntu machine.

`systemctl start nginx`<br>
`systemctl enable nginx`

Be careful if you have any previous PHP framework or Apache server installed inside your Ubuntu machine; make sure you have cleaned the previous packages correctly. You can check the open the port of Nginx. Normally the open port of Nginx is 80. To check Nginx’s open port, I am using the curl command tool on Ubuntu. If you don’t have the cURL installed inside your machine, install the cURL tool first.

`sudo apt install curl`<br>
`curl -I localhost`

## Step 2: Install Mysql Database for PHP Framework
Then start and enable the MySQL server by using the terminal commands given below.

`sudo apt-get install mysql-server -y`<br>
`systemctl start mysql`<br>
`systemctl enable mysql`

## Step 3: Install PHP Framework on Ubuntu Linux

Here, we are going to install the PHP framework on our Ubuntu machine. We will install the PHP fast process manager, document object model (DOM), PHP MySQL extension, and multi-byte string functions alongside the PHP framework. All the extension packages are required to run the Laravel PHP framework smoothly on your machine. You can copy and paste the following command line in your terminal shell to install the PHP framework and extension packages.

`sudo apt install php7.3 php7.3-fpm php7.3-mysql php7.3-mbstring php7.3-dom -y`

Now we can install the Mcrypt PHP extension to make the PHP framework more encrypted and secure. Use the following command lines to install the Mycrypt PHP extension on your Ubuntu device.

`sudo apt install php7.3-dev libmcrypt-dev gcc make autoconf libc-dev pkg-config vim -y`<br>
`sudo pecl install mcrypt-1.0.1`

In this step, we have to edit the configuration script of the PHP-FPM (Fast Process Manager) to add the new address and remove the default address. Using the Gedit text editor to edit the configuration script, you can use any script editor you want. Use the following command line to edit the configuration script.

`sudo vim /etc/php/7.3/fpm/pool.d/www.conf`

Now find the following line inside the `www.conf` script. Make the line as a comment line by adding a `;` before the line.

`listen = /run/php/php7.3-fpm.sock`

Now add a new line into the script to add a new server address. Here, I am using the localhost address and port 8000.

`listen = 127.0.0.1:8000;`

Now save the script file and restart and enable the PHP framework on your Ubuntu machine. Then restart the Nginx server.

`systemctl restart php7.3-fpm`<br>
`systemctl enable php7.3-fpm`<br>
`systemctl restart nginx`

## Step 4: Install Laravel PHP Framework on Ubuntu

Composer is the dependency manager of the PHP framework. If you get the Composer installer inside your PHP framework, you don’t need to download and install related packages individually over and over. It can update, reload, and configure PHP frameworks automatically. To install the Composer for a PHP framework, you can follow the cURL command given below.

`curl -sS https://getcomposer.org/installer | php`

Now move the Composer files into the bin directory using the mv command in your Ubuntu machine. This will help your PHP framework to get interacted with the Composer settings. Then move the Nginx directories into the read-only /usr/share/ directory. Remind that you must not run the Composer as a root user.

`sudo mv composer.phar /usr/local/bin/composer`<br>
`cd /usr/share/nginx`

Now we are going to generate a new Laravel project and deploy it to check whether the Composer can load the Laravel PHP framework or not.

`composer create-project laravel/laravel test dev-master --prefer-dist`

If you need, you can grant access permission to the Nginx server by using the chown command in the terminal shell.

`chown -R www-data:www-data test/`

You can also override and replace the available site information in the Nginx web server to ensure that your localhost address is placed perfectly.

`mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.old`

Check whether the Composer tool is installed correctly or not. Use the following command to check the version of the Composer tool.

`composer`

Now go to the test directory to access the storage permission. Remember, you must have to grand the storage access to the database server to load your server. Use the following commands-lines in your Ubuntu terminal shell to grand the storage access to the Nginx server.

`cd test/`<br>
`chmod -R 0777 storage/`<br>
`mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.old`

Here, we have to edit the Nginx server script according to the PHP framework that we have set previously to make an environment for the Laravel PHP framework. Use the command-line given below to edit the default settings of the Nginx server.

`gedit /etc/nginx/sites-available/default`

Now copy and paste the following script given below into your Nginx server script. Then save and exit the script.
```
server {
    listen 80 default_server 
    listen [::]:80 default_server ipv6only=on 
    root /usr/share/nginx/test/public 
    index index.php index.html index.htm 
    server_name 127.0.0.1 
    location / {
        try_files $uri $uri/ /index.php?$query_string 
    }
    location ~ .php$ {
        try_files $uri /index.php =404 
        fastcgi_split_path_info ^(.+.php)(/.+)$ 
    fastcgi_pass 127.0.0.1:8000 
        fastcgi_index index.php 
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name 
        include fastcgi_params 
    }
}
```

Now, restart the Nginx server to refresh the Laravel PHP framework on your Ubuntu machine. If you have done everything right, you will be able to load the Laravel PHP framework on your web browser by entering the IP address and port you set.

`systemctl restart nginx`

## Remove Laravel PHP Framework from Ubuntu Linux

There are many reasons that you might want to remove the Laravel PHP framework form your Ubuntu machine. You might want a fresh installation of Laravel, or you might want to switch back to your previous PHP framework. Whatever the reason is, here is the command-line that you can use to remove the Laravel PHP framework from your Linux machine.

`composer global remove laravel/installer`
