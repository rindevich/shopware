# Docker Containers for fast Shopware

### Instruction for usage:

    1. Clone the repository to your local machine
    `$ ssh git@github.com:rindevich/shopware.git shopware && cd shopware`
    2. Use composer for create your new Shopware Project:
    "$ composer create-project shopware/composer-project shopware --no-interaction --stability=dev"
    
    About project : https://github.com/shopware/composer-project

    3. Build containers and run 
    `$ docker-compose up -d`
    4. Login to container 
    `$ docker exec -it sw_server_1 bash`
    5. First Run the Shopware:
     Update missing packages 
     `$ composer install`
     Installation (Credantials you can find in .env file in root folder)
     `$ ./app/install.sh` 

    6. Add Redis configuration, add this to app/congif/config.php
        	`'session' => array(
    		'save_handler' => 'redis',
    		'save_path' => "tcp://127.0.0.1:6379",
		),

		'backendsession' => array(
    		'save_handler' => 'redis',
    		'save_path' => "tcp://127.0.0.1:6379",
		),`
		
    7. Login to backend and clear all caches

### Extra feature:
    Install Shopware profiler https://github.com/FriendsOfShopware/FroshProfiler
