---
layout: packages
title: Validation
tab: packages
---

<h1>Werx.Validation</h1>

[![Source](https://img.shields.io/badge/source-werx/validation-blue.svg?style=flat-square)](https://github.com/werx/validation) [![Build Status](https://img.shields.io/travis/werx/validation.svg?style=flat-square)](https://travis-ci.org/werx/validation) [![Total Downloads](https://img.shields.io/packagist/dt/werx/validation.svg?style=flat-square)](https://packagist.org/packages/werx/validation) [![Latest Stable Version](https://img.shields.io/github/tag/werx/validation.svg?label=version&style=flat-square)](https://packagist.org/packages/werx/validation)

<p class="lead">Provides set of standard validation methods and an input validation engine.</p>

<ul>
    <li><a href="#validators">Validators</a></li>
    <li><a href="#validation-engine">Validation Engine</a></li>
    <li><a href="#installation">Installation</a></li>
</ul>

## Validators

The Validator class can be used to quickly validate a single piece of input.

``` php
use werx\Validation\Validator;

$input = 'foo';
$valid = Validator::minlength($input, 4);
var_dump($valid);

/*
bool(true)
*/
```

The following validators are available. Each validator returns a bool. `true` = passed validation, `false` = failed validation.

``` php
bool required (mixed $input)
bool date (mixed $input [, $input_format = MM/DD/YYYY])
	# Other input formats available YYYY/MM/DD, YYYY-MM-DD, YYYY/DD/MM, YYYY-DD-MM, DD-MM-YYYY, DD/MM/YYYY, MM-DD-YYYY, MM/DD/YYYY, YYYYMMDD, YYYYDDMM
bool minlength(mixed $input, int $min)
bool maxlength(mixed $input, int $max)
bool exactlength(mixed $input, int $length)
bool greaterthan($input, int $min)
bool lessthan(mixed $input, int $max)
bool alpha(mixed $input)
bool alphanumeric(mixed $input)
bool integer(mixed $input)
bool float(mixed $input)
bool numeric(mixed $input)
bool email(mixed $input)
bool url(mixed $input)
bool phone(mixed $input)
bool zipcode(mixed $input)
bool startswith(mixed $input, string $match)
bool endswith(mixed $input, string $match)
bool contains(mixed $input, string $match)
bool regex(mixed $input, string $regex)
bool inlist(mixed $input, array $list)
```

## Validation Engine
The Validation Engine is used to validate a set of data against a set of rules.

### Usage
First, get an instance of the Validation Engine:

``` php
use werx\Validation\Engine as ValidationEngine;

$validator = new ValidationEngine;
```

Then add rules:

``` php
$validator->addRule('firstname', 'First Name', 'required|minlength[2]|alpha');
```
#### Parameters

- Form input name / array key of the element you are validating
- User friendly label for the element
- Pipe-delimited list of rules
	- Each rule corresponds to a method name from the Validator class
	- If the method accepts arguments, the args should be in square brackets after the rule name
		- Example: `minlength[2]`
		- Exception: methods which accept an array as parameter should be in curly brackets after the rule name.
			- Example: `inlist{red,white,blue}`
	- Except for the `required` validator, all validators will return true if the input is empty.
		- In other words, `minlength[2]` will only actually fire if you also add a `required` rule.

Now you can get a validation result.

``` php
$valid = $validator->validate($_POST);
```

#### Validating Input Arrays
Sometimes you aren't using a simple string as your input field name. Let's say your HTML input form is something like this:

``` html
<input type="text" name="volunteer[name]">
<input type="text" name="volunteer[email]">
```

To build a rule for in this scenario, separate the array name and key name with a period when adding your rule.

``` php
$validator->addRule('volunteer.name', 'Name', 'required|minlength[2]|alpha');
$validator->addRule('volunteer.email', 'Email Address', 'required|email');
```

#### Closures
In addition to predefined validation methods from the `Validator` class, you can also use [closures](http://www.php.net/manual/en/functions.anonymous.php) to create custom validation methods.

``` php
$closure = function ($data, $id, $label) {
	$message = null;
	$success = $data[$id] == 'Foo';

	if (!$success) {
		$message = sprintf('%s must equal "Foo"', $label);
	}

	return [$success, $message];
};

$validator->addRule('firstname', 'First Name', $closure);

$valid = $validator->validate($_POST);
```

Three values will be passed to your closure:

1. The full data set being validated.
2. The id of the element being validated.
3. The label for the element being validated.

The closure is expected to return an array.

- The first element of the array should be the validation result (`bool`).
- The second element of the array should be an error message to display if validation failed.
	- If validation passed, message may be null.

#### Rulesets
What if you want to save groups of rules instead of adding each rule individually every time you want to validate them?  We've got you covered.

Create a new class that extends `werx\Validation\Ruleset` and add your rules in the constructor.

``` php
namespace your\namespace\Rulesets;

use werx\Validation\Ruleset;

class Contact extends Ruleset
{
	public function __construct()
	{
		$this->addRule('firstname', 'First Name', 'required|minlength[2]');
		$this->addRule('lastname', 'Last Name', 'required');
		$this->addRule('phone', 'Phone Number', 'required|phone');
		$this->addRule('email', 'Email Address', 'required|email');
	}
}
```

Then when you are ready to validate this group of rules:

``` php
$contact_rules = new your\namespace\Rulesets\Contact;
$validator->addRuleset($contact_rules);
$valid = $validator->validate();
```

### Utility Methods

There are a couple utilities to make dealing with validation results easier.

##### getErrorSummary()
Returns a simple array containing a list of validation error messages.

``` php
if (!$valid) {
	$summary = $validator->getErrorSummary();
}

/*
Array
(
	[0] => First Name must only contain the letters A-Z.
	[1] => First Name must be at least 2 characters long.
	[2] => Last Name is a required field.
)
*/
```

##### getErrorSummaryFormatted()
Returns the error summary formatted as an html unordered list (`<ul>`).

##### getErrorFields()
Returns list of fields that had an error. Useful if you want to apply some decoration to your form indicating which fields had a validation errors.

``` php
if (!$valid) {
	$error_fields = $validator->getErrorFields();
}

/*
Array
(
	[0] => firstname
	[1] => lastname
)
*/
```

##### getRequiredFields()
Once you've added your rules, you can get back a list of required fields. This is useful when you want to indicate on your form
which fields must be completed.

``` php

$validator->addRule('firstname', 'First Name', 'required');
$validator->addRule('lastname', 'Last Name', 'required');
$validator->addRule('age', 'Age', 'required|integer');

$required = $validator->getRequiredFields();

/*
Array
(
	[0] => firstname
	[1] => lastname
	[2] => age
)
*/
```

##### addCustomMessage()
Allows you to set custom error messages.

When displaying the error messages, `{name}` will be replaced with the name of the field being validated. The rest of the field
is parsed with [`sprintf()`](http://php.net/sprintf) so that parameters like `minlength` can be placed in the returned error message.

Examples:

``` php
$validator->addCustomMessage('required', "You didn't provide a value for {name}!");
$validator->addCustomMessage('minlength', "Oops, {name} must be at least %d characters long.");
```

## Installation
This package is installable and autoloadable via Composer as [werx/validation](https://packagist.org/packages/werx/validation). If you aren't familiar with the Composer Dependency Manager for PHP, [you should read this first](https://getcomposer.org/doc/00-intro.md).

```bash
$ composer require werx/validation --prefer-dist
```

