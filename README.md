GO1 monolith
====

## 1. TODO

- Build the scripts as a single phar file.
- Check #ui build again.

## 2. Dependencies

- git
- php7
    - composer
- golang:
    - `composer global require symfony/yaml`
    - glide: `curl https://glide.sh/get | sh`
    - `export PATH="/path/to/monolith/scripts:$PATH"`

## 3. Usage

1. `php ./scripts/build.php`: Build the code base.
    - `--skip-php`: Don't run composer commands. 
    - `--skip-web`: Don't run npm commands.
    - `--skip-drupal`: Don't build drupal code base.
    - `--skip-go`: Don't build golang code base.
- `php start.php`, then try some links:
    - http://localhost/v3/
    - http://localhost/GO1/user/
- Run test cases:
    - `./scripts/test.php php/api`
    - `./scripts/test.php php/lo/tests/domain/tag/`
    - `./scripts/test.php php/api/tests/ProxyTest.php --filter=testStatusOfBlockedService`
    - `./scripts/test.php drupal/gc/modules/applications/aduro/modules/lms/lms_services/tests/Apiom/Course/CourseAccountsServicesTest.php`
- Run test cases without Docker:
    - `php ./scripts/phpunit.php php/outcome/`

To avoid PHPStorm to index too much, exclude these directory:

- .data
- drupal/gc/test
- php/adminer
- web/ui (if you're not #ui dev)

## 4. Control the services

- start/stop/restart
    - `docker-compose start`
    - `docker-compose stop`
    - `docker-compose restart`
- Or
    - `docker-compose up --force-recreate`
    - `ctrl+C` to stop.

## Tools

- php ./git/prune.php
- php ./git/pull.php
- php ./git/generate.php
- php ./gitlab/build-configuration.php
- php ./gitlab/deploy/staging.php
- php ./gitlab/deploy/production.php
- Dummy: Generate dummy content for testing.
    1. Make sure the services are up. Ref (4).
    - `docker exec -it monolith_web_1 php /scripts/dummy/generate.php`
