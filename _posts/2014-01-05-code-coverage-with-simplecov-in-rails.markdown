---
layout: post
current: post
navigation: True
title: Code coverage with SimpleCov in rails
date: 2014-01-05 15:12:22 +0100
tags: programming rails
class: post-template
subclass: 'post tag-programming'
author: geert
---

I wanted my rails application to communicate with an API still under heavy development by my collegue. I wasn't writing test cases because we were still discovering how to implement this API. Not a best practice ;)

This also meant I lost sight what I covered in my test suite and what not. So I investigated a code coverage tool.

I found [SimpleCov](https://github.com/colszowka/simplecov) and it's indeed simple. It works for several test suites in rails and it's quite easy to setup. In my case it was Rspec.

Add SimpleCov to your `Gemfile` and bundle install:

     gem 'simplecov', :require => false, :group => :test
 
I'm using Rspec so at the top of `spec_helper.rb` (the very top):

     require 'simplecov'
     SimpleCov.start
     
     # Previous content of test helper now starts here
 
 Don' forget to:
 
      bundle
  
Now we can just run rspec spec and you will see something like:

     Coverage report generated for RSpec to /Users/me/Code/some_app/coverage. 1340 / 1570 LOC (85.35%) covered.
 
 in this folder there is a `index.html `file which can be opened in your browser.
 
I'm quite happy I still have 85% of my code covered with tests. It wasn't as bad as I thougt.
