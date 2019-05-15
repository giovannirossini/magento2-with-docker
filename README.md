# Magento 2.3 with Docker Compose

#### Dockerizing Magento 2.x with Docker and Docker-Compose

Magento is an e-commerce platform written in PHP and based on Zend framework available under both open-source and commercial licenses.

For this project:

* Ubuntu 16.04
* Apache2
* Mysql-5.7
* PHP-7.1

Now follow the following steps:

1). Clone or download this repository as 

`git clone https://github.com/giovannirossini/magento2-with-docker.git`

1) Set mysql credentials and name of the database to be created. Go to ~/magento2-with-docker/docker-compose.yml and change mysql password, if you wish.
2) I set user, password and database name as default:

```sh
MYSQL_USER:     "admin"
MYSQL_PASSWORD: "admin123"
MYSQL_DATABASE: "magento2"
```

3). Download Magento 2.x version you wish to dockerize and upload it in directory **magento2** inside *web_server* directory:

> Go to https://magento.com/tech-resources/download.

4). Build the docker image:

`docker-compose build`

5). Run the containers from built image as:

`docker-compose up -d`

6). Now you can access on http://localhost/setup to install Magento 2.x from web.

## Automating Setup From The Command Line

To setup magento 2 via follow the steps:

1). Permission on magento file inside docker:

`docker exec -it magento bash`

Then:

`find ./bin -name 'magento' -exec chmod u+x {} \;`

2). Run the setup command:

```sh
php bin/magento setup:install \
--base-url='http://localhost/' \
--db-host='mysql' \
--db-name='magento2' \
--db-user='admin' \
--db-password='admin123' \
--backend-frontname='admin' \
--admin-firstname='admin' \
--admin-lastname='admin' \
--admin-email='admin@admin.com' \
--admin-user='admin' \
--admin-password='admin123' \
--language='pt_BR' \
--currency='BRL' \
--timezone='America/Sao_Paulo' \
--use-rewrites='1'
```

3). Permissions on magento files and paths:

For files:

`find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +`

For paths:

`find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +`

4). Now we have to compile:

`bin/magento setup:di:compile`

And start the cron jobs:

```sh
bin/magento cron:install
bin/magento cron:run
```