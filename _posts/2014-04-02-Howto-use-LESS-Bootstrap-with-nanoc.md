---
layout: post
current: post
navigation: True
title: Howto use LESS Bootstrap with nanoc
date: 2014-04-02 10:00:00 +0100
tags: programming
class: post-template
subclass: 'post tag-programming'
author: geert
---

I had some trouble getting nanoc to work with bootstrap LESS. But it wasn't so difficult to setup.

First download bootstrap with less: [getbootstrap](https://github.com/twbs/bootstrap/archive/v3.1.1.zip)

Unzip this file and copy the LESS folder into your nanoc content folder. I copied all the `*.less*`files into `mysite/content/assets/bootstrap`


Edit your `Rules`file.

{% highlight ruby %}
compile '/assets/bootstrap/bootstrap' do
    filter :less
    filter :rainpress
end

route '/assets/bootstrap/bootstrap' do
  item.identifier.chop + '.css'
end

## hack:
##       Bootstrap.less already processed and routed
##       But don't want to use the other less files :)

route '/assets/bootstrap/*/' do
end
{% endhighlight %}

You only compile the bootstrap.less and need to exclude the other `*.less`files. I feel I did it on a non-elegant way but it did the trick.

Next step is to edit the default.html where you indicate to use your new compiled bootstrap.css.

{% highlight html %}
<link href="/assets/bootstrap/bootstrap.css" rel="stylesheet">
{% endhighlight %}

After running `nanoc compile` you should be using your own compiled bootstrap.css. Now you can edit the variables.less to edit the default behaviour or add your own less files. Don't forget to import them or add them to the `Rules`

Also you can add the bootstrap javascripts to assets folder if you need them. Again add this to the `Rules`and maybe use the minify option.
