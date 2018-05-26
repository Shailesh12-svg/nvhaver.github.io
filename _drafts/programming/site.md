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

At first I set out to create my own website using PHP, but I soon discovered that this technology was rather cumbersome to use. In the end I was more focused on developing the PHP code than I was on the actual content. Having a site you created from scratch is nice, but it should not hinder you from creating content.

In a second attempt, I decided that the site should be centered around the content. As such, the content would be written in the MarkDown format. This format is easy to use and comes quite naturally to me. However, webservers serve webpages, not MarkDown files. In stubbornly sticking to the "not made here" mentality, I started development on a templating engine to convert the .md files into HTML5 content. My nephew, Glen Van Haver, helped me out the design for the site at that time, because his taste for design is far superior to mine. 

The engine picks up any submitted files using incron. Once a new file is added, incron will call a custom script which calls pandoc and tidy to convert the .md file into HTML5 partials and to validate respectively. These partials would then be pasted into a pre-made template to construct the final page. The resulting static pages are then put in the website's content folder, which is served by Nginx.  

While working with the first version of this engine, I felt that my way of hosting the server, initially using a Raspberry Pi, was not the way to go. Things definitly worked, but required quite some manual intervention to deploy new content and to configure the Nginx server correcty. Although this is part of the learning process, it was again hindering me to create content.

After talking about this undertaking with Pieter Van Goethem and Xavier Geerinck, they both independently recommended me to take a look at GitHub pages with the Jekyll templating engine and Travis CI.

As a result, you are now reading this blog post. 

# Requirements

The requirements that this site needed to fill are:

- Simplicity: The site should be easy to set up and maintain, but not in the end-user sense... Otherwise I would have used WordPress.
- Security: The web server should, obviously, be secure.
- Minimalistic: The web server should have a small footprint in terms of resources.
- Content-first: The emphasis lies on the creation of content. It should be a platform for all my notes on ongoing projects.

# Future ideas

There are still some features I would like to include in the website. In particular, I would like to be able to keep track of upcoming events and have a constant RSS feed on the latest security news.

## RSS Feed

During earlier iterations of the website, I experimented with adding an RSS feed in PHP to keep track of the latest news on the home page. One complication in the process of adding this feature is to do it in a secure fashion. The content provided by the RSS feed supplier cannot be trusted and as such needs to be filtered for executable content. Failing to do so would result in an XSS vulnerability. As a reference on how to perform this sanitisation, I looked at the references of [OWASP](https://www.owasp.org) and in particular the [PHP Cheat Sheet](https://www.owasp.org/index.php/PHP_Security_Cheat_Sheet) and the [Input Validation Cheat Sheet](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).

A valuable resource in this context is the white paper "[Feed Injection in Web 2.0](http://www.cgisecurity.com/papers/HackingFeeds.pdf)" by SPI Labs.

TODO: This project has its own [Taiga page](https://tree.taiga.io/project/nvhaver-experimentation-site/).

## References

- [Pandoc user documentation](https://pandoc.org/MANUAL.html "Pandoc manual")
- [Github Gist on use of Pandoc](https://gist.github.com/maxogden/97190db73ac19fc6c1d9beee1a6e4fc8 "Github Page")

Vim plugins:
- https://github.com/plasticboy/vim-markdown
- https://stackoverflow.com/questions/10964681/enabling-markdown-highlighting-in-vim
