---
date: '2024-12-30T12:39:10-05:00'
draft: false
title: 'Hello World, Part 2: Github Pages'
tags: ['Hugo', 'Github Pages']
---

I launched this site three weeks ago, deploying a Hugo build to [Github Pages](https://pages.github.com/). It was a steeper learning curve than I anticipated. Honestly, publishing anything is nerve-wracking - especially knowing every single change is out there for the world to see. I had to actively resist the urge to constantly fiddle with every little detail.

## GitHub Pages Deployment

Getting the site live on GitHub Pages wasn't entirely smooth. I had a simple HTML placeholder page sitting in my repository, which seemed to cause a delay. GitHub took a few minutes to recognize the new Hugo build after I pushed it. Turns out, understanding how GitHub Actions handles caching and updates requires a bit of research.

Here's a breakdown of the deployment:

1. Pushed my local repository to Github.
2. Changed "Build and deployment" source in my repository settings to "GitHub Actions."
3. Created a `.github/workflows/hugo.yaml` file using the template from the [Hugo documentation](https://gohugo.io/hosting-and-deployment/hosting-on-github/#procedure). This file tells Github how to build and deploy the site.
4. Committed the workflow file and pushed changes to the GitHub repository. This triggered the GitHub Actions deployment.

## Transparency

Having the website's code publically available is a double-edge sword. I believe transparency is a good thing, and maybe someone will learn from my setup or even contribute and help me fix something. But, it's also a bit terrifying. Every tweak, every typo fix, every half-baked experiement is permanently recorded in the repository's history. 

Is GitHub pages the right fit for a personal site?

## What's the alternative?

I'm currently working on the Google Cloud Data Analytics Certificate through [Google Cloud Skills Boost](https://www.cloudskillsboost.google/). It's got me thinking: maybe I should move the site to Google Cloud?

Potential advantages:
- **More control:** I'd have more control over the server enviroment and configurations.
- **GCP Integration:** I can play around with integrating other Google Cloud services.
- **Learning:** It would be a good way to reinforce what I'm learning in the Google Cloud Data Analytics Certificate program.

But, Google Cloud isn't free. Is the added expense worth it? And is it a good idea to commit to one cloud provider when there's AWS, Azure, Digital Ocean, Linode, and so many others to explore?

**More research is needed.**
