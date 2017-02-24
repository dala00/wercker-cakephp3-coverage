# wercker-cakephp3

CakePHP3 testing box for wercker with coverage

Change your database testing setting of app.php like

```php
'host' => env('MYSQL_PORT_3306_TCP_ADDR', 'localhost'),
'port' => env('MYSQL_PORT_3306_TCP_PORT', 3306),
```

wercker.yml example

```yaml
box: dala00/wercker-cakephp3
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
        code: |
          composer install

    - script:
        name: Run phpunit
        code: |-
          vendor/bin/phpunit
```
