---
layout: packages
title: Skeleton
tab: packages
---

<h1>Werx.Skeleton</h1>

[![Source](https://img.shields.io/badge/source-werx/skeleton-blue.svg?style=flat-square)](https://github.com/werx/skeleton) [![Total Downloads](https://img.shields.io/packagist/dt/werx/skeleton.svg?style=flat-square)](https://packagist.org/packages/werx/skeleton) [![Latest Stable Version](https://img.shields.io/github/tag/werx/skeleton.svg?label=version&style=flat-square)](https://packagist.org/packages/werx/skeleton)

<p class="lead">Use this as a clean starting point in your Werx-based projects.</p>

## Installation

This library is on Packagist at [werx/skeleton](https://packagist.org/packages/werx/skeleton). It can be installed with [composer](https://getcomposer.org).

### Easy Installation

Just run the composer create-project command.

``` bash
$ composer create-project werx/skeleton your-directory-name --stability=dev
```

### Alternate Installation

Clone the repo and run composer install.

``` bash
$ git clone git@github.com:werx/skeleton.git your-directory-name
$ cd your-directory-name
$ composer install --stability=dev --prefer-dist
```

The default base namespace of your new app skeleton is werx\Skeleton. You can easily change the namespace in all files using the provided `install.php` after downloading this project.

``` bash
$ cd your-directory-name
$ php install.php "Your\AppNamespace"
```
> Of course, replace `Your\AppNameSpace` in the above command with whatever you want to use as your base namespace.

> __IMPORTANT__ : Make sure your new namespace is quoted in the install command as shown above.

If all went well, you should be able to point your browser at the directory where you installed the app and be rewarded with the welcome page.