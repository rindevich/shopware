# Docker Containers for fast Shopware
### Instruction for usage

1. Clone the repository to your local machine
```
$ ssh git@github.com:rindevich/shopware.git shopware && cd shopware
```
2. Use composer for create your new Shopware Project:
```bash
composer create-project shopware/composer-project shopware --no-interaction --stability=dev
```
[Composer Shopware Project](https://github.com/shopware/composer-project)

3. Build containers and run on background
```
$ docker-compose up -d
```
4. Login to container, and install Shopware
```
$ docker exec -it sw_server_1 bash
$ composer install
$ ./app/install.sh
```
(Credantials you can find in .env in root folder)
5. Add Redis configuration, add this to app/congif/config.php
```
'session' => array(
    'save_handler' => 'redis',
    'save_path' => "tcp://127.0.0.1:6379",
),
'backendsession' => array(
    'save_handler' => 'redis',
    'save_path' => "tcp://127.0.0.1:6379",
),
```
6. Login to backend and clear all caches

### Extra feature:
[Install Shopware profiler](https://github.com/FriendsOfShopware/FroshProfiler)
