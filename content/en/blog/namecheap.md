---
title: "Using namecheap domain with github pages!"
description: Using a namecheap domain to use with a github page site, and deploy it with https!
date: 2022-09-02T18:45:39-05:00
image: "images/namecheap.png"
license: GPLv3
comments: true
draft: false
toc: true
categories: "Productivity"
tags: ["Blog"]
---

Hello everyone! It's crazyc4t once again, I have some news, I changed my domain! There is no more saidneder.tech, now it's [crazyc4t.xyz](https://crazyc4t.xyz) since my saidneder.tech domain was really expensive for renewal and I felt that it was my mistake because of the company that I chose to buy my domain from, that is HostGator, which I totally don't recommend, difficult to use and configure, expensive, and not friendly at all, so instead of renewing my old domain I bought a new one in [namecheap.com](https://namecheap.com)! and It was awesome, I deployed my blog to my new domain with https encryption in less than 30 minutes! And that was because of the wait time to generate the security certificate, so in this post am going to show you how to use your domain bought from namecheap with github pages!

## Making your github repository public

This is really easy, if you already have your github repository public, you can skip this step, but if you don't you can head over to settings in your repo and in the general tab of it, scroll all the way down over to the **danger zone**, and change the visibility of the repo.

![Danger zone](/images/danger.png)

## Publish your source branch

This is really easy as well, you need to deploy the source branch of your website, by doing so going into settings of your website repo, head over to the pages branch and click the save button.

![Branch](/images/branch.png)

## Time to add the domain!

Head over to [namecheap.com](https://namecheap.com) and login with your credentials, go over domain list, click the "manage" button over your domain.

![manage](/images/manage.png)

Go to advanced DNS, once there we will add github's pages IPs addresses, for linking our domain with them, so we will add 4 A records, with the same host of "@", with the following IPs:

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

Also we will add one CNAME record, with the address that github gives you for your website, for example: "username.github.io"

![records](/images/records.png)

## Adding the domain to github!

This is the step that will take a bit of time, since github will need to make a DNS checking of your domain and it will create the SSL certificate for your https address, first of all, add to the root of your repo, a file name "CNAME" and put your domain in there, for example:

`crazyc4t.xyz`

With no `https://` at the beginning, just the domain, after that head over to your repo settings, go to pages, and add your domain and save it, remember clicking "enforce https", like so:

![pages](/images/pages.png)

After that, check that your website is deployed and go to it!

## Outro

This was a quick blogpost on how to add your namecheap domain to github's pages, which it was really easy as you saw, hope you enjoyed the read, have a good one!
