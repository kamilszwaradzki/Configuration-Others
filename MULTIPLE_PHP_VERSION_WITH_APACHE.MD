# Install Multiple PHP Version with Apache on Ubuntu
## Step 1: Installing Apache
To install the Apache web server and module whose is a FastCGI implementation inside your Ubuntu machine, 
you can use the following apt command-line in your terminal shell. <br>

`sudo apt update`<br>
`sudo apt install apache2 libapache2-mod-fcgid`

## Step 2: Installing PHP
A PPA is maintained for PHP installation on Ubuntu systems from a long years. You just need to add that PPA to your system to install packages.
Use the below couple of commands to add the PPA to your system.<br>

`sudo apt install software-properties-common`<br>
`sudo add-apt-repository ppa:ondrej/php`

For this tutorial, we are using the PHP 7.3 and PHP 7.4 to configure with Apache web server.
To use the multiple PHP versions, we will use PHP FPM and FastCGI. Let’s install the following packages on your system.<br>

`sudo apt update -y`<br>
`sudo apt install php7.3 php7.3-fpm -y`<br>
`sudo apt install php7.4 php7.4-fpm -y`

After installation, php-fpm services will be started automatically. Use the following commands to make sure both services are running.

`sudo systemctl status php7.3-fpm`<br>
`sudo systemctl status php7.4-fpm`

However, if you are using WSL or just don't have systemctl installed then I recommend using the commands:

`sudo service php7.3-fpm status`<br>
`sudo service php7.4-fpm status`

## Step 3: Configure Apache with Multiple PHP
Now enable few modules required for the configuration of multiple PHP versions with Apache.
These modules are necessary to integrate PHP FPM and FastCGI with Apache server.

`sudo a2enmod actions fcgid alias proxy_fcgi`

Get ready for the configuration of websites on your Apache server. For the testing purpose, I am configuring two websites to work with two different-2 PHP versions.
First, create two directories on your server.

`sudo mkdir /var/www/php73-app`<br>
`sudo mkdir /var/www/php74-app`

Now, create and index.php containing the `phpinfo();` function.

`echo "<?php phpinfo(); ?>" > /var/www/php73-app/index.php`<br>
`echo "<?php phpinfo(); ?>" > /var/www/php74-app/index.php`

Let’s start the creation of VirtualHost. Apache keeps all the VirtualHost configuration files under /etc/apache2/sites-available with the extension .conf.
Create a file for the first virtual host and edit in your favorite text editor.

`sudo vim /etc/apache2/sites-available/php73.example.com.conf`

Add the following content. Make sure to use correct ServerName and directory path according to your setup. This website is configured to work with PHP 7.3.

```
<VirtualHost *:80>
    ServerName php73.example.com
    DocumentRoot /var/www/php73-app
    <Directory /var/www/php73-app>
        Options -Indexes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all granted
    </Directory>
    <FilesMatch \.php$>
        # Apache 2.4.10+ can proxy to unix socket
        SetHandler "proxy:unix:/var/run/php/php7.3-fpm.sock|fcgi://localhost"
    </FilesMatch>
</VirtualHost>
```

Similarly, create a second VirtualHost configuration file to work with PHP 7.4. Edit configuration file in text editor:

`sudo vim /etc/apache2/sites-available/php74.example.com.conf`

Add the following content to file with proper ServerName and DocumentRoot.

```
<VirtualHost *:80>
    ServerName php74.example.com
    DocumentRoot /var/www/php74-app
    <Directory /var/www/php74-app>
        Options -Indexes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all granted
    </Directory>
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php7.4-fpm.sock|fcgi://localhost"
    </FilesMatch>
</VirtualHost>
```

You both of the websites are configured now. But they are still not active. Apache keeps active sites under /etc/apache2/sites-enabled directory.
You can simply create a symbolic link of config files to this directory or use below command to do the same.

`sudo a2ensite php73.example.com`<br>
`sudo a2ensite php74.example.com`

After making all the changes restart Apache to reload new settings changes.

`sudo systemctl restart apache2`

Your setup has been completed now. Go to the next step to test your setup.

## Step 4: Test Your Setup
Edit /etc/hosts file on your local system and make an entry like below. This will resolve temporary names to localhost IP address.

`sudo vim /etc/hosts`

or if you use WSL then:

```
press 'Windows' key on your keyboard, write 'notepad.exe' in search bar, then click right mouse button on a 'Notepad'
and select with menu option 'Run as administrator'.
Next when the program starts, then press Ctrl+O, and select path 'C:\Windows\System32\drivers\etc' (if the 'hosts' file is not visible, switch the file type from 'Text documents (*.txt)' to 'All files (*.*)' from the selection menu on the right.)
```

Add following entry to end of file

```
127.0.0.1   php74.example.com
127.0.0.1   php73.example.com
```
After a reboot system (if you are using windows). Open a web browser and visit both of the sites. You will see that php73.example.com shows the version PHP 7.3 and php74.example.com is showing the PHP 7.4 as the configuration.

<hr/>

[Source](https://tecadmin.net/how-to-install-multiple-php-version-with-apache-on-ubuntu-20-04/)
