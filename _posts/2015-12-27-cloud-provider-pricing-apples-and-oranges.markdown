---
layout: post
current: post
navigation: True
title: "Cloud provider pricing: apples and oranges"
date: 2015-12-27 10:00:00 +0100
tags: aws
class: post-template
subclass: 'post tag-aws'
author: geert
---

Today there was a good discussion on [HN](https://news.ycombinator.com/item?id=10794951) about differences between cloud providers. In particular between [Amazon Webservices](https://aws.amazon.com/) and [Linode](http://linode.com/). Almost always the discussion comes to price. It's something people tend to do.

I agree the price is important and also I'm interested in the price. I want to pay the correct price for my purchases. The problem we have here is that we are comparing oranges to apples. You can say I want a piece of fruit and start discussing why the price differs between them. But apples and oranges [are different](http://www.diffen.com/difference/Apples_vs_Oranges). And they will appeal to different people.

On a daily basis, I'm recommending cloud architecture to different clients. These customers range from small websites using WordPress/Drupal, to larger web and mobile apps. I believe there is a market for both types of providers. Which can cover different segments of the market.

At one end of the spectrum, we have AWS who provides various cloud services. The other side is a provider like Linode who provides virtual machines in the cloud. I'm not entirely fair as Linode provides some services too. But their offering is pale next to AWS. 

But is Linode cheaper? Let's check with the most basic cloud service there is. Does Linode have block storage?  No, they don't, you have to set it up yourself. The setup cost is a one time cost. Managing it is an ongoing operational cost. You have to hire people to monitor the server(s) and fix issues. From the top of my mind some problems that happen on a regular basis: diskspace, DDOS attacks, 0-day exploits, ... AWS has a whole range of cloud services where they will take care of all the engineering. 

There are several use cases where AWS is not the right choice. Of you don't need redundancy, global coverage or other services besides compute. Other providers then AWS have cheaper alternatives. It all depends on what you want and need. But don't expect to pay [apple prices for oranges](http://www.economist.com/blogs/graphicdetail/2014/04/daily-chart) :)
