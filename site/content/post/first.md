---
title: "How did I setup this blog? Notes for future reference."
date: 2018-04-22
draft: false
comments: false
showSocial: false
keywords: hugo, blog, tranquilpeak, netlify
---

<!-- toc -->

# Why hosting your own blog?
Given that there are great manged services availabe in the form of [Medium](medium.com) and may be a few others, the first question that may arise, especially in the mind of older Khurram who is always critical of the younger Khurram, why go through all the pain and hassle to manag and host your own blog? Is it worth the effort especially when you are not *really* into writing? Isn't it just a way to avoid doing the *real* [work](hazen.ai) which is more crucial? 

Well, as always my answer to future Khurram is: I *just* want to do it. Any problem? 

Back to the ral question of why buying (and in this case nurturing may be a better word) a cow when you can jsut the milk from market (and that is free too!)? and the answer is I am a CS grad and I should know this stuff or more appropriately this is a *fun* distraction and (hopfully) will be worth the effort.

Now tht this debate between two versions of inner self is settled (or not, who cares!) lets move to the real part. how will you set-up the blog?


# Step 0

The very first step is to note down a wish list of features or goals that you would like to achieve by the end of this project. In my case these were following (order deos not signify priority)

- it should have a place to write down things (aka a blog!)
- there should be some automation; the last experience of compiling & deploying everything manually was not very time efficient
- I should use the toolset which is *hip-hop* these days (as of the publishing date of this blog.)
- HTTPS instead of plain-boring-http?
- Free? or as minimum as it can get


## Design Constraints?

- Continuous Deployment (& potentially CI if I am writing my own chunk of code for back/front-end and plan to change it as I go)
- **Free/Cheap** but not at the cost of quality 
- Go is popular these days. So why not?
- TranquilPeak is a good theme. I had used it in my previous iteration and I was quite happy with it. So lets not change it.
- Containerization; I don't want to go through the hasssle of managing my Droplets manually and trying too hard to **not** break it


## What choices did I have & my design decisions 


### Where to deploy?
I previously hosted this blog on [DigitalOcean](digitalocean.com) ($5 droplet; dirtcheap and supercool service which is easy to manage and have plathora of guides available on almost all the applications that you potetially can have for an online hosting/infrastructure provider). As I'll have my own (virtual) machine I could potentially configure it for *CI/CD thing* and for any other feature that I want to have for this blog. But this time I happend to find [netlify](netlify.com), "an all-in-one platform for automating modern web projects" and I was impressed with their offering; a CDN based hosting with CD and a free (one-click-setup) HTTPS certificate at $0. What does a blind person want?


### What backend to use for this project?
Given that the a big chunk of setup issues (Hosting, Security, CD, CDN and $) were auto-sorted by choosing Netlify there only remained a couple of questions to answer before proceeding to the actual setup. 

This was also a rather easy choice. In previous iteration I had used Hexo blog and it was a good choice. This time I wanted to experiment with Go so HUGO, a close cousin of HEXO, was an easier option.


Now that all **decisions** are out of the way, lets move to the setup part.

# Step 1
# Step 2
# Step 3
# Step 4
# Step 5


“Peace of mind produces right values, right values produce right thoughts. Right thoughts produce right actions and right actions produce work which will be a material reflection for others to see of the serenity at the center of it all.”
― Robert M. Pirsig, Zen and the Art of Motorcycle Maintenance