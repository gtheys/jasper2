---
layout: post
current: post
navigation: True
title: "AWS cli: quering multiple accounts"
date: 2015-08-01 10:00:00 +0100
tags: aws
class: post-template
subclass: 'post tag-aws'
author: geert
---

At [skyscrapers](http://skyscrapers.eu) we manage the aws infrastructure for multiple clients. We automated accessing the different aws consoles with a pretty cool script. It gives our federated user automatically a login url and opens a browser accessing this url. It uses 2FA and the access is only temporary to add more security. Maybe this is a writeup on itself ;)

But switching between these consoles takes time. Especially just to get a private ip of a server or query some basic information. Last time I was working on a production and staging environment for 1 client. I needed to switch every 5 minutes between environment to get some information.

It was slow especially on my slow [Thai 3G line](http://geerttheys.com/remote/skyscrapers/2015/07/10/remote-working-in-a-far-away-timezone-preparation.html) it took very long just to load the console.

I wanted to simplify switching between accounts and queuing data. After some investigation I learned that the  aws cli had all I ever needed.

# Installing the aws cli

I started with [installing the aws cli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html). Don't forget to setup the [command completion](http://docs.aws.amazon.com/cli/latest/userguide/cli-command-completion.html). This is a huge timesaver! Instead of looking up the exact syntax the completer will help you narrowing down the options. As a shell I use [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) and there is [a plugin](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/aws/aws.plugin.zsh) to use the aws command completion.

If you tab after the describe- you get all the describe options for the ec2 command:

```
› aws ec2 describe-
describe-account-attributes                describe-import-image-tasks                describe-placement-groups                  describe-spot-datafeed-subscription        describe-vpc-attribute
describe-addresses                         describe-import-snapshot-tasks             describe-prefix-lists                      describe-spot-fleet-instances              describe-vpc-classic-link
describe-availability-zones                describe-instance-attribute                describe-regions                           describe-spot-fleet-request-history        describe-vpc-endpoint-services
describe-bundle-tasks                      describe-instance-status                   describe-reserved-instances                describe-spot-fleet-requests               describe-vpc-endpoints
describe-classic-link-instances            describe-instances                         describe-reserved-instances-listings       describe-spot-instance-requests            describe-vpc-peering-connections
describe-conversion-tasks                  describe-internet-gateways                 describe-reserved-instances-modifications  describe-spot-price-history                describe-vpcs
describe-customer-gateways                 describe-key-pairs                         describe-reserved-instances-offerings      describe-subnets                           describe-vpn-connections
describe-dhcp-options                      describe-moving-addresses                  describe-route-tables                      describe-tags                              describe-vpn-gateways
describe-export-tasks                      describe-network-acls                      describe-security-groups                   describe-volume-attribute
describe-image-attribute                   describe-network-interface-attribute       describe-snapshot-attribute                describe-volume-status
describe-images                            describe-network-interfaces                describe-snapshots                         describe-volumes
```

# Setting up read only access to all the different aws accounts.

To make it a bit more secure and I use this only to query aws we'll setup an read-only account. We use terraform and puppet to setup and install instances.

There are suffcient google search sources [like this one](http://support.cloudcheckr.com/getting-started-with-cloudcheckr/adding-your-credentials-to-cloudcheckr/creating-read-only-policy/) on how to do this.

Extract your `aws_access_key_id` and `aws_secret_access_key`.

# Setup multiple accounts for aws cli

Create in your home directory a `.aws` folder if not already exists. This folder need to contain 2 files: `config` and `credentials`.

In the credentials file you just have to add your keys:

```ini
[default]
aws_access_key_id = XXX
aws_secret_access_key=XXX
[id-mob-staging]
aws_access_key_id = XXX
aws_secret_access_key = XXX
[id-mob-prod]
aws_access_key_id = XXX
aws_secret_access_key = XXX
[bs]
aws_access_key_id = XXX
aws_secret_access_key = XXX
```

In the config file:

```ini
[default]
region = eu-west-1
[profile id-mob-staging]
output = table
region = eu-west-1
[profile id-mob-prod]
output = table
region = eu-west-1
[profile bs]
output = json
region = eu-west-1
```

The option I keep switching in this file will be the `output`. When I want it readable to me I put `table` when I want to do some funky filtering I use `json`.

Let's test this:

```
› aws ec2 describe-instances
{
    "Reservations": []
}
```

This will query my `default` account which is my personal account. No instances started as I'm a cheapskate :)

Let's try with another profile:

```
› aws ec2 describe-addresses --profile id-mob-staging
-------------------------------------------------------------------------------------------------------------------------------------------------------------
|                                                                     DescribeAddresses                                                                     |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
||                                                                        Addresses                                                                        ||
|+-------------------+--------------------+---------+-------------+---------------------+--------------------------+--------------------+------------------+|
||   AllocationId    |   AssociationId    | Domain  | InstanceId  | NetworkInterfaceId  | NetworkInterfaceOwnerId  | PrivateIpAddress   |    PublicIp      ||
|+-------------------+--------------------+---------+-------------+---------------------+--------------------------+--------------------+------------------+|
||  eipalloc-000     |  eipassoc-fffff    |  vpc    |  i-e0000    |  eni-6sssssa        | 35467754                 |  10.0.0.137        |  52.52.52.53     ||
```

This queried the `id-mob-staging` credentials and the config said table style. As you can see quite handy.

# Adding --filter

As I told before in this article I most of the time I query private and public ip addresses. I only need to know this for a particular type or set of servers.

Let's query all the staging webservers:

```
› aws ec2 describe-instances --filter Name=tag:class,Values=web Name=tag:Env,Values=staging --profile bs
```

Off course it depends how you use tags in amazon and you setup will be different. Using sound tagging strategy can simplify tghe usage of aws cli as querying tool.

This gives me as output a json file or a table depending on your `.aws/config`settings.

# Some jq in the mix

As a json file is hard to parse and I just want to see 1 specific entry we can do some more cli trickery. I love the cli and I love tools designed for the cli. JSON is hard to parse using traditional awk/sed toolchain. But [Jq](http://stedolan.github.io/jq/tutorial/) to the rescue. On macosx we can install it using [brew](http://brew.sh): `brew install jq`

Now let's show an example:

```
› aws ec2 describe-instances --filter Name=tag:class,Values=web Name=tag:Env,Values=staging --profile bs | jq '.Reservations[].Instances[].PublicDnsName'
"ec2-52-52-52-52.eu-west-1.compute.amazonaws.com"
"ec2-52-52-52-53.eu-west-1.compute.amazonaws.com"
```

# what's next?

This example can be extended to a more complete bash script. Accepting differnt variables for `class` or `env` or to switch between staging and production. 

You can use your imagination and try to find other use cases. The aws cli has a large set of options which can be applied to all kinds of use cases. If you find some don't hesitate to post in the comments.

# What can be improved

The credentials in my `.aws/credentials` are not [best practice](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html). The script I talked about in the beginning that generates an url to the correct console also generates temporary `aws_access_key_id` and `aws_secret_access_key`. Which are only temporary usable so more secure! My next step is to edit this script so I can easily switch between environments using the more safe tempaorary `aws_keys`. Switching using this script means using my phone for 2FA... But security always has a cost :)
