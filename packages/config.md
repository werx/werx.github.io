---
layout: packages
title: Config
tab: packages
---

<h1>Werx.Config</h1>

[![Source](https://img.shields.io/badge/source-werx/config-blue.svg?style=flat-square)](https://github.com/werx/config) [![Build Status](https://img.shields.io/travis/werx/config.svg?style=flat-square)](https://travis-ci.org/werx/config) [![Total Downloads](https://img.shields.io/packagist/dt/werx/config.svg?style=flat-square)](https://packagist.org/packages/werx/config) [![Latest Stable Version](https://img.shields.io/github/tag/werx/config.svg?label=version&style=flat-square)](https://packagist.org/packages/werx/config)

<p class="lead">Use environment-specific configuration files in your app.</p>

<ul>
    <li><a href="#directory-structure">Directory Structure</a></li>
    <li><a href="#usage">Usage</a>
        <ul>
            <li><a href="#arrayprovider">ArrayProvider</a></li>
            <li><a href="#jsonprovider">JsonProvider</a></li>
            <li><a href="#loading-a-configuration-group">Loading A Configuration Group</a></li>
        </ul>
    </li>
    <li><a href="#installation">Installation</a>
</ul>


## Directory Structure
For the default config providers, you'll need to create a directory structure somewhere in your project to hold your configuration files.

Recommended structure:

```
config/
    config.php
    another_config.php
    /local/
        config.php
    /test/
        config.php
    /prod/
        config.php
```

Default configs go in the root `config` directory. There must also sub-directories for each environment (local/test/prod) if you want to override the default setting when running in different environments.

##  Usage

```php
# Get an instance of the ArrayProvider
$provider = new \werx\Config\Provider\ArrayProvider('/path/to/config/directory');

# Create a Config\Container Instance from this provider.
$config = new \werx\Config\Container($provider);

# Load config/config.php
$config->load('config');

# Get an item.
$item = $config->get('foo');
```


### ArrayProvider
Create a .php file in your config directory that returns an array of config values.

``` php
<?php
return [
	'foo' => 'Foo',
	'bar' => 'Bar'
];
```

Get an instance of the `ArrayProvider` class, passing the path to your config directory to the constructor.

``` php
$provider = new \werx\Config\Provider\ArrayProvider('/path/to/config/directory');
```

### JsonProvider
Create a .json file in your config directory that returns an array of config values.

``` javascript
{
    "foo" : "Foo",
    "bar" : "Bar"
}
```

Get an instance of the `JsonProvider` class, passing the path to your config directory to the constructor.

``` php
$provider = new \werx\Config\Provider\JsonProvider('/path/to/config/directory');
```

### Creating new Config Providers
You can extend werx\config to create your own providers by implementing `\werx\config\Providers\ProviderInterface`

### Loading A Configuration Group
In this example, you would be loading the array from `config.php` in your config directory.

```php
$config = new \werx\Config\Container($provider);
$config->load('config');

```

### Get A Configuration Value

```php
$item = $config->get('foo');

print $item;
// Foo
```

### Get A Default Value
If a configuration item doesn't exist, `$config->get()` will return null. You can override the default return value by passing the new default as the 2nd parameter.

```php
$item = $config->get('doesnotexist', false);
var_dump($item);
// false;
```

### Loading Multiple Configuration Groups

If you have more than one configuration group to load, you can call `load()` multiple times, or you can pass an array of config groups.

```php
$config->load(['config', 'database', 'email']);
```

### Avoiding Collisions On Config Property Names

By default, if a configuration property name is found in multiple config files, the config value will be replaced each time that property name is found in a config file. If you prefer, you can tell the loader to index the config container with the name of the config group to prevent name collisions. This is accomplished by passing `true` as the second parameter to `load()`.

```php
$config->load(['config', 'email'], true);
```

Then to retrieve your indexed property name, call the "magic" method named the same as the config file you loaded.

``` php
// Get the value for the 'host' property from the 'email' configuration group.
$item = $config->email('host', 'smtp.mailgun.org');
```
> As with the `get()` method, the second parameter above is the default value if the config item doesn't exist.

Or you can return all of the items in the 'email' config group as an array by not passing any parameters.

```
$items = $config->email();
```

### Loading Environment-Specific Configuration Group
In this example, you would be loading the array from `config.php` and `test/config.php`. Keys from `test/config.php` will replace keys with the same name from `config.php`.

```php
$config->setEnvironment('test');
$config->load('config');
```

## Installation
This package is installable and autoloadable via Composer as [werx/config](https://packagist.org/packages/werx/config). If you aren't familiar with the Composer Dependency Manager for PHP, [you should read this first](https://getcomposer.org/doc/00-intro.md).

```bash
$ composer require werx/config --prefer-dist
```
