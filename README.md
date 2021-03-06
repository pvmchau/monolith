GO1 monolith
====

> This project is **not yet useful** for community as we are trying to opensource the GO1 LMS.
> Keep your eyes here to have updates.

## 1. Dependencies

- git
- php7
    - composer
- golang:
    - `composer global require symfony/yaml`
    - glide: `curl https://glide.sh/get | sh`
    - `export PATH="/path/to/monolith/scripts:$PATH"`
- nodejs if you are frontend developers.
    - npm
    - bower
    - grunt
- /etc/hosts
    - `127.0.0.1	localhost staff.local website.local host portal1.go1.local portal2.go1.local`

## 2. Usage

- `php ./scripts/build.php`: Build the code base.
    - `--skip-php`: Don't run composer commands. 
    - `--skip-web`: Don't run npm commands.
    - `--skip-drupal`: Don't build drupal code base.
    - `--skip-go`: Don't build golang code base.
    - `--skip-tools`: Don't build adminer.
    - Backend developer: `php scripts/build.php --skip-go --skip-web --skip-drupal`
    - Frontend developer: `php scripts/build.php --skip-go --skip-drupal`
- `php scripts/start.php`, then try some links:
    - http://localhost/ — #ui
    - http://localhost/v3/ — #api
    - http://localhost/GO1/user/ — #service
    - http://staff.local — #staff, with some notes:
        - You can use staff@local/root to login.
    - http://localhost:7474/ - #neo4j admin
    - http://localhost/GO1/adminer/ — SQL database management.
    - http://localhost:15672/ - rabbitMQ admin
    - http://website.local — #website, to create portal:
        -Click 'Free Trial' -> email 'admin@portal1.go1.local' -> click 'Get Started' to add new portal.
    - http://localhost:9900/minio - #minio (s3) file management.
    - http://portal1.go1.local/ or http://portal1.go1.local:9090 to test issues related to domain.
- `php scripts/install.php` to install database.
- If you are frontend developers:
    - `cd web/ui && grunt serve`
    - Then try:
        - http://localhost:9090/ — #ui live
    - `grunt build`
    - `grunt test`
- If you need to work with interactive li:
    - `php scripts/start-scorm.php`
- Run test cases without Docker:
    - `cd php/user/`
    - `phpunit`
- To rebuild microservices:
    - `docker-compose pull`
    - `php scripts/git/pull.php --confirm`
    - `php scripts/clean.php`
    - `docker images -q --filter "dangling=true" | xargs -r docker rmi` - Remove untagged images
    - `php scripts/build.php --skip-web --skip-drupal --skip-go`
    - `cd php && rm composer.lock && composer install -v && cd ..`
    - `php scripts/start.php`
    - `php scripts/install.php`

To avoid PHPStorm to index too much, exclude these directory:

- .data
- drupal/gc/test
- php/adminer
- web/ui (if you're not #ui dev)

## 3. Tools

- `php scripts/git/prune.php`
- `php scripts/git/pull.php` — pull master
- `php scripts/git/pull.php --confirm` — pull master with confirmation
- `php scripts/git/generate.php`
- `php scripts/gitlab/build-configuration.php`
- `php scripts/gitlab/deploy/staging.php`
- `php scripts/gitlab/deploy/production.php`
- `php scripts/ecs-ssh.php staging lo-staging` - to configure aws, see [this](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
- `php scripts/migration/util/fix-namespace.php` - fix namespace errors, because not all microservices come with new
  version of util library.
- Dummy: Generate dummy content for testing.
    1. Make sure the services are up. Ref (4).
    - `docker exec monolith_web_1 bash -c 'php /scripts/dummy/generate.php'`
- Run xdebug on certain request:
    - From Google Chrome > `Copy as CURL`
    - `bash -c "clear && docker exec -it monolith_web_1 sh"`
    - `php /app/scripts/xdebug.php $STRING_COPIED_FROM_GOOGLE_CHROME` 

## 4. TODO

- Install #realtime
- Install #cron
