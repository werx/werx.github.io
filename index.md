---
layout: packages
title: About
tab: about
---

# The Werx Project for PHP

## Overview

The Werx project is not a framework. It is a "glue" project created by [Josh Moody](http://www.joshmoody.com) to bring together the components
you need to build a PHP application without all the overhead and learning curve associated with the big PHP frameworks.

I appreciate the hard work that has gone into creating these amazing PHP frameworks. I just think full stack frameworks
are overkill for many developers working on small-ish projects.

Here's what I really need when writing applications (alphabetically):

- Configuration Management
	- Has to support per-environment settings (local, test, stage, prod)
- Database Abstraction
- Routing
- Templating
	- Native PHP. I don't need a templating language - I have PHP.
- Unit Tests

Not strictly required, but very useful:

- Object-oriented abstraction for HTTP requests/responses
- Form Validation

## Third Party Components

The goal of this project is to combine into a cohesive package some of the best components in the community to meet these needs.
There are over 20,000 libraries available on [Packagist](https://packagist.org/). Surely there is one there to fit just about every need.

Here are the primary packages that make up the heart of the Werx Project:

- Database Abstraction - [Illuminate\Database](https://github.com/illuminate/database)
- HTTP Abstraction [Symfony\HttpFoundation](https://github.com/symfony/HttpFoundation)
- Routing - [Aura.Router](https://github.com/auraphp/Aura.Router)
- Templates - [Plates](http://platesphp.com/)
- Unit Tests - [PHPUnit](https://github.com/sebastianbergmann/phpunit) (of course, why use anything else?)

## Werx Packages

In addition to the above third party packages, I've also released a few component packages myself. As much as I wanted
to strictly use third party components, I've found instances where it made more sense to release my own reusable packages.

<ul>
    <li><a href="/packages/config/">Config</a></li>
    <li><a href="/packages/url/">Url</a></li>
    <li><a href="/packages/validation/">Validation</a></li>
    <li><a href="/packages/email/">Email</a></li>
</ul>
