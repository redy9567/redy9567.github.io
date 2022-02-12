---
layout: post
title:  "Personal Graphics Drawing Library"
summary: "When you feel like building Architexture from scratch."
author: redy9567
date: '2022-02-11 12:35:23 +0530'
category: ['jekyll','guides', 'sample_category']
tags: jekyll
thumbnail: /assets/img/posts/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes, multi categories and tags
usemathjax: false
permalink: /blog/project-one-postmortem/
---

## Personal Graphics Drawing Postmortem

1. How do you feel about this project overall?

Reflecting upon this project, I feel mostly positive about the outcome. I hit many different points of growth. I learned how to navigate reading documentation to not only learn its content but also to adapt it to other situations. This was achieved when I had to adapt opening a window from an application-based environment to a console-based one. I also learned how to use Windows native API to create a graphical environment. A negative about this project was that I spend a lot of time sifting through convoluted code. I realized that the Win32 API that I was utilizing was not optimized very well for the purpose that I was intending for it. This was evident as I tried to draw bitmaps, as the API used a different API for loading a Bitmap from a file. That being said, I believe that the most important aspect of a project is how much you learned from the experience. Personally, I think I learned a lot, and I'm excited to tackle a new project next week!

2. Did your project achieve its stated intention (it's "why")? Did your "What" align with your "Why?"

I believe that my project achieved it's stated intention. My goal for the project was to learn how to use Windows API to create the start of a Game Graphics Library. The purpose for this was to have lower-level access to the graphics library code. I love lower-level code, so it was a great experience for me to learn a litte about what is happening behind the scenes.

3. Are there other project ideas that your work on this project inspired?

Yes! This project inspired me to look into OpenGL, which I have suspicions that it might be easier to create a Game Graphics Library using that API. I suspect that OpenGL would be much more tailored to the original goal that I had in mind.

4. Did your project change significantly over time? Do you feel you adapted to these changes well?

I don't think my project changed too heavily. If anything, I think that it took me longer than I anticipated to implement features such as bitmaps, that I didn't get into learning other areas of the Windows API that I wanted to learn, such as an input system. I feel like I adapted very well because I scaled the project down to fit the time scale that I had left. I knew going into it that input was a low priority, and I was able to adjust accordingly. 