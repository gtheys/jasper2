---
layout: post
current: post
navigation: True
title: 2 tips on improving your releases
date: 2016-11-07 10:00:00 +0100
tags: devops
class: post-template
subclass: 'post tag-devops'
author: geert
cover: /assets/images/elephant.jpg
logo: assets/images/ghost.png
---

Everyone can start **doing** DevOps. It's all about collaboration and improving the way your developers and operations people work together. Focus is on Collaboration with the big C.

Why do we want to do DevOps? Most of the time it's the next step after implementing Agile. You have releasable code but aren't able to migrate it to production. We need to figure out how to release our code faster instead of letting it be __waste__ in a queue.

Most companies want a continuous delivery of working software in production. Many still have a long road to go before they can deliver in a steady stream into production.

If we want to reach the continuous delivery phase, we have to start somewhere.

> When eating an elephant take one bite at a time. -- Creighton Abrams

It is possible; we need to concentrate on that first bite. To identify where to take that first juicy bite. There are two major things you can do to improve.

Before we start a reminder what DevOps is about. It focusses on four principles. One of the principles is measurement. Before we start, we want to verify what to improve and how we're going to know it did.

Let's agree on om the following 2 KPI's

* how much time it takes to deliver a well-tested feature
* how long it takes before it will end up in production

## Size of releases

Releases are always too large. If your release scope is significant, it will take more time to release. Too many moving parts, too many different configs, too many people, just too much of everything.

The reason why releases got so big is that releasing was painful. Companies/People didn't like to do it. Actual this made it even harder to release and somewhere we got stuck in this rabbit hole.

What could be the problem? Does your Agile delivery team deliver code but is it not releasable? If this is the case, we will need to investigate and resolve this. Every iteration should produce working software. Maybe we need to define working software better because some developers will assume it works on my laptop as working code.

Maybe the Ops team is not that keen on releasing your software in production because it takes a lot of time and effort. Good automation can resolve this.

Too many dependencies can make releases painful. Just like on the code we need to decouple as much as possible.

The more we release, the more noticeable the improvements. 

## Method of releasing

Next to the size of your release. The method of releasing is also crucial. I have worked in environments where there was a release bible with all the manual steps needed to release the code. It was a huge word document.

Manual steps can be automated and we should spend time doing this. As we increase our release moments, we will have to execute the steps more frequent. So if they're automated our release will go faster and smoother.

As we codify our release we can also use practices as TDD on our release.

Most companies are not yet able to automate deployments completely. The ones that have full automated deployments are most of the time small companies that started from scratch.

The bigger companies that have a large legacy codebase have great difficulties in getting rid of the manual release process.

But don't despair. Just take one bite from that elephant. Pick 1 item to automate every release, and slowly you will reach your continuous delivery goal.  

# Conclusion

At first glance, it looks we're increasing our stress moments. The reason we did fewer releases was to reduce these moments. By improving your releases, you will reduce the stress.

It will appear counterintuitive at the beginning because it will be worse than before. As you implement improvements in small increments slowly, you will be better off than before.

Like in Agile implementing DevOps is done in small increments. __NEVER__ start a big project to automate the whole release; you will never catch up. Start now and make everything smaller!

Don't scale by creating something bigger. Scale by making things smaller but increase frequency.