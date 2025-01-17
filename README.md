# Facile.it Coding Standard

[![PHP Version](https://img.shields.io/badge/php-%5E7.2-blue.svg)](https://img.shields.io/badge/php-%5E7.2-blue.svg)
[![Build status](https://github.com/facile-it/facile-coding-standard/workflows/Continuous%20Integration/badge.svg)]( https://github.com/facile-it/facile-coding-standard/actions?query=workflow%3A%22Continuous+Integration%22+branch%3Amaster)
[![Code coverage](https://codecov.io/gh/facile-it/facile-coding-standard/branch/master/graph/badge.svg?token=HlbkT2oJiB)](https://codecov.io/gh/facile-it/facile-coding-standard)

[![Latest Stable Version](https://poser.pugx.org/facile-it/facile-coding-standard/v/stable)](https://packagist.org/packages/facile-it/facile-coding-standard)
[![Total Downloads](https://poser.pugx.org/facile-it/facile-coding-standard/downloads)](https://packagist.org/packages/facile-it/facile-coding-standard)
[![Latest Unstable Version](https://poser.pugx.org/facile-it/facile-coding-standard/v/unstable)](https://packagist.org/packages/facile-it/facile-coding-standard)
[![License](https://poser.pugx.org/facile-it/facile-coding-standard/license)](https://packagist.org/packages/facile-it/facile-coding-standard)
[![composer.lock](https://poser.pugx.org/facile-it/facile-coding-standard/composerlock)](https://packagist.org/packages/facile-it/facile-coding-standard)

Repository with all coding standard ruleset.


## Installation

Currently, [Composer](https://getcomposer.org/) is the only supported installation tool.

```
$ composer require --dev facile-it/facile-coding-standard
```

When you install it, a plugin will ask you some questions to setup your project automatically.

The installer will add a `.php-cs-fixer.dist.php` file in your project root directory,
then you can edit manually if you need some changes.

The CS config will be configured to find your project files using
composer autoload sources.

Only `psr-0`, `psr-4` and `classmap` autoloads are supported.

The installer will also add two scripts in your `composer.json`;

```php
"scripts": {
  "cs-check": "php-cs-fixer fix --dry-run --diff",
  "cs-fix": "php-cs-fixer fix --diff"
}
```

## Configuration

The installation configuration should be enough to use it.

If you need to change the CS config file, we suggest to don't edit the main `.php-cs-fixer.dist.php` file.

You can create a new file `.php-cs-fixer.php` with something like this:

```php
<?php

/** @var PhpCsFixer\Config $config */
$config = require __DIR__ . '/.php-cs-fixer.dist.php';

// change your configuration...
$config->setUsingCache(false);

return $config;
```

## Usage

To start code style check:

```
$ composer cs-check
```

To automatically fix code style:

```
$ composer cs-fix
```

## PhpCsFixer configuration

See [PhpCsFixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) GitHub page.

## Risky rules

Risky rules are dangerous. Starting from version `0.3.1` we removed
them from default configuration.

If you want to add these rules, you can create your own `.php-cs-fixer.php`
configuration:

```php
<?php

/** @var \PhpCsFixer\Config $config */
$config = include __DIR__ . '/.php-cs-fixer.dist.php';

$rulesProvider = new Facile\CodingStandards\Rules\CompositeRulesProvider([
    new Facile\CodingStandards\Rules\DefaultRulesProvider(),
    new Facile\CodingStandards\Rules\RiskyRulesProvider(),
    new Facile\CodingStandards\Rules\ArrayRulesProvider([
        // additional rules or rules to override
    ]),
]);

$config->setRules($rulesProvider->getRules());

return $config;

```

## Generate configuration

If you have any problem updating to a new version, you can regenerate
the default `.php-cs-fixer.dist.php` with the command:

```
$ composer facile-cs-create-config
```

```
$ composer facile-cs-create-config --help

Usage:
  facile-cs-create-config [options]

Options:
      --no-dev                   Do not include autoload-dev directories
      --no-risky                 Do not include risky rules
```

## Migrating to 0.3

Updating the plugin from v0.2 to v0.3 will not automatically update your
configuration. Since your previous configuration should be committed into 
your Git repository, we suggest to temporarily delete it and just run the
update, asking the plugin to create a new one; this way, you can use the
diff afterwards to merge your personal modifications onto the new 
configuration format.

### v0.3 default configuration

```
<?php

/*
 * Additional rules or rules to override.
 * These rules will be added to default rules or will override them if the same key already exists.
 */
$additionalRules = [];

$rulesProvider = new Facile\CodingStandards\Rules\CompositeRulesProvider([
    new Facile\CodingStandards\Rules\DefaultRulesProvider(),
    // new Facile\CodingStandards\Rules\RiskyRulesProvider(), // risky rules
    new Facile\CodingStandards\Rules\ArrayRulesProvider($additionalRules),
]);

$config = PhpCsFixer\Config::create();
$config->setRules($rulesProvider->getRules());

$finder = PhpCsFixer\Finder::create();
$autoloadPathProvider = new Facile\CodingStandards\AutoloadPathProvider();
$finder->in($autoloadPathProvider->getPaths());

$config->setFinder($finder);

return $config;
```
