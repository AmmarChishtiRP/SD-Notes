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
- `./compose/bin/root bash -c "pecl install -f xdebug-2.9.8" && ./compose/bin/restart && ./compose/bin/xdebug disable && ./compose/bin/xdebug enable && ./compose/bin/root bash -c "php -v"`

## Magento Upgrades

### Easy way to check difference between two magento version installs/applied-patches
- Get the current output of composer install
    - git clone git@github.com:sdinteractive/HardwareSales-HardwareSales.git
    - php -d memory_limit=-1 /usr/local/bin/composer install --ignore-platform-reqs
- Clean Vendor
    - rm -rf vendor/*
- Upgrade
    - php -d memory_limit=-1 /usr/local/bin/composer require magento/product-community-edition=2.3.7-p1 --no-update
    - php -d memory_limit=-1 /usr/local/bin/composer update --ignore-platform-reqs

## Environment specific tips

### Clicking on a product in accelerator will return a BrainTree Error
- Go to Admin Panel -> Stores -> Configuration -> Sales -> Payment Methods -> Click on "Configure" button besides "Braintree Payments" -> Click on "Save Config" in the top right

### Hardwaresales home directory path
- /var/www/vhosts/www.hardwaresales.com/current

## Magento Specific

### How controllers work in Magento 2 (TBC)
- When you query a URL, that URL resolves to a Controller route, which based on that controller route hits a controller action and loads an associated block. Based on that block, it specifies a template and possibly child templates. The block can provide resources for that template

### Crontab.xml vs .magento.app.yaml
- Cron jobs defined in .magento.app.yaml run on their own separate threads and are considered more "critical" then cron jobs in crontab.xml

## Misc.

### Upgrade a module to a specific version
- `php -d memory_limit=-1 /usr/local/bin/composer require <module-name>:<module-version>`
- For example, run `php -d memory_limit=-1 /usr/local/bin/composer require magento/magento-cloud-metapackage:2-3-7-p1` to upgrade "magento/magento-cloud-metapackage" to version 2.3.7-p1

### Show all module versions
- `composer show --all <module-name>`
- For example, to check all versions for the "magento-cloud-metapackage" package, run `composer show --all magento/magento-cloud-metapackage`

### To find admin panel URL
- ssh into environment you want to find URL for
- run `grep frontName app/etc/env.php` and go to URL `<env-URL>/<output of grep cmd>`
- For example, `grep frontName app/etc/env.php` returns "'frontName' => 'adminvdI42K',". Therefore the valid URL for Juliska staging is is mcstaging.juliska.com/adminvdI42K

### Email Address used for Google Tag Manager
- devsdmail@gmail.com

### Flush Redis
- `./compose/bin/redis redis-cli flushall`

### Print line
- `file_put_contents(BP . '/var/log/{logname}.log', " Debug Content\n", FILE_APPEND);`

### Before Staging deployment
- When obtaining a database backup before a staging deployment we DO NOT STRIP ANYTHING

### zcat command does not work on osx
- Try `zcat < <compressed-file-name>`
