# wordpres4.8.3

sudo su
apt-get update
apt-get install nginx
apt-get install php-fpm php-mysql unzip

sudo su
apt-get update
apt-get install nginx
apt-get install php-fpm php-mysql php-zip php-curl unzip git

nano /etc/php/7.0/fpm/php.ini
#Uncomment and change default value 1 for 0 on cgi.fix_pathinfo=0, search with ctrl+w on nano editor.

cgi.fix_pathinfo=0;

#change the default upload_max_filesize = 2M, search with ctrl+w on nano editor. 
upload_max_filesize = 64M 

#change the default post_max_filesize = 8M, search with ctrl+w on nano editor. 

post_max_filesize = 64M

nano /etc/nginx/sites-available/default

#add index.php to processing

index index.php index.html index.htm index.nginx-debian.html;

#add php location

location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
}

location ~ /\.ht {
    deny all;
}

chown -Rf www-data:www-data /var/www
systemctl restart php7.0-fpm
systemctl reload nginx

cd /var/www/html
wget https://wordpress.org/latest.zip
unzip latest.zip
mv wordpress/* /var/www/html

nano /etc/nginx/sites-available/default

location / {  
#try_files $uri $uri/ =404;  
try_files $uri $uri/ /index.php$is_args$args;  
}

nano /etc/nginx/nginx.conf
#add to http section
client_max_body_size 64M;


chown -Rf www-data:www-data /var/www/html
systemctl restart php7.0-fpm
systemctl reload nginx
