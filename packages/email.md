---
layout: packages
title: Email
tab: packages
---

<h1>Werx.Email</h1>

[![Source](https://img.shields.io/badge/source-werx/email-blue.svg?style=flat-square)](https://github.com/werx/email) [![Build Status](https://img.shields.io/travis/werx/email.svg?style=flat-square)](https://travis-ci.org/werx/email) [![Total Downloads](https://img.shields.io/packagist/dt/werx/email.svg?style=flat-square)](https://packagist.org/packages/werx/email) [![Latest Stable Version](https://img.shields.io/github/tag/werx/email.svg?label=version&style=flat-square)](https://packagist.org/packages/werx/email)

<p class="lead">Use <a href="https://github.com/EllisLab/CodeIgniter/">CodeIgniter's</a> email library outside CodeIgniter.</p>

Full Documentation at <http://ellislab.com/codeigniter/user-guide/libraries/email.html>

<ul>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#installation">Installation</a></li>
    <li><a href="#testing">Testing</a></li>
</ul>

## Usage

```php
$email = new \werx\email\Message();
$email->clear();
$email->from('me@example.com', 'My Name');
$email->to('you@example.com');
$email->subject('your subject line');
$email->attach($attachment, 'attachment', 'filename.html', 'text/html');
$email->message('Message body goes here.');
$email->send();
```

## Installation
This package is installable and autoloadable via Composer as [werx/email](https://packagist.org/packages/werx/email). If you aren't familiar with the Composer Dependency Manager for PHP, [you should read this first](https://getcomposer.org/doc/00-intro.md).

```bash
$ composer require werx/email --prefer-dist
```

## Testing
There are unit tests available for this package. To run them, you must have [MailCatcher](http://mailcatcher.me/) installed and running.

You can also see the latest build on [Travis CI](https://travis-ci.org/werx/email).

``` bash
$ vendor/bin/phpunit
```
