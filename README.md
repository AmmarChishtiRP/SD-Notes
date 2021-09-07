# SD-Notes

## Common Errors

### General solutions to every problem
- Restart container
- Flush cache
- `rm -rf var/*`
- `rm -rf generated/*`

### Memory limit reached when running composer commands in container
Fatal error: Allowed memory size of 2147483648 bytes exhausted (tried to allocate 4096 bytes) in phar:///usr/local/bin/composer/src/Composer/DependencyResolver/Solver.php on line 223 
- Run `php -d memory_limit=-1 <command>`
- To run composer update we use the command `php -d memory_limit=-1 /usr/local/bin/composer update`

### Frontend of environment is messed up
- Run `./compose/bin/bash`
- `cd tools` and run `yarn`, then `cd ..` and run `yarn build`

## XDebug Commands

### Check Xdebug version
- `./compose/bin/root bash -c "php -v"`

### Check if Xdebug is enabled
- `./compose/bin/root bash -c "php -i | grep xdebug"`
- Xdebug is enabled if the above command produces A LOT of output in the format "xdebug.*"

### Install/Reinstall Xdebug 2.9.8 (Current Version)
- `/compose/bin/root bash -c "pecl install -f xdebug-2.9.8" && ./compose/bin/restart && ./compose/bin/xdebug disable && ./compose/bin/xdebug enable && ./compose/bin/root bash -c "php -v"`

## Magento Upgrades

### Easy way to check difference between 

## Environment specific tips

### Clicking on a product in accelerator will return a BrainTree Error
- Go to Admin Panel -> Stores -> Configuration -> Sales -> Payment Methods -> Click on "Configure" button besides "Braintree Payments" -> Click on "Save Config" in the top right

### Hardwaresales home directory path
- /var/www/vhosts/www.hardwaresales.com/current


## Misc.

### Upgrade a module to a specific version
- `php -d memory_limit=-1 /usr/local/bin/composer require <module-name>:<module-version>`
- For example, run `php -d memory_limit=-1 /usr/local/bin/composer require magento/magento-cloud-metapackage:2-3-7-p1` to upgrade "magento/magento-cloud-metapackage" to version 2.3.7-p1

### Show all module versions
- `composer show --all <module-name>`
- For example, to check all versions for the "magento-cloud-metapackage" package, run `composer show --all magento/magento-cloud-metapackage`

## To find admin panel URL
- ssh into environment you want to find URL for
- run `grep frontName app/etc/env.php` and go to URL `<env-URL>/<output of grep cmd>`
- For example, `grep frontName app/etc/env.php` returns "'frontName' => 'adminvdI42K'," for Juliska staging, therefore the valid URL is mcstaging.juliska.com/adminvdI42K

### Print line
- `file_put_contents(BP . '/var/log/{logname}.log', " Debug Content\n", FILE_APPEND);`




=================================================

## How Controllers work in Magento 2


## config.xml format in Magento 2




# Get the current output of composer install
git clone git@github.com:sdinteractive/HardwareSales-HardwareSales.git
php -d memory_limit=-1 /usr/local/bin/composer install --ignore-platform-reqs
# Clean vendor
rm -rf vendor
# Upgrade
(Manually update reference in composer.json from vertex to vertex inc)
php -d memory_limit=-1 /usr/local/bin/composer require magento/product-community-edition=2.3.7-p1 --no-update
php -d memory_limit=-1 /usr/local/bin/composer update --ignore-platform-reqs

If we are doing a database backup before a deployment we DO NOT STRIP ANYTHING