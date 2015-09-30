---
layout: post
title: "Use composer to manage your library dependencies for php"
date: 2014-01-23 12:38:17 +0800
comments: true
categories: PHP
---

If you use npm for Node, pypi for Python, or bundler for ruby, you will want to use [composer](https://getcomposer.org/doc/00-intro.md), _if you still use_ PHP.

<!-- more -->

To get started, [install](https://getcomposer.org/doc/00-intro.md) as per instruction.

For Mac, it will be a few `brew` commands.

```bash
brew tap josegonzalez/homebrew-php
brew install josegonzalez/php/composer
```

If you have some errors, you might need to `brew install php53-intl`, `brew update` and `brew doctor`. I have to do that for some [weird errors](https://github.com/Homebrew/homebrew/issues/24290).

Next, prepare a basic `composer.json` in your root directory. The below json is an example of a project that requires [monolog](https://github.com/Seldaek/monolog).

```json
{
    "require": {
        "monolog/monolog": "1.0.*"
    }
}
```

Install the library and it's dependencies (monolog in this example).

```bash
curl -sS https://getcomposer.org/installer | php
php composer.phar install
```

A `vendor` directory will be created and it contains all the libraries. You can start using the libraries as per normal.

Or use [autoload](https://getcomposer.org/doc/01-basic-usage.md).

## For developers with library/SDK to share..

If you have your library/SDK and you want to upload to composer repository, create a `composer.json` in your project root directory.

Here's [one](https://github.com/Hoiio/hoiio-php/blob/master/composer.json) for reference.

Then go to [packagist](https://packagist.org), register an account, and point it to your repository.
