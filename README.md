# Minimum reproducible case for [57071](https://github.com/symfony/symfony/issues/57071)

## Steps to reproduce:

```bash
git clone git@github.com:heiglandreas/symfony-reproducer-57071.git
cd symfony-reproducer-57071
composer install
./vendor/phpunit/phpunit/phpunit
```

This should result in this console output:

```
PHPUnit 10.5.20 by Sebastian Bergmann and contributors.

Error in bootstrap script: PHPUnit\Event\Code\NoTestCaseObjectOnCallStackException:
Cannot find TestCase object on call stack
#0 /repo/vendor/phpunit/phpunit/src/Runner/ErrorHandler.php(64): PHPUnit\Event\Code\TestMethodBuilder::fromCallStack()
#1 /repo/vendor/symfony/phpunit-bridge/DeprecationErrorHandler.php(381): PHPUnit\Runner\ErrorHandler->__invoke(2, 'include(/repo/....', '/repo/tests/boo...', 7)
#2 /repo/vendor/symfony/phpunit-bridge/DeprecationErrorHandler.php(132): Symfony\Bridge\PhpUnit\DeprecationErrorHandler::Symfony\Bridge\PhpUnit\{closure}(2, 'include(/repo/....', '/repo/tests/boo...', 7, Array)
#3 /repo/tests/bootstrap.php(7): Symfony\Bridge\PhpUnit\DeprecationErrorHandler->handleError(2, 'include(/repo/....', '/repo/tests/boo...', 7)
#4 /repo/tests/bootstrap.php(7): include('/repo/vendor/sy...')
#5 /repo/vendor/phpunit/phpunit/src/TextUI/Application.php(292): include_once('/repo/tests/boo...')
#6 /repo/vendor/phpunit/phpunit/src/TextUI/Application.php(103): PHPUnit\TextUI\Application->loadBootstrapScript('/repo/tests/boo...')
#7 /repo/vendor/phpunit/phpunit/phpunit(104): PHPUnit\TextUI\Application->run(Array)
#8 {main}
```

Then run 

```
composer update symfony/phpunit-bridge:6.4.x-dev#a33ca737283c76617c4089a8425c7785b344e283
./vendor/phpunit/phpunit/phpunit
```

which will run the one test successfully

```
PHPUnit 10.5.20 by Sebastian Bergmann and contributors.

Runtime:       PHP 8.3.2RC1
Configuration: /repo/phpunit.xml.dist

.                                                                   1 / 1 (100%)

Time: 00:00.007, Memory: 8.00 MB

OK (1 test, 1 assertion)
```

Go back to the broken version via

```
composer update symfony/phpunit-bridge:6.4.x-dev
```
