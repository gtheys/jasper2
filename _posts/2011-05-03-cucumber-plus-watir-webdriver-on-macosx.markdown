---
layout: post
current: post
navigation: True
title: Cucumber + watir webdriver on macosx
date: 2011-05-03 16:34:35 +0100
tags: programming testing
class: post-template
subclass: 'post tag-programming'
author: geert
---

As project manager I also took on the part time role as tester for a new project I'm working on. Together with my customer I have written cucumber test scenario's. When trying to automate the testing I needed to get watir working on macosx with cucumber.

These instructions will install and run watir webdriver with headless chrome. There are some [known issues with watir webdriver.](http://stackoverflow.com/questions/3504322/watir-webdriver-wait-for-page-load)

Make sure you also read the [FAQ](https://github.com/jarib/watir-webdriver/wiki/FAQ)

There are some [waiting issues](http://stackoverflow.com/questions/4356281/how-do-i-use-watirwaiterwait-until-to-force-chrome-to-wait). 

But you can circumvent this by adding:

    require "watir-webdriver/extensions/wait"

These give you access to following methods:

    Watir::Wait.until { "" }: where you can wait for a block to be true
    object.when_present.set: where you can do something when it's present
    object.wait_until_present:; where you just wait until something is present
    object.wait_while_present:; where you just wait until something disappears
    Instead of installing and configuring my environment to start testing. I just use the work of somebody else to get me started. It does everything for me.

First install de Chrome driver

install it in your PATH

    ln -s "/Applications/Google Chrome.app"/ \
            "/Applications/Chromium.app"
    ln -s "/Applications/Chromium.app/Contents/MacOS/Google Chrome" \
            "/Applications/Chromium.app/Contents/MacOS/Chromium"
        
Clone from git:

    git clone https://github.com/alisterscott/EtsyWatirWebDriver.git
    cd EtsyWatirWebDriver
    ./go

I just drop my .feature files in de feature dir accompanied with my step definitions.
