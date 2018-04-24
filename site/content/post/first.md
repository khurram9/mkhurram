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


### What about theme?
As I mentioned earlier, previously I had used TranquilPeak theme for Hexo and I was satisfied with it so I didn't want to experiment with a new theme. But since I was changing the backend from HEXO to HUGO, the question was how to port the theme? Luckilly a good soul [Thibaud Leprêtre](https://github.com/kakawait/hugo-tranquilpeak-theme) had already [ported](https://themes.gohugo.io/hugo-tranquilpeak-theme/) this theme for HUGO. Isn't it looking like a lucky day?


Now that all **decisions** are out of the way, lets move to the setup part.

Mainly I followed [an excellent guide](https://www.netlify.com/blog/2016/09/21/a-step-by-step-guide-victor-hugo-on-netlify/) from official Netlify guide page. Lets walk through it step by step.


# Step 1: Setup tools required on local machine

First of all install Node and NPM locally. Since I am working on MacOS I'll use Homebrew. Run the following command to install Node and NPM if these are not already installed.
```
$ brew install node
==> Installing dependencies for node: icu4c
==> Installing node dependency: icu4c
==> Downloading https://homebrew.bintray.com/bottles/icu4c-61.1.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring icu4c-61.1.high_sierra.bottle.tar.gz
==> Caveats
This formula is keg-only, which means it was not symlinked into /usr/local,
because macOS provides libicucore.dylib (but nothing else).

If you need to have this software first in your PATH run:
  echo 'export PATH="/usr/local/opt/icu4c/bin:$PATH"' >> ~/.bash_profile
  echo 'export PATH="/usr/local/opt/icu4c/sbin:$PATH"' >> ~/.bash_profile

For compilers to find this software you may need to set:
    LDFLAGS:  -L/usr/local/opt/icu4c/lib
    CPPFLAGS: -I/usr/local/opt/icu4c/include
For pkg-config to find this software you may need to set:
    PKG_CONFIG_PATH: /usr/local/opt/icu4c/lib/pkgconfig

==> Summary
🍺  /usr/local/Cellar/icu4c/61.1: 249 files, 67.2MB
==> Installing node
==> Downloading https://homebrew.bintray.com/bottles/node-9.11.1.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring node-9.11.1.high_sierra.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/node/9.11.1: 5,125 files, 49.7MB
```

This is is the only toolset that we'll need to setup and manage the blog locally. Now lets move to the next step.

# Step 2: Setup base HUGO on local machine
this step is also super simple just like the previous one. We first need to clone the github directory of base HUGO using git and then `cd` into the cloned directory.
```
$ git clone https://github.com/netlify/victor-hugo
$ cd victor-hugo
```
and then to install the HUGO on local machine run 
```
$ npm install
> fsevents@1.1.3 install /Users/[USER]/Documents/HUGO/victor-hugo/node_modules/fsevents
> node install

[fsevents] Success: "/Users/[USER]/Documents/HUGO/victor-hugo/node_modules/fsevents/lib/binding/Release/node-v59-darwin-x64/fse.node" is installed via remote

> uws@0.14.5 install /Users/[USER]/Documents/HUGO/victor-hugo/node_modules/uws
> node-gyp rebuild > build_log.txt 2>&1 || exit 0


> uglifyjs-webpack-plugin@0.4.6 postinstall /Users/[USER]/Documents/HUGO/victor-hugo/node_modules/uglifyjs-webpack-plugin
> node lib/post_install.js


> hugo-bin@0.22.0 postinstall /Users/[USER]/Documents/HUGO/victor-hugo/node_modules/hugo-bin
> del-cli vendor && node lib/install

  ✔ Hugo binary is installed successfully
added 2465 packages in 180.065s

```


# Step 3: Running the server on local machine
Now that NODE webserver and HUGO backend has been installed in your local machine, now its time to start the webserver and HUGO backend and see the blog in our web-browser. SO first lets start the webserver service.
```
$ npm start
```
Techincally HUGO backend should also have been installed after second last step we did. If it hadn't then you can alos use homebrew to install it. Try to type `$ hug` and then press `TAB` key and see if it auto-completes or some options are displayed in the terminal, If not then run the following command to install HUGO backend. (I forgot how I did it)
```
$ brew install hugo
```
Now lets create some (dummy) pages so that our webservice has something to display. 
```
$ cd site
$ hugo new aboutme.md
$ hugo new post site/content/first.md
```
Till this point our blog is setup (atleast on our own machine). To test it open a browser window and go to following address `127.0.0.1:1313`. You should be able to see a post with title `First`.

# Step 4: Installing Theme
Download theme from [HUGO TranquilPeak Github Page](https://github.com/kakawait/hugo-tranquilpeak-theme) as **ZIP** create new folder under site/themes and move the downloaded folder into it. Rename hugo-tranquilpeak-theme-master to hugo-tranquilpeak-theme.


# Step 5
Final setup.


# Step 6: Push to Github
Final setup.

# Step 7: Deploying on Netlify
Final setup.


“Peace of mind produces right values, right values produce right thoughts. Right thoughts produce right actions and right actions produce work which will be a material reflection for others to see of the serenity at the center of it all.”
― Robert M. Pirsig, Zen and the Art of Motorcycle Maintenance