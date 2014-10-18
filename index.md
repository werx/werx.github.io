---
layout: packages
title: About
tab: about
---

# The Werx Project for PHP

## Overview

Werx is a micro-framework for PHP 5.4+. It brings together the building blocks you need to build a PHP application without all the overhead and learning curve associated with the big PHP frameworks.

### Features

I've combined some of the best [composer packages](https://packagist.org/) I could find to provide most of the core functionality:

- Database Abstraction ([Illuminate/Database](https://github.com/illuminate/database))
- HTTP Abstraction ([Symfony\HttpFoundation](https://github.com/symfony/HttpFoundation))
- Routing ([Aura.Router](https://github.com/auraphp/Aura.Router))
- Templating ([Plates](http://platesphp.com/))
- Unit Tests ([PHPUnit](https://github.com/sebastianbergmann/phpunit) of course, why use anything else?)

In addition to the above third party packages, I've also released a few component packages myself. As much as I wanted
to strictly use third party components, I've found instances where it made more sense to release my own reusable packages.

- [Configuration Management](/packages/config/)
    - Multiple Environment Support (local, test, prod, etc)
    - Extensible configuration providers (array and json supported out of the box)
- [Email Abstraction](/packages/email/)
    - Use <a href="https://github.com/EllisLab/CodeIgniter/">CodeIgniter's</a> email library outside CodeIgniter.
- [Forms](/packages/forms/)
    - Framework agnostic form helpers.
- [Url Builder](/packages/url/)
    - Uses [rize\UriTemplate](https://github.com/rize/UriTemplate) for the heavy lifting of expanding urls with additional functionality to make it easier to build action and asset urls to resources within your application.
- [Validation](/packages/validation/)
    - Provides set of standard validation methods and an input validation engine.

