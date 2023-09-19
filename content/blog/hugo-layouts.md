---
title: "Hugo Layouts"
date: 2023-09-18T18:48:13-04:00
draft: false
slug: hugo-layouts
tags: ['hugo']
---

Today was a somewhat frustrating lesson in Hugo layouts. For a little backstory here, I am working with Hugo after a several years long intention of making a new website for myself. I previously had a personal blog on Blogspot, which I stopped updating when social media largely replaced personal blogs for most people. I have since stopped using social media much anymore. So, back to blogging.

I chose Hugo because I wanted this new site to be static. Working as a sysadmin, part of the job is dealing with patching servers and occasionally having to clean up a compromised system. Static site generators make this site a much harder target. If there is no database, no account credentials to steal and no server-side code execution, you can't hack it. At least not easily. So, I get the appeal. Years ago, I started with Jekyll but didn't get very far. A colleague recently started converting legacy Wordpress sites into static versions for archival purposes, and this got me thinking about getting back to my own site.

I briefly considered [Pelican](https://getpelican.com/) as it was written in Python and used Werkzeug for templating, both things I am very comfortable with. After reading a bit about it and spying on what other professionals are doing these days, I decided to switch to Hugo before getting too invested. There was a procedure to automatically build the static site for Github Pages (my deployment target) and there seemed to be in general a lot more support and resources to pull from while figuring it out.

One issue that I have been fighting with several days and just make a breakthrough today on was how Hugo handles templates. In Django or Flask, you specify a function that responds to a URL scheme, it selects a template as a parameter and returns HTML. In Hugo, the template being used is less explicit and instead relies on some inferred data about where the Markdown file containing the text of a page resides. There is documentation about the [order](https://gohugo.io/templates/lookup-order/) in which templates are applied if the files exist.

However, after spending several hours fussing with this today I couldn't get it to work. I was trying to simply create a template for one-off pages that extended the [hugo-sustain](https://themes.gohugo.io/themes/hugo-sustain/) theme I selected for this project. The default ``single.html`` template for this theme assumes all pages are blog posts, and helpfully prints the timestamp under the heading. I wanted to create a template for my About and Publications pages that did not contain this functionality. No matter how many iterations of me trying to place an override in my ``layouts`` folder to match the "collection" as determined by the folder hierarchy, this didn't apply. Also frustrating, there is no level of log verbosity I can find with the builder or the testing web server to tell me what template file paths were tested and what was selected. In short, a closed system with insufficient feedback to figure out what was going on.

What I have ended up doing for the time being is including a ``layout: "standalone"`` variable definition in each page's front matter metadata. This lets me more closely mimic the behavior I am used to with template engines, which is to tell the renderer the template I want it to use. This is accompanied by including a ``layouts\_default\standalone.html`` Go template file that converts the data coming from the Markdown into HTML. It works, it isn't what the documentation told me to do, and I am going to leave it like this for now.

At some point in the future I have ambitions to make my own template, but that is more than I want to take on at the moment. Like all good projects, I am going to iterate on this, I will make mistakes, and I will learn from them.