+++
title = 'Hello World, Part 1: Hugo'
date = 2024-12-08T13:38:04-05:00
tags = ['Hugo','Computers']

+++


This is it. The inaugural post. I've been delaying the inevitable for too long. Now, I have a ~~blog~~ portfolio website.

The following is an outline of my process for getting this ~~blog~~ up and running. So far. 

## Hugo or: How I Learned to Stop Worrying and Choose a Static Site Generator

I have no great love for making choices about tools. Even better, I like to hem and haw. I could spend all day reading about ottertail vs. beavertail canoe paddles. And, spend all night researching the most versatile socket wrenches. I've been paying attention to Static Site Generators for a while. I've even messed around with a few over the past decade. Locally deploying jekyll. Following Gatsby tutorials. 

But, I finally decided to actually publish this ~~blog~~ and I chose Hugo. 

Here's why:

- The docs are good and the community is helpful. I had a few initial questions about deployment and theming before starting. I easily found answers on the community pages.

- I want to start listing used books online again. That would require making this site bigger. A lot bigger. Hugo is fast. Even with a lot of pages! It also has some important SEO features that may come in handy.  *Note: I still believe static site ecommerce is possible*

- It's written in Go. Sure, I don't know anything about Go. Using Hugo doesn't require knowing anything about Go. But, I'm working on expanding my horizons. When I squint I can see that little blue Go logo zooming back and forth in the distance, just waiting for the first time I type `go build`. *Note: Go is back. Reaching #7 in November 2024 on the [Tiobe Index](https://www.tiobe.com/tiobe-index/go/)*

- I had to choose! Right now, I'm thinking of all the software I didn't choose at work... And, you know what? I know from experience that even with less than perfect tools I can often find creative workarounds. If I need to do that with Hugo, then so be it.

## Choosing a Theme

I chose [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/). No hemming and hawing this time. It was the first one listed on the [Hugo Themes Page](https://themes.gohugo.io/), and it has 10.4k stars on [github.](https://github.com/adityatelange/hugo-PaperMod)

## Installing

I bought a refurbished laptop two days ago. A Thinkpad X13s. Maybe I'll write a review about it in the future. But for now, just know that it is Windows on Arm. Arm-based PCs have amazing battery life. But, that battery life doesn't hold up with bare metal Linux installs. So WSL it is. Yes, I know there is a Windows binary for Hugo. But, WSL it is.

`Operating System: Ubuntu 24.04.1 LTS`

`Kernel: Linux 5.15.167.4-microsoft-standard-WSL2`

`aarch64`

Alright! Let's get down to business: `sudo apt install hugo`. Easy enough, right? *Foreshadowing.*

## The Quick Start Guide

In the Hugo [quick start guide](https://gohugo.io/getting-started/quick-start/) the first step after installing Hugo is to run the command `hugo version` to "verify that you have installed Hugo v0.128.0 or later." I did not do this.

Here is what I did:
- created a new directory and hopped into it, `$ mkdir home/hadley/websites && cd home/hadley/websites`
- created a new site `$ hugo new site portfolio --format yaml` PaperMod's installation guide recommends adding the `--format yaml` flag for ease of use. 
- ran `$ git init` Because I can't live without that version control.
- cloned the PaperMod theme submodule. `$ git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod`.
- ran `$ hugo server`

And then it happened...

![Terminal screenshot showing The First Error](/images/first_error.png)
the first error!

`ERROR => hugo v0.125.7 or greater is required for hugo-PaperMod to build`

It looks like the apt repository is out of date. So, I downloaded the latest extended version .deb from https://github.com/gohugoio/hugo/releases using `curl`.

`$ curl -OL https://github.com/gohugoio/hugo/releases/download/v0.139.3/hugo_extended_0.139.3_linux-arm64.deb`

Next, I installed with dpkg.

`$ sudo dpkg -i hugo_extended_0.139.3_linux-arm64.deb`

Okay. Let's check `$ hugo version`

Returns `hugo v0.139.3-2f6864387cd31b975914e8373d4bf38bddbd47bc+extended linux/arm64 BuildDate=2024-11-29T15:36:56Z VendorInfo=gohugoio`

I ran `$ hugo server` again.

![Success! Screenshot of hugo running on localhost](/images/success_2.png)
Success!

Alright! Now I've got Hugo running locally.

TO BE CONTINUED...

Stay tuned for:
- Using Gemini to help set up the hugo.yaml config file.
- Deploying the site to GitHub pages.