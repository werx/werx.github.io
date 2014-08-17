---
layout: packages
title: Packages
tab: packages
---

<h1>Werx.Url</h1>
<p class="pull-right"><a class="btn btn-info btn-sm" href="https://github.com/werx/url">View Source</a></p>

[![Build Status](https://travis-ci.org/werx/url.png?branch=master)](https://travis-ci.org/werx/url) [![Total Downloads](https://poser.pugx.org/werx/url/downloads.png)](https://packagist.org/packages/werx/url) [![Latest Stable Version](https://poser.pugx.org/werx/url/v/stable.png)](https://packagist.org/packages/werx/url)

<p class="lead">Build urls to resources in your app.</p>

Available as a standalone library or as a [Plates Extension](http://platesphp.com/extensions/).

This library uses [rize\UriTemplate](https://github.com/rize/UriTemplate) for the heavy lifting of expanding urls with additional functionality to make it easier to build action and asset urls to resources within your application.

<ul>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#plates-extension">Plates Extension</a></li>
    <li><a href="#installation">Installation</a></li>
</ul>

## Usage

### Creating an instance of the url builder.
Get an instance of the url builder. If you pass no constructor values, it will base all urls on the root-relative path to the currently executing script using `$_SERVER['SCRIPT_NAME']`.

``` php
$builder = new \werx\Url\Builder;
```

You can also provide a base url and script name.

``` php
$builder = new \werx\Url\Builder('/path/to/app/', 'index.php');
```

If you set a fully qualified url in the constructor, it will be used when building urls.

``` php
$builder = new \werx\Url\Builder('http://example.com/path/to/app/', 'index.php');
```

### Building URLs

Simple pattern replacement with a single `id`

``` php
$url = $builder->action('home/details/{id}', 5);
var_dump($url); # /path/to/app/index.php/home/details/5
```

Multiple pattern replacements

``` php
$url = $builder->action('home/{action}/{id}', ['action' => 'details', 'id' => 5]);
var_dump($url); # /path/to/app/index.php/home/details/5
```

Convert an array to a query string.

``` php
$url = $builder->query('home/search', ['name' => 'Josh', 'state' => 'AR']);
var_dump($url); # /path/to/app/index.php/home/search?name=Josh&state=AR
```

Build an url to a static resource in your application.

``` php
$url = $builder->asset('images/logo.png');
var_dump($url); # /path/to/app/images/logo.png
```

## Plates Extension
This library includes an extension that allows it to be used within [Plates Templates](http://platesphp.com/).

In your controller...

```php
$engine = new \League\Plates\Engine('/path/to/templates');
$ext = new \werx\Url\Extensions\Plates;
$engine->loadExtension($ext);

$template = new \League\Plates\Template($engine);
$output = $template->render('home');
```

Then in your view, you can call any of the `werx\Url\Builder` methods via `$this->url()->methodtocall()`.

```php
<a href="<?=$this->url()->action(home/details/{id}, 5)?>">Details</a>
<img src="<?=$this->url()->asset('images/logo.png')?>"/>
```


## Installation
This library is on Packagist at [werx/url](https://packagist.org/packages/werx/url). It can be installed and auto loaded with [composer](https://getcomposer.org).

Example composer.json

``` javascript
{
	"require": {
		"werx/url": "dev-master"
	}
}
```

Don't forget to include Composer's auto loader.

``` php
<?php
require 'vendor/autoload.php';
```

