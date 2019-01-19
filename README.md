# Pixelfed Installation Instructions with script

Alpha instructions for installation of Pixelfed with Scripts

```sh
$ sudo apt install git
$ git clone https://github.com/capmisson/pixelfedinstall
$ cd pixelfedinstall
$ bash script1.sh
```

Edit /var/www/vhosts/pixelfed/httpdocs/.env and change:
```sh
APP_URL=http://localhost -> change localhost to your domain name
ADMIN_DOMAIN="localhost" -> change localhost to your domain name
APP_DOMAIN="localhost" -> change localhost to your domain name
```

Then continue with Script2
```sh
$ bash script2.sh
$ screen
$ sudo -u www-data php artisan horizon
```

Leave screen on background (ctrl + a + d)
```sh
$ sudo nano /etc/nginx/sites-available/example.com
```

Edit the file as https://github.com/capmisson/pixelfedinstall/blob/master/example.conf

```sh
$ sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
$ sudo service nginx reload
$ sudo nano /etc/apt/sources.list
```

add:
```sh
deb http://deb.debian.org/debian stretch-backports main contrib non-free
deb-src http://deb.debian.org/debian stretch-backports main contrib non-free
```

Then:
```sh
$ sudo apt install python-certbot-nginx -t stretch-backports
$ sudo certbot --nginx -d example.com -d www.example.com
```
Select the option of redirecting all transit thru https

```sh
$ sudo nano /var/www/vhosts/pixelfed/httpdocs/.env
```

edit app_url to https
edit mail smtp info

```sh
$ cd /var/www/vhosts/pixelfed/httpdocs/
$ php artisan config:cache
```
Create admin:
```sh
$ php artisan tinker
$ $username = ‘yourusername’;
$ $user = User::whereUsername($username)->first();
$ $user->is_admin = true;
$ $user->save();
$ exit
$ php artisan config:cache
$ sudo chown www-data:www-data /var/www/vhosts/pixelfed/ -R
```
