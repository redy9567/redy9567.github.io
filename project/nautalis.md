---
layout: page
title: Project Nautilus
---

## Project Nautilus

<img src="{{site.url}}{{site.baseurl}}/assets/img/project/nautalis/PlacingBeacon.gif" width="600" height="450">

Project Nautilus is a slow paced, exploration focused, 3D first person action game where the player takes the role of an underwater drone operator hired by the Isthmus Corporation to expand the company's autonomous underwater drone network. The player must remotely pilot a drone through a labyrinthian set of underwater caves, installing wireless signal extenders and eliminating any threat to those signal extenders. As the pilot dives deeper, slowly they begin to realize that these caves are far older and more alive than their employer is trying to convince them.

Project Nautilus is the first project that I have been able to explore Tools & Pipeline Programming. With a team of 16+ people, we have a lot more manpower, and more ability to specialize into different areas. For me, this was an opportunity to explore the power that Unity offers you within Tools Development.  After exploring a few ideas for productive tools, the most pressing obstacle was an issue brought up to me from one of our artists, Conrad. He explained to me that  in previous development, populating each rock into our cave by hand took over eight hours to do, which sounded mind numbingly painful. He explained to me that he has tried to use the Unity tool PolyBrush to make it easier, however he explained to me that the tool is world-based and not camera-based, which means that he couldn’t paint inside the caves, it would sometimes only paint on top of the caves, which would be on the outside. Hearing this issue, it sounded like one that I knew exactly how to fix programmatically. I didn’t quite know how to create a painting tool, but I felt more comfortable creating a Cave Population Tool that would take the mesh data of the cave, and randomly spawn rocks upon the floor of that cave. You can read more about my <a href="{{site.url}}{{site.baseurl}}/toolProjects/population/index.html">Cave Population Tool</a>. 

Creating this tool has allowed me to learn so much about how much control Unity actually gives their developers for Tool Development. Throughout making my Cave Population Tool, I was able to learn so much about how to run certain pieces of code in engine rather than in game. With my new knowledge, I decided to take my hand at creating an Object Painter Tool, similar to PolyBrush, but much simplier along with camera-based placement. After tasking myself with this, I was able to get a working prototype of it within a week! Offering both of these tools to Conrad, I am confident that he will have a much smoother time populating the different areas that we want to develop within our game. By taking hours off of his workload in these tasks, he is able to focus on creating new assets and art development. You can read more about my <a href="{{site.url}}{{site.baseurl}}/toolProjects/painter/index.html">Object Painter Tool</a>.

Recently, I have had the oportunity to dive into the Pipeline part of my title. It was brought to my attention that Sound Implementation for our game was struggling. Looking into this, I found that these issues wasn't because Sound wasn't being developed, but rather than the rest of the team didn't know how to take the assets and properly put them to where they need to be in game. Knowing this, I immediately dove into research into how the raw Sound Implementation functioned, and then proceeded to create a script that will allow any programmer to trigger sounds from anywhere else in the game. After creating this infrastructure, I then developed a Sound Pipeline Document to share with the rest of the team. This document contains a lot of clarity of how our Sound Designers should pass sound assets to our programmers, and how our programmers can utilize them after being received.

This game is currently still in development! And I will update this page as necessary!

You can find our itch.io link to try out a demo for yourself <a href="https://studioisthmus.itch.io/project-nautilus">here</a>!

Here is a video that shows off our gameplay:

<figure class="video_container">
<video width="800" height="600" controls="true" allowfullscreen="true">
  <source src="{{site.url}}{{site.baseurl}}/assets/img/project/nautalis/demoVideo.mp4" type="video/mp4">
  <source src="{{site.url}}{{site.baseurl}}/assets/img/project/nautalis/demoVideo.ogg" type="video/ogg">
Your browser does not support the video tag.
</video>
</figure>


