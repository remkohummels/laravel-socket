# Laravel + Socket.io Example
***

## Prerequisites for LAMP(Linux/Apache/MySQL/PHP) stack

1). Install PHP 7.2 (reference: [installation in ubuntu](https://thishosting.rocks/install-php-on-ubuntu/))

2). Install MySQL v5.7 (reference: [installation in ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04))

3). Install Apache2 (reference: [installation in ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04))

4). Install Composer (reference: [installation in ubuntu](https://websiteforstudents.com/how-to-install-php-composer-on-ububuntu-16-04-17-10-18-04/))

```composer
$ curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

5). Install Node.js v10.x & Npm v6.5.0 (reference: [installation in ubuntu](https://askubuntu.com/questions/594656/how-to-install-the-latest-versions-of-nodejs-and-npm-for-ubuntu-14-04-lts))

```node
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get update
$ sudo apt-get install -y nodejs
```

6). If the installed npm version is lower than v6.5.0, upgrade it to the latest one.

```npm
$ sudo npm install npm -g
```

7). Install php 7.2 extensions 

```php
$ sudo apt install php7.2 libapache2-mod-php7.2 php7.2-curl php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-zip
```

8). Install the Redis

```redis
$ sudo apt-get install redis-server
$ sudo systemctl enable redis-server.service
```

- Open the redis config file: `$ sudo nano /etc/redis/redis.conf`

```redisConfig
maxmemory 256mb
maxmemory-policy allkeys-lru
```

- Restart the redis server and install the redis php extension

```redis-php
$ sudo systemctl restart redis-server.service
$ sudo apt-get install php7.2-redis
```

## Application Setup in the LAMP stack Environment

In the case of LAMP usage...

Clone git@bitbucket.org:xxx/laravel-socket.git repository to "laravel-socket" folder.

In the /laravel-socket folder,

1). Create a .env file copying from .env.example.

2). Run `$ composer install` to install the necessary dependencies.

3). Run `$ npm install` to install the node modules & libraries by NPM. 

4). Run `$ npm run dev` to publish the assets(fonts/images/js/css/scss) by Laravel Mix + Webpack.

5). Run `$ php artisan key:generate` to generate a new key.

6). Run `$ php artisan config:cache` to reflect the .env configuration.

7). Run `$ php artisan migrate --seed` to migrate and seed the data in DB.

8). Install the "laravel-echo-server" and initialize its configuration.

```echo
$ sudo npm install -g laravel-echo-server
$ laravel-echo-server init
$ laravel-echo-server client:add

```

9). Follow up the below supervisor configuration.

10). To start the scheduler itself, we only need to add one cron job on the server (using the `crontab -e` command), which executes `php /path/to/artisan schedule:run` every minute in the day. 
To discard the cron output we put `/dev/null 2>&1` at the end of the cronjob expression. `* * * * * php /path/to/artisan schedule:run 1>> /dev/null 2>&1`

## Supervisor Configuration

1). Run `$ sudo apt-get install supervisor` to install supervisor.

2). Open `$ sudo nano /etc/supervisor/conf.d/laravel-horizon.conf` to create a config file in /etc/supervisor/conf.d directory.

3). Copy the followings in the `laravel-horizon.conf` file and save it.

```supervisor
[program:horizon]
process_name=%(program_name)s
command=php /var/www/html/laravel-socket/artisan horizon
autostart=true
autorestart=true
user=dev
redirect_stderr=true
stdout_logfile=/var/www/html/laravel-socket/storage/logs/horizon.log
```

4). Copy the followings in the `laravel-echo.conf` file and save it.

```supervisor
[program:laravel-echo]
directory=/var/www/html/laravel-socket
process_name=%(program_name)s_%(process_num)02d
command=laravel-echo-server start
autostart=true
autorestart=true
user=dev
numprocs=1
redirect_stderr=true
stdout_logfile=/var/www/html/laravel-socket/storage/logs/echo.log
```

5). Copy the followings in the `laravel-worker.conf` file and save it.

```supervisor
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/html/laravel-socket/artisan queue:listen --sleep=3 --tries=3
autostart=true
autorestart=true
user=dev
numprocs=8
redirect_stderr=true
stdout_logfile=/var/www/html/laravel-socket/storage/logs/worker.log

```

6). Update the Supervisor configuration and start the processes using the following commands.

`$ sudo supervisorctl reread`

`$ sudo supervisorctl update`

`$ sudo supervisorctl start laravel-worker:*`


## Reference

- [How to Install Redis on Ubuntu 18.04 & 16.04 LTS](https://tecadmin.net/install-redis-ubuntu/)

- [How to use Laravel with Socket.IO](https://medium.freecodecamp.org/how-to-use-laravel-with-socket-io-e7c7565cc19d)

- [Laravel Horizon](https://laravel.com/docs/5.7/horizon)

- [Laravel Echo Server](https://github.com/tlaverdure/laravel-echo-server)

- [Laravel Echo Server — How To](https://medium.com/@dennissmink/laravel-echo-server-how-to-24d5778ece8b)

- [Real-Time notification using laravel as back-end API and AngularJS as front-end](https://stackoverflow.com/questions/50035393/real-time-notification-using-laravel-as-back-end-api-and-angularjs-as-front-end)

## TroubleShooting

- [Laravel Mix “sh: 1: cross-env: not found error”](https://stackoverflow.com/questions/43685777/laravel-mix-sh-1-cross-env-not-found-error)

