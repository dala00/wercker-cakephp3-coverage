# wercker-cakephp3-coverage

CakePHP3 testing box for wercker with coverage

Change your database testing setting of app.php like

```php
'host' => env('MYSQL_PORT_3306_TCP_ADDR', 'localhost'),
'port' => env('MYSQL_PORT_3306_TCP_PORT', 3306),
```

wercker.yml example

```yaml
box: dala00/wercker-cakephp3-coverage
services:
  - id: mysql
    env:
      MYSQL_ROOT_PASSWORD: rootpasswd
      MYSQL_USER: testuser
      MYSQL_PASSWORD: testpasswd
      MYSQL_DATABASE: testdb

build:
  steps:
    - script:
        name: Install dependencies
        code: composer install

    - script:
        name: Run phpunit
        code: vendor/bin/phpunit --coverage-html coverage_html

coverage:
  steps:
    - add-ssh-key:
        keyname: COVERAGE_KEY
        host: hostname

    - create-file:
        name: write key
        filename: $HOME/.ssh/id_rsa
        overwrite: true
        content: $COVERAGE_KEY_PRIVATE

    - script:
        name: Set permissions for ssh key
        code: chmod 600 $HOME/.ssh/id_rsa

    - script:
        name: Scp coverage html
        code: scp -o StrictHostKeyChecking=no -r coverage_html/* user@host:~/your/up/path
```
