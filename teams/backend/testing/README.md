# DRUPAL TESTS

[Drupal 8 Testing](https://www.drupal.org/docs/8/testing )

[Drupal 8 PHPUnit](https://www.drupal.org/docs/8/phpunit )

[Running PHPUnit Tests](https://www.drupal.org/docs/8/phpunit/running-phpunit-tests )

[PHPUnit](https://phpunit.de/ )

[Simpletest Countdown](http://simpletest-countdown.org/ )

[Convert all Simpletest web tests to BrowserTestBase (or UnitTestBase/KernelTestBase)](https://www.drupal.org/node/2735005 )

## PHPUNIT

If you created your Drupal site using composer, PHPUnit should already be installed.

Run PHPUnit tests from the <docroot>/core folder.

```
cd /var/www/html/drupal8.dev/web/core
../../vendor/bin/phpunit -v -c ../modules/mymodule/tests/src/Functional/MyTest.php
```

## DRUPAL TEST RUNNER

Configure the Drupal PHPUnit environment.

```
cp /var/www/html/drupal8.dev/web/core/phpunit.xml.dist /var/www/html/drupal8.dev/web/core/phpunit.xml
```

Edit phpunit.xml to fill in SIMPLETEST_DB, SIMPLETEST_BASE_URL,
BROWSERTEST_OUTPUT_DIRECTORY and uncomment the printerClass property in the
phpunit tag. See [Running PHPUnit Tests](https://www.drupal.org/docs/8/phpunit/running-phpunit-tests ) for more details.

```
/var/www/html/drupal8.dev/web $ drupal module:install simpletest
/var/www/html/drupal8.dev/web $ chown -R www-data:www-data sites/default/files
```

Note that if you are using a vhost URL you need to specify the --url arg as it
is not picked up from the phpunit.xml config file and defaults to http://localhost.

```
/var/www/html/drupal8.dev/web $ php core/scripts/run-tests.sh --url http://drupal8.dev --module mymodule
/var/www/html/drupal8.dev/web $ php core/scripts/run-tests.sh --url http://drupal8.dev --directory modules/mymodule
/var/www/html/drupal8.dev/web $ php core/scripts/run-tests.sh --url http://drupal8.dev --class "Drupal\Tests\mymodule\Unit\MyTest"
/var/www/html/drupal8.dev/web $ php core/scripts/run-tests.sh --url http://drupal8.dev --class "Drupal\Tests\mymodule\Functional\MyTest"
/var/www/html/drupal8.dev/web $ php core/scripts/run-tests.sh --verbose --clean
```
