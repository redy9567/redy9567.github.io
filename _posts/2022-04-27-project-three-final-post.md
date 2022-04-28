---
layout: post
title:  "Build Optimization Tool"
summary: "Building, but with de-duplication"
author: brandonlabbe
date: '2022-04-27 01:35:23 +0530'
category: ['jekyll','guides', 'sample_category']
tags: jekyll
thumbnail: /assets/img/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes, multi categories and tags
usemathjax: true
permalink: /blog/project-three-final-post/
---

## Build Optimization Tool

Game Development is a very time-consuming and process. It usually involves very iterative development, where each week's work builds off of the previous. Recently, I have been wrapping up my first year-long project, Project Nautilus. Looking at our almost-final product, I can't help but realize how LARGE our repository has become. Looking at this, I can't help but think how large other games that require much more time and work than ours can become. Take any AAA title, and the amount of space it can take up dwarfs what we have used for Nautilus. This large space-consumption can be very problematic very quickly. Because Game Development requires constant iterative direcation, it needs constant testing. For constant testing, you need constant builds. However if you created nightly builds of a 100+ GB project, then it would come close to a TB at the end of the week. I don't know about you, but my hard drive would be full by the end of the month!

Looking at multiple builds that have been made for Nautilus, an idea came to my mind. When making consistent builds, much of the data that is built will carry over from one build to the next. For example, if you add a new level to your game, it is not like the old levels were changed and need to be rebuilt, however, the engine builder is going to recompile, and restore all that data anyways! This leads us into the solution! Deduplication. If we are able to split our builds into parts, we can compare these parts to see what has changed. If from one build to the next we find parts that aren't changed, we won't store those parts. By storing only changed parts, we could rebuild Build2 by grabbing the unchanged parts from Build1, and the changed parts from Build2 to recreate our 2nd Build.

Knowing this, I decided to set out to find a way to make this possible! For this project, I used the Windows Disk Management Tool (DISM) to create image files of my build. These Image files will take snapshots of my build folders and put them into their individual image files. These image files can later be unpacked to reveal the contents. The benefit to using these image files is that they are compressed and have functionality to split into multiple image files. In my project, I have batch files that will package a Unity project into multiple image files and take a SHA256 hash of each piece. If you don't know what a hash function is, it is a function that takes data as an input, and maps it to a string that is representative of that data. So by renaming each image piece to be it's hash, I can quickly compare pieces from one build to the next by comparing their hashes. If they are the same, then their data is identical.

I was able to do this and get two different builds split up, hashed, and ready to compare, however, when I did so, every single hash was different. This is because I ran into the issue of piece boundaries. Because DISM determines where to split one image file into the next, I couldn't guarantee that adding one file to the build wouldn't change how the build was split up, and hence changing every single piece and hash of the build.

Unfortunately, I don't have more time to put into this project, and if I did, my next step would be looking into making these chunk boundaries much more consistent. By doing this, we can then have more consistency of the image pieces. If we achieve that, then by adding a file to a build, only one of the images files should have a different hash. We could then store text files of the list of image pieces that make up a build, and use that file as a guide later to rebuild old builds.

I really recommend that you look at my full GitHub repo of the project! You can find the link here: https://github.com/redy9567/PortfolioProject3

Reflecting on this project, I learned a lot about builds throughout the process! I also wasn't too familiar with the usefulness of hash functions. Automating the build process is a EXTREMELY important task because almost EVERY single person on a game development team contributes to the build pipeline, which is arguably the largest pipeline on any project. By optimizing this, you can save time, effort, and space for all of your projects!