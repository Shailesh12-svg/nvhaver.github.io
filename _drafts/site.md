---
layout: post
current: post
cover: assets/images/scrable_internet.jpg
navigation: True
title: Project Site
date: 2018-05-12 18:10:30
tags: webdev
class: post-template
subclass: 'post'
author: nvhaver
---
# Goal

As a developer in both a professional and non-professional context, I have a lot of ongoing projects. So, I figured it was time to finally get myself a decent website to host them on. If you're reading this, it means that I finally managed to act on that idea.

The main idea of this project is to have one place for all my project files, code snippets and references. Any time I would want to pick up one of my projects, it should suffice to take a look at the corresponding page on this site.

# Requirements

- Simplicity: The site should be easy to set up and maintain, but not in the end-user sense... Otherwise I would have used WordPress.
- Security: The web server should, obviously, be secure.
- Minimalistic: The web server should have a small footprint in terms of resources as it should be able to run on a Raspberry Pi.

# Design
To select a colour scheme for the redesign of the site, Glen used [Coolors.co](https://coolors.co/).

# Future ideas

- Template for content pages
- Responsive design
- Event calendar
- Contact form
- Blog
- RSS feed on home page

# RSS Feed

I added an RSS reader in PHP to keep track of the latest news on the home page. One complication in the process of adding this feature is to do it in a secure fashion. The content provided by the RSS feed supplier cannot be trusted and as such needs to be filtered for executable content. Failing t do so would result in an XSS vulnerability. As a reference on how to perform this sanitisation, I looked at the references of [OWASP](https://www.owasp.org) and in particular the [PHP Cheat Sheet](https://www.owasp.org/index.php/PHP_Security_Cheat_Sheet) and the [Input Validation Cheat Sheet](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).

A valuable resource in this context is the white paper "[Feed Injection in Web 2.0](http://www.cgisecurity.com/papers/HackingFeeds.pdf)" by SPI Labs.

This project has its own [Taiga page](https://tree.taiga.io/project/nvhaver-experimentation-site/).

# References
[Pandoc user documentation](https://pandoc.org/MANUAL.html "Pandoc manual")

[Github Gist on use of Pandoc](https://gist.github.com/maxogden/97190db73ac19fc6c1d9beee1a6e4fc8 "Github Page")

Vim plugins:
- https://github.com/plasticboy/vim-markdown
- https://stackoverflow.com/questions/10964681/enabling-markdown-highlighting-in-vim
