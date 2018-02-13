---
layout: post
current: post
navigation: True
title: Rbenv and local gems
date: 2014-01-03 15:12:22 +0100
tags: programming rails
class: post-template
subclass: 'post tag-programming'
author: geert
---

Rbenv just felt more natural to me. I removed RVM from my system and installed rbenv. I'm not going in the discussion which one is better and why. You can read a good blogpost about the differences and/or similarities [here](http://jonathan-jackson.net/rvm-and-rbenv).

To solve the problem with gemsets the blog post mentions also [rbenv-gemset](https://github.com/jf/rbenv-gemset) which deals with different local gemsets. 

There are 2 main ways to do this.

### RVM style gem-sets

    cd ~/.rbenv/plugins
    git clone git://github.com/jamis/rbenv-gemset.git
    rbenv gemset create 1.9.3-p125 helloset
    >.rbenv-gemsets <<<helloset
    rbenv gemset active
    gem install ronn
    rbenv rehash
    rbenv gemset list


### Project specific gem-sets

I prefer this  because If I stop working on a project I can just delete this gemset together with the project.


    cd $PROJECT
    echo '.gems' > .rbenv-gemsets
    bundle install --path .gems


If you use git don't forget to update your .gitignore to exclude the .gems directory.