---
layout: home
title: About
tab: about
---

# The Werx Project for PHP

What is the Werx Project?

It is a "glue" project created by [Josh Moody](http://www.joshmoody.com) to bring together the components
you need to build an application without all the overhead and learning curve associated with the big PHP frameworks.

The Werx Project is *NOT* a framework.
 
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

The goal of this project is to combine into a cohesive package some of the best components in the community to meet these needs.
There are over 20,000 libraries available on [Packagist](https://packagist.org/). Surely there is one there to fit just about every need.

This is also an experiment to see if you really can build a system composed entirely of other components without actually using a framework (or building a framework in the process).

At present, the Werx project consists of 2 main projects:

- [werx\Core](https://github.com/werx/core) : A front controller that provides routing and dispatching. Includes the following components:

	- Config Management - [werx\Config](https://github.com/werx/config)
	- Database Abstraction - [Illuminate\Database](https://github.com/illuminate/database)
	- Form Validation - Still looking.
	- HTTP Abstraction [Symfony\HttpFoundation](https://github.com/symfony/HttpFoundation)
	- Routing - [Aura.Router](https://github.com/auraphp/Aura.Router)
	- Templates - [Plates](http://platesphp.com/)
	- Unit Tests - [PHPUnit](https://github.com/sebastianbergmann/phpunit) (of course, why use anything else?)

- [werx\Skeleton](https://github.com/werx/skeleton): A simple web application skeleton implementing the werx\Core features.

Check back soon to see how the project is progressing.

