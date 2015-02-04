---
layout: packages
title: Forms
tab: packages
---

<h1>Werx.Forms</h1>

[![Source](https://img.shields.io/badge/source-werx/forms-blue.svg?style=flat-square)](https://github.com/werx/forms) [![Build Status](https://img.shields.io/travis/werx/forms.svg?style=flat-square)](https://travis-ci.org/werx/forms) [![Total Downloads](https://img.shields.io/packagist/dt/werx/forms.svg?style=flat-square)](https://packagist.org/packages/werx/forms) [![Latest Stable Version](https://img.shields.io/github/tag/werx/forms.svg?label=version&style=flat-square)](https://packagist.org/packages/werx/forms)

<p class="lead">Framework agnostic form helpers.</p>

<ul>
    <li><a href="#about">About</a></li>
    <li><a href="#form-basics">Form Basics</a></li>
    <li><a href="#setting-form-data">Setting Form Data</a></li>
    <li><a href="#pre-populating-input">Pre-Populating Input</a></li>
    <li><a href="#input-builders">Input Builders</a></li>
    <li><a href="#extended-attributes">Extended Attributes</a></li>
    <li><a href="#installation">Installation</a></li>
</ul>

## About
I've never found form libraries terribly useful. In my experience, they either work only for the most simple forms, or end up being way more difficult to use than just writing the HTML by hand.

That said, I do need some really basic form helpers to easily pre-populate form fields (like from previous POST to correct errors or from a database) and build drop down boxes.

The primary goal of this library is to solve those 2 problems in a way I don't hate. If I can make some more robust form helpers that don't get in my way while I'm at it, all the better.

This library provides helpers for setting default values in otherwise static html form elements as well as an input builder for dynamically creating form elements.


> All examples below assume you are using the Composer Autoloader and have imported the correct namespace.

``` php
use werx\Forms\Form;
```

## Form Basics

Open a form:

``` php
<?=Form::open(['method' => 'POST', 'action' => 'home/submit'])?>
```

Close a form:

``` php
<?=Form::close()?>
```

## Setting Form Data

Whether using the included input builder or hand writing your html, you can set an array of data that will be available for pre-populating form input values.

```
<?php
Form::setData($_POST);
?>
```

## Pre-Populating Input

When using `Form::setData()`, you can dynamically populate text boxes and textareas and pre-select dropdowns, radios, and checkboxes.

### Text Boxes
``` php
<input type="text" name="username" id="username" value="<?=Form::getValue('username')?>">
```
We can also pre-populate with a default value if the data isn't available from `Form::setData()`. It defaults to null, but you can override this with a 2nd parameter to `Form::getValue()`.

``` php
<input type="text" name="username" id="username" value="<?=Form::getValue('username', 'josh')?>">
```
> There is also a third parameter to `Form::getValue()` that controls whether or not to escape the value. Default: true

### Textareas

Textareas work the same way.

``` php
<textarea name="comments" id="comments"><?=Form::getValue()?></textarea>
```

### Selects

Pre-select an item in a drop down using `Form::getSelected('name', 'option value')`

``` php
<select name="state">
	<option value="AR" <?=Form::getSelected('state', 'AR')?>>Arkansas</option>
	<option value="TX" <?=Form::getSelected('state', 'TX')?>>Texas</option>
	<option value="OK" <?=Form::getSelected('state', 'OK')?>>Oklahoma</option>
</select>
```

### Checkboxes and Radios

Checkboxes and Radios work the same way as Selects, using `Form::getChecked()`.

``` php
<input type="checkbox" name="pets" value="Cat" <?=Form::getChecked('pets', 'Cat')?> /> Cat
<input type="checkbox" name="pets" value="Dog" <?=Form::getChecked('pets', 'Dog')?> /> Dog
<input type="checkbox" name="pets" value="Fish" <?=Form::getChecked('pets', 'Fish')?> /> Fish
```

``` php
<input type="radio" name="color" value="Red" <?=Form::getChecked('color', 'Red')?> /> Red
<input type="radio" name="color" value="Blue" <?=Form::getChecked('color', 'Blue')?> /> Blue
<input type="radio" name="color" value="Green" <?=Form::getChecked('color', 'Green')?> /> Green
```

## Input Builders
When using the input builders to generate your html, values are automatically pre-populated / selected / checked based on the array passed to `Form::setData()`

### Text Boxes

``` php
<?=Form::text('username')?>
```
Renders: `<input name="test" id="test" type="text" value="josh" />`

Or you can try to automatically determine the value with a fall back value if not found.

``` php
<?=Form::text('username')->getValue('josh')?>
```

You can also manually set the value and ignore anything from `Form::setData()`.

``` php
<?=Form::text('username')->value('josh')?>
```

### Other Input Types:

Other text-based input types are also supported and work identically to `Form::text()`.

``` php
Form::textarea();
Form::hidden();
Form::password();
Form::email();
Form::url();
Form::tel();
```

### Selects

You can use the builder to create a select box and specify the options and an optional label. If this key was included in the array passed to `Form::setData()`, the proper item will be pre-selected.

``` php
<?=Form::select('state')->data(['TX' => 'Texas', 'AR' => 'Arkansas', 'OK' => 'Oklahoma'])->label('Choose');?>
```

Or you can manually select an option.

``` php
<?=Form::select('state')->selected('AR')->data(['AR' => 'Arkansas', 'TX' => 'Texas', 'OK' => 'Oklahoma'])?>
```

You can also dynamically select an option with default override if the value wasn't set in `Form::setData()`.

``` php
<?=Form::select('state')->getValue('AR')->data(['AR' => 'Arkansas', 'TX' => 'Texas', 'OK' => 'Oklahoma'])?>
```

In the previous example, we've used an associative array where the array keys were used for the option values and the array values where used for the option display text.

``` html
<select name="state" id="state">
    <option value="TX">Texas</option>
    <option value="AR">Arkansas</option>
    <option value="OK">Oklahoma</option>
</select>
```

Sometimes, you may want to use the array values for both the option values and the display text. To accomplish this, pass `true` as the second parameter to `data()`.

```php
<?=Form::select('color')->data(['Red', 'White', 'Blue'], true)->selected('White')?>
```

``` html
<select name="color" id="color">
    <option value="Red">Red</option>
    <option selected="selected" value="White">White</option>
    <option value="Blue">Blue</option>
</select>
```

### Custom Select Boxes

Two custom select boxes are provided: `selectState()` and `selectCounty()`. These behave exactly as the standard `select()`, but come preloaded with data.

```php
<?=Form::selectState('state')?>
```
> Note: In addition to the 50 U.S. States, the District of Columbia is also presented.

By default, the 2 character state code will be used as the value. If you want to use the full display name as both the display and the value:

``` php
<?=Form::selectState('state')->useDisplayNameForValue()?>
```

To generate a `<select>` list of counties, just tell it which U.S. State you want counties for.

``` php
<?=Form::selectCounty('county')->forState('AR')?>
```

If you want, you can get the array of state/county data easily enough.

``` php
$counties = \werx\Forms\DataSets::counties('AR');

/*
Array
(
    [0] => Arkansas
    [1] => Ashley
    [2] => Baxter
    [3] => Benton
    [4] => Boone
    [5] => Bradley
    ...
*/

$states = \werx\Forms\DataSets::states();

/*
Array
(
    [AL] => Alabama
    [AK] => Alaska
    [AZ] => Arizona
    [AR] => Arkansas
    [CA] => California
    [CO] => Colorado
    [CT] => Connecticut
    [DE] => Delaware
    ...
*/
```

### Checkboxes and Radios

You can also use the builder to create checkboxes and radios. As with the other elements, items will be pre-checked if they were passed to `Form::setData()`.

``` php
<?=Form::checkbox('pets')->value('Cat')?> Cat
<?=Form::checkbox('pets')->value('Dog')?> Dog
<?=Form::checkbox('pets')->value('Fish')?> Fish
```

``` php
<?=Form::radio('color')->value('Red')?> Red
<?=Form::radio('color')->value('Blue')?> Blue
<?=Form::radio('color')->value('Green')?> Green
```

You can force an item to be checked.

``` php
<?=Form::checkbox('pets')->value('Cat')?> Cat
<?=Form::checkbox('pets')->value('Dog')->checked()?> Dog
<?=Form::checkbox('pets')->value('Fish')?> Fish
```

``` php
<?=Form::radio('color')->value('Red')?> Red
<?=Form::radio('color')->value('Blue')->checked()?> Blue
<?=Form::radio('color')->value('Green')?> Green
```

Use the `checkedWhen()` method if you want to check something pending some condition. Pass any condition you like. If it evaluates to true, it will call `checked()` on the element.

``` php
<?=Form::radio('color')->value('Red')->checkedWhen($some_value == 'Red')?> Red
<?=Form::radio('color')->value('Blue')->checkedWhen($some_value == 'Blue')?> Blue
<?=Form::radio('color')->value('Green')->checkedWhen($some_value == 'Green')?> Green
```
> `checkedWhen()` works for both radios and checkboxes.

### Buttons

``` php
<?=Form::button('reset')->type("reset")->value('submit')->label('Start Over')?>
<?=Form::submit('submit')->value('Submit')->label('Search')?>
```


### Extended Attributes
With all dynamic form builders, you can chain methods together to add additional attributes like CSS classes or Inline Styles. Just call the method that corresponds to the attribute name you want, passing in the attribute value.

``` php
<?=Form::text('name')->class('form-control')->style('width: 250px')?>
```

``` php
<?=Form::submit('submit')->class('btn btn-primary')->value('Submit')->label('Submit')?>
```

You can also add the "required" attribute to any form element.

``` php
<?=Form::text('name')->required()?>
```


## Installation
This package is installable and autoloadable via Composer as [werx/forms](https://packagist.org/packages/werx/forms). If you aren't familiar with the Composer Dependency Manager for PHP, [you should read this first](https://getcomposer.org/doc/00-intro.md).

```bash
$ composer require werx/forms --prefer-dist
```

## Contributing

### Unit Testing

	$ vendor/bin/phpunit

### Coding Standards
This library uses [PHP_CodeSniffer](http://www.squizlabs.com/php-codesniffer) to ensure coding standards are followed.

I have adopted the [PHP FIG PSR-2 Coding Standard](http://www.php-fig.org/psr/psr-2/) EXCEPT for the tabs vs spaces for indentation rule. PSR-2 says 4 spaces. I use tabs. No discussion.

To support indenting with tabs, I've defined a custom PSR-2 ruleset that extends the standard [PSR-2 ruleset used by PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer/blob/master/CodeSniffer/Standards/PSR2/ruleset.xml). You can find this ruleset in the root of this project at PSR2Tabs.xml

Executing the codesniffer command from the root of this project to run the sniffer using these custom rules.


	$ ./codesniffer
