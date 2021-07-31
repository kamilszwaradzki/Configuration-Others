# Laravel PHP Framework with XAMPP/LAMPP
## Step 1: Install LAMPP on Ubuntu
To install the LAMPP inside your Ubuntu machine, 
first download LAMPP from this [site](https://www.apachefriends.org/download.html), once you have downloaded it, run the following command in terminal:

`sudo ./xampp-linux-x64-{your-version-xampp}-installer.run`<br>

## Step 2: Create Laravel project
Composer is the dependency manager of the PHP framework. If you get the Composer installer inside your PHP framework, you don’t need to download and install related packages individually over and over. It can update, reload, and configure PHP frameworks automatically. To install the Composer for a PHP framework, you can follow the cURL command given below.

`curl -sS https://getcomposer.org/installer | php`

Now move the Composer files into the bin directory using the mv command in your Ubuntu machine. This will help your PHP framework to get interacted with the Composer settings. Then move the Nginx directories into the read-only /usr/share/ directory. Remind that you must not run the Composer as a root user.

`sudo mv composer.phar /usr/local/bin/composer`<br>
`cd /opt/lampp/`

Now we are going to generate a new Laravel project and deploy it to check whether the Composer can load the Laravel PHP framework or not.

`sudo composer create-project laravel/laravel {your-name-project}`

## Step 3: Create Virtual Host in Xampp or Lampp on Linux
We creates virtual host for is a method for hosting multiple domain names on a single server (or multiple servers).In this process one server to share its resources like memory and processor cycles.

We will following step to create virtual host.

### Step 1
Go to /opt/lampp/etc/httpd.conf And open httpd.conf.

Now search "#Include conf/extra/httpd-vhosts.conf" and remove "#" sign in front of "Include conf/extra/httpd-vhosts.conf".

### Step 2
Open httpd-vhosts.conf file located in "/opt/lampp/etc/extra/httpd-vhosts.conf" and add following code.

<pre>
&lt;VirtualHost *:80&gt;
DocumentRoot /opt/lampp/htdocs
ServerName localhost
ServerAlias www.localhost
&lt;/VirtualHost&gt;
#New Virtual host
&lt;VirtualHost *:80&gt;
    ServerName localhost2
    DocumentRoot <font color="#AD7FA8">&quot;/opt/lampp/{your-project}/public&quot;</font>
    &lt;Directory <font color="#AD7FA8">&quot;/opt/lampp/{your-project}/public&quot;</font>&gt;
        Options +Indexes +Includes +FollowSymLinks +MultiViews
        AllowOverride All
        Require local
    &lt;/Directory&gt;
    ErrorLog <font color="#AD7FA8">&quot;logs/{your-name-project}-error.log&quot;</font>
    CustomLog <font color="#AD7FA8">&quot;logs/{your-name-project}-access.log&quot;</font> common                              
&lt;/VirtualHost&gt;
</pre>
### Step 3
Go to "/etc/hosts" and open the file "hosts" and add following line.

`127.0.0.1 localhost2`<br/>

### Step 4
Restart the Apache service

Now we have two virtual host in our computer. we can access it through following url.
<pre>
http://localhost      //has document root /opt/lampp/htdocs
http://localhost2     //has document root /opt/lampp/{your-project}/public 
</pre>
## Step 4: Configure permissions to the Laravel project
When Laravel creates files on extension installations or any other operation, it uses the default mask 755 for folders and 644 for files. These permissions are correct and secure for production servers but it will give us problems on our local server because our editor will not be able to edit those files and folders. To fix these problems, simply run again this command:

`cd /opt/lampp/{your-project}`<br/>
`sudo chown -R $USER:www-data storage`<br/>
`sudo chown -R $USER:www-data bootstrap/cache`<br/>
`chmod -R 775 storage`<br/>
`chmod -R 775 bootstrap/cache`<br/>

An other option is to configure the default Apache owner and group. For all new files created by the server these settings are used. We can change the in the environment variables file. Run this command to open and edit the file:

`sudo gedit /etc/apache2/envvars`<br/>

Find the following lines and change them this way (replace "youruser" with your current username):
<pre>
export APACHE_RUN_USER=youruser
export APACHE_RUN_GROUP=www-data
</pre>
Restart apache with the following command:

`sudo ./../lampp restart`
