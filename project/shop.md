---
layout: page
title: Hold Down the Shop
---

## Hold Down the Shop

<img src="{{site.url}}{{site.baseurl}}/assets/img/project/shop/shopGif.gif" width="600" height="450">

Hold Down The Shop is a 2 player co-op experience where the players control either the apothecary or the apprentice. The apothecary is in the field communicating with customers and gathering ingredients. The apprentice is holding down the shop and taking orders from the apothecary. Together they must communicate and platform around to create potions for customers. 

As the apothecary, the player’s duties include going to customer’s houses to find out what they need, describing the required ingredients to the apprentice, and scavenging around the area for any natural ingredients that are found in the torn pages of the potion book that stayed in the shop with the apprentice. 

The apprentice will be located in the shop itself, and they must find the ingredients the apothecary needs, tell the apothecary what natural ingredient is needed (through the use of torn pages from the potion book), and brew the final potion to send to the apothecary. 

Since the players will be trying to serve customers, they will be on a time limit to make the potions a good quality. The longer they take the lower the quality, which means it is less helpful to the people in need. Players will need to communicate effectively with each other in order to make potions quickly. Any wrong ingredients in the potion and the portal can cause it to become unstable, resulting in it blowing up in the faces of both players!

Team Members:

Alex Barnett - Producer  
Blade Osborn - Animator  
Amber Williamson - Environment Artist  
Jen Loefstedt - Prop Artist  
Christian Roby - Level Designer  
Carter Ivancic - UI/UX Designer  
Olli Machina - Gameplay Programmer  
Brandon L'Abbe - Network Programmer  

My role on the team was the purely focused on Networking. In the early stages of the project, I developed multiple networked prototypes in the Unreal 4 engine. I first started off with a LAN prototype, then later developed a prototype that supported a dedicated server model. Throughout the project, I constantly worked with my co-programmer and the designers to test and implement new gameplay features in our networked prototype. This was challenging because we had to ensure that all data was properly replicated between both clients and the server. I also implemented the voice chat that we used to allow the two player to communicate. For this we implemented Vivox with our Unreal prototype. By the end of the project, we had a networked game that allowed two players to complete the basic game loop that we outlined in our design documents.

Throughout this project, I experienced many obstacles. For one, I have never worked on a networked game, nor have I ever worked in the Unreal engine. Throughout the few months that I worked on the project, I had to research how the engine functioned as well as how to develop an efficient server-client model. This at times proved challenging, as I would struggle to debug data that hasn't been properly replicated. This often resulted in the two players playing in unsynchronized games. One player would make the potion and send it through the portal, and the other player's game never knew it happened. I sought to overcome this by utilizing different resources, such as reaching out to professionals in the industry, as well as extensive research on networking and replication. At the end of our project, we got a single-player prototype. However, our game still didn't fluidly work with two players. 

Looking back, there's a lot that I would do differently about this project. First off, I would've taken a more network-first approach. Throughout our project, my co-programmer and I spent a lot of time working on separate game and networking prototypes, only to try to collide the two together late in the project. This led to us not properly utilizing AGILE development, resulting in a lot of bugs and issues at the very last minute. Instead, a better approach would be to have one prototype constantly in development. One programmer will implement a gameplay feature, while the other works on replicating this gameplay feature over a network. If we had iteratively applied this process throughout the project's lifespan, we would have a more well-polished game as our product.

Here is a video that shows off our game:

<figure class="video_container">
<video width="600" height="450" controls="true" allowfullscreen="true">
  <source src="{{site.url}}{{site.baseurl}}/assets/img/project/shop/hdts.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>
</figure>


