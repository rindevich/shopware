# Docker Containers for fast Shopware
Apache 2.4 | PHP 7.0 | Elastic Search | Redis | MariaDB 10.1 
### Instruction for usage

1. Clone the repository to your local machine
```
$ ssh git@github.com:rindevich/shopware.git shopware && cd shopware
```
2. Use composer for create your new Shopware Project:
```
$ composer create-project shopware/composer-project shopware --no-interaction --stability=dev
```
[Composer Shopware Project](https://github.com/shopware/composer-project)

3. Build containers and run on background
```
$ docker-compose up -d
```
4. Login to container, and install Shopware.
> Credantials you can find in .env root folder
```
$ docker exec -it sw-server bash
$ composer install
$ ./app/install.sh
```
5. Add Redis configuration to app/cocongif/config.php
```
'session' => array(
    'save_handler' => 'redis',
    'save_path' => "tcp://redis:6379",
),
'backendsession' => array(
    'save_handler' => 'redis',
    'save_path' => "tcp://redis:6379",
),
```
6. Add to your local hosts `127.0.0.1 shopware.local`
```
$ sudo nano /etc/hosts
```
7. [Login to backend](http://shopware.local/backend) and clear all caches

### Extra feature:
[Install Shopware profiler](https://github.com/FriendsOfShopware/FroshProfiler)

#### Maintained by: [ETECTURE GbmH](https://www.etecture.de)