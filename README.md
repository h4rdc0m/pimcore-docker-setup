# pimcore-docker-setup
Simple Docker setup for pimcore local development

## Run Docker
* Go to root directory
* run `docker-compose build`
* run `docker-compose up -d`

## Install Pimcore within container
After starting the container some additional manual steps are needed.

Go into the PHP container with
* run `docker-compose exec php bash`

#### Install pimcore (see https://pimcore.com/de/download)
* run `COMPOSER_MEMORY_LIMIT=-1 composer create-project pimcore/demo-basic-twig pimcore`

The pimcore project structure will be created in the directory pimcore

Change owner for calling next commands.

* run `chown -R 1000:1000 pimcore`

The next command creates pimcore with an initial admin user and our defined database properties.

* run `cd pimcore && COMPOSER_MEMORY_LIMIT=-1 ./vendor/bin/pimcore-install  
--admin-username=admin --admin-password=admin --mysql-host-socket=mysql 
--mysql-username=pimcore --mysql-password=pimcore --mysql-database=pimcore --no-interaction`


* run `chown -R 1000:1000 var`
* run `bin/console assets:install`


## Access Pimcore 
You can now access pimcore frontend at http://localhost:8080 and admin at http://localhost:8080/admin

## Troubleshooting

If you're getting the following error with Pimcore 6

```* Warning: Declaration of Pimcore\Bundle\CoreBundle\EventListener\LegacyTemplateListener::onKernelView(Symfony\Component\HttpKernel\Event\GetResponseForControllerResultEvent $event) should be compatible with Sensio\Bundle\FrameworkExtraBundle\EventListener\TemplateListener::onKernelView(Symfony\Component\HttpKernel\Event\KernelEvent $event)```

You can fix it with changing the `pimcore/composer.json` file and require and older version of `sensio/framework-extra-bundle`

The require part should look like this:

```json
"require": {
    "php": ">=7.2",
    "wikimedia/composer-merge-plugin": "^1.4",
    "pimcore/pimcore": "~6.0.0",
    "sensio/framework-extra-bundle": "5.3.1"
  },

```

Then run `composer update` and the `pimcore-install` script again. 
