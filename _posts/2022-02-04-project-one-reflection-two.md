---
layout: post
title:  "Personal Graphics Drawing Library Reflection 2"
summary: "Reflecting upon my workflow"
author: redy9567
date: '2022-02-04 12:35:23 +0530'
category: ['jekyll','guides', 'sample_category']
tags: jekyll
thumbnail: /assets/img/posts/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes, multi categories and tags
usemathjax: false
permalink: /blog/project-one-reflection-two/
---

## Project 1 Reflection 2

1. What did you accomplish?

For the second week, I started by implementing a proper Window Message handling loop. Through this process, I was able to get the window to properly close. Upon close, a sub-window opens asking to confirm your choice to close, which functions as intended. Then, I tackled abstracting the window creation into a class called GraphicsSystem. Throughout this process, I realized that it would be beneficial to edit my code to use a Console entry point rather than an Application entry point. So I reworked my code to function with a console. Following that I researched into how to draw with the library using Direct2D. I then was able to draw a circle on the screen upon startup.

2. What difficulties did you face? How did you overcome them?

When I started abstracting the GraphicsSystem class, I realize that it would make much more sense to have a console entry point. I initially didn't implement this because the examples presented on the Windows API documentation used the Application entry point, which inherently passed some of the information needed to open a window. I, therefore, had to tweak my program a bit to figure out how to get the application handle and the other information needed to open a window with a console based program. Another obstacle that I faced was when trying to create the GraphicsSystem class. One of the functions imperative to opening a window required that you passed your window messaging handler function into it. Making this handler function private, however, would not allow the linker to find the reference. My solution was to make this function static so that the reference could always be found, but that created a new problem. Because static function can only call other static functions, my handler function now wanted any other function in my class to be static which isn't ideal. My solution to this was to make the GraphicsSystem a Singleton, which always allowed the static message handling function able to find the reference for the other functions that it needed to call.

3. What do you plan to do for next week?

For next week, I hope to fully abstract the code for a window, along with abstracting the code to make bitmaps and draw them to the window. A stretch goal of mine would be to mess around with the API for input controls.

4. Did your goals for the project change? If so, how?

My goals for the project no longer include having input, but I'm still happy with just being able to draw bitmaps from their classes.