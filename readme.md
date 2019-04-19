# Laravel + Socket.io Example
***

## Prerequisites

- Laravel 5.7

- PHP 7.2

- MySQL 5.7

- Install the Redis

- `$ sudo apt-get install redis-server`

- `$ sudo systemctl enable redis-server.service`

- `$ sudo systemctl enable redis-server.service`

- Open the redis config file: `$ sudo nano /etc/redis/redis.conf`

```redisConfig
maxmemory 256mb
maxmemory-policy allkeys-lru
```

- `$ sudo systemctl restart redis-server.service`

- `$ sudo apt-get install php7.2-redis`

## Installation

- `$ composer require laravel/horizon`

- `$ npm install -g laravel-echo-server`

- `$ laravel-echo-server init`

- `$ laravel-echo-server client:add`


## Reference

- [How to Install Redis on Ubuntu 18.04 & 16.04 LTS](https://tecadmin.net/install-redis-ubuntu/)

- [How to use Laravel with Socket.IO](https://medium.freecodecamp.org/how-to-use-laravel-with-socket-io-e7c7565cc19d)

- [Laravel Horizon](https://laravel.com/docs/5.7/horizon)

- [Laravel Echo Server](https://github.com/tlaverdure/laravel-echo-server)

- [Real-Time notification using laravel as back-end API and AngularJS as front-end](https://stackoverflow.com/questions/50035393/real-time-notification-using-laravel-as-back-end-api-and-angularjs-as-front-end)

