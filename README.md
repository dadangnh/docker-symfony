Dockerized Symfony 4 and PHP Development Stack
==============

This stack is a complete tools to develop a web based application. It support [Symfony Framework](https://symfony.com) by default. And you can use it for other PHP Projects too.

## This stack contains

* [Nginx](http://nginx.org/)
  | A powerful web server, latest nginx will be installed by default
* [MariaDB](http://www.mariadb.org/)
  | One of the most popular database engine, by default it will deploy MariaDB 11
* [PostgreSQL](http://www.postgresql.org/)
  | Powerfull database engine (version 11.2)
* [PHP-FPM](http://php-fpm.org/)
  | Complete PHP7.2 stack (include xdebug, apc, memcached, redis)
* [Memcached](http://memcached.org/)
  | Memory based cache (latest)
* [Redis](http://redis.io/)
  | Powerful backend cache (latest)
* [Elasticsearch](http://www.elasticsearch.org/)
  | Search engine library
* [Logstash](https://www.elastic.co/products/logstash)
  | Data collection engine
* [Kibana](https://www.elastic.co/products/kibana)
  | Data visualization (used for log data with elasticsearch and logstash)
* [RabbitMQ](https://www.rabbitmq.com/)
  | Message broker software
* [Adminer](https://www.adminer.org/)
  | Simply yet powerful database administration tool (latest)

## Requirements

* [Docker Engine](https://docs.docker.com/installation/)
* [Docker Compose](https://docs.docker.com/compose/)
* [Docker Machine](https://docs.docker.com/machine/) (Mac and Windows only)

# Installation
## Fresh Install Symfony
First, clone this repository:

```bash
$ git clone git@gitlab.com:dadangnh/docker-symfony.git
```

Then, you must put your symfony project to apps/symfony directory. Or you can create new project by running:

```bash
$ composer create-project symfony/website-skeleton apps/symfony
```
And create your docker environment by editing `.env.dist` and save as `.env`

Then, run:

```bash
$ docker-compose up
```
## Your current project (not fresh install)
Copy or pull your project to apps/symfony directory:
```bash
$ git clone git@yourgit:youruser:yourproject.git apps/symfony
```
or
```bash
$ cp -Rf /path/to/yourproject apps/symfony
```
Then, run:

```bash
$ docker-compose up
```
Note if you install symfony by pulling from git or copy the script to apps directory manually,
sometimes you need to  install the composer dependency and cache warmup:
```bash
$ docker exec -it docker-symfony_php_1 composer install
$ docker exec -it docker-symfony_php_1 php bin/console cache:warmup
```
Configure the database and environment:
* DB Backend: use `mariadb` or `postgres` as the hostname
* Cache Backend: use `memcached` or `redis` as the hostname
You need to specify the environmenr variable at `.env`

and create migrations:
```bash
$ docker exec -it docker-symfony_php_1 php bin/console make:migrations
$ docker exec -it docker-symfony_php_1 php bin/console doctrine:migrations:migrate
$ docker exec -it docker-symfony_php_1 php bin/console doctrine:fixtures:load

```
_____
And finally, edit your hosts file to match the docker:
```bash
$ sudo echo "127.0.0.1 project.ddg" > /etc/hosts
```

You are done, Open your browser and here is the default link:

* Your Symfony Application `http://project.ddg`
* Kibana `http://project.ddg:81`
* Adminer `http://project.ddg:8080`
* RabbitMQ `http://project.ddg:15672`

_Note :_ you can rebuild all Docker images by running:

```bash
$ docker-compose build
```

If you got error permissions denied on log directory (`Unable to write in the logs directory (/var/www/symfony/var/log)`), you can fix by run this:
```bash
$ docker exec -it docker-symfony_php_1 chmod 777 /var/www/symfony/var/log
```
you need to change the docker-symfony_php_1 to be your php docker name, run this to check:
```bash
$ docker ps
```
normally it will look like this:
```bash
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                                                                                        NAMES
ea9814d825a6        docker-symfony_nginx   "nginx"                  6 minutes ago       Up 6 minutes        0.0.0.0:80->80/tcp, 443/tcp                                                                  docker-symfony_nginx_1
70ce4891c24a        docker-symfony_php     "php-fpm7 -F"            7 minutes ago       Up 6 minutes        9002/tcp, 0.0.0.0:9000->9001/tcp                                                             docker-symfony_php_1
816613d78598        postgres                "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:5432->5432/tcp                                                                       docker-symfony_postgres_1
e72b8b12e508        willdurand/elk          "/usr/bin/supervisor…"   7 minutes ago       Up 7 minutes        0.0.0.0:81->80/tcp                                                                           docker-symfony_elk_1
316bb78d286b        redis                   "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:6379->6379/tcp                                                                       docker-symfony_redis_1
949a8679d730        memcached               "memcached '-m 64'"      7 minutes ago       Up 7 minutes        11211/tcp                                                                                    docker-symfony_memcached_1
2a1a50381e15        mariadb                 "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:3306->3306/tcp                                                                       docker-symfony_db_1
092590c64b52        adminer                 "entrypoint.sh docke…"   7 minutes ago       Up 7 minutes        0.0.0.0:8080->8080/tcp                                                                       docker-symfony_adminer_1
d2ab4969549b        rabbitmq:management     "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, 15671/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp   docker-symfony_rabbitmq_1
```
# Read logs

You can access Nginx and Symfony application logs in the following directories on your host machine:

* `logs/nginx`
* `logs/symfony`

or by using Kibana at `http://project.ddg:81`

# Xdebug for development

To use xdebug change the line `"docker-host.localhost:127.0.0.1"` in docker-compose.yml and replace 127.0.0.1 with your machine ip addres.
If your IDE default port is not set to 5902 you should do that, too.

# License

This code is published under [MIT License](LICENSE.md).