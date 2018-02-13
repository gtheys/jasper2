---
layout: post
current: post
navigation: True
title: Couldn't Find User With Id=sign_in Devise on Rails 4
date: 2014-09-22 10:00:00 +0100
tags: programming rails
class: post-template
subclass: 'post tag-programming'
author: geert
---

When the default routes are created for devise you always get:

```
devise_for :users
resource :users
```

but I tried to write custom routes for devise and user to get invitations working. But I added the custom router below the ```resource :users```. This resulted in the following error:

```
Couldn't Find User With Id=sign_in Devise on Rails 3
```

But finally I figure it out. ```routes.rb``` gets read top down and firt rule that matched wins. So ```resource ;users```one over the devise rules. When I put it in the correct order it worked again :)