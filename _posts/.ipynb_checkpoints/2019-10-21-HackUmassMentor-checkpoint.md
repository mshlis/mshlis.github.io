---
author: Michael Shliselberg
author_profile: true
date: 2019-10-21
tags: [hackathon, personal]
comments: true
excerpt: "This past weekend I had the pleasure of being a mentor at Hack Umass. In this post I discuss how the experience was along with common debugging ideas that are helpful in a variety of scenarios"
title: "(Pers) Hack Umass Mentor"
---

This past weekend I had the pleasure of being a mentor at Hack Umass. Going to Umass as an ECE student, I saw the development of this event from a small hardware-only hackathon to what it is now. Now every year, me and a few friends drive up to the event to see the cool hacks and reconnect. This year though was my first as being a mentor  

<p align="center">
  <img src="/images/HUM/nutshell.jpg">
  <br><b>Hack Umass in a nutshell</b>
</p>  

#### What is a hackathon?  
Most reading this probably already know what a hackathon is but just in case I will reverberate it. Hackathons are events where people come together and compete in a fixed time interval to "hack" together a cool project! Hackathons can be themed or have some form of goal statement. Examples can include Healthcare, AI, Blockchain, or any buzzword of choice... or they can just be open ended (Hack Umass fits in this category). Its a great time, brainstorming, coding, food, stress, energy drinks and a lack of sleep. Its a haven.       


#### Hack Umass
I was a mentor. This just includes being available for groups that have questions or need any other form of help. The group of mentors included a diverse set of volunteers with skills spanning a variety of realms of hardware and software.  
Unfortunately there was over 850 participants and not nearly as many mentors. So sometimes you're left with groups requiring assistance in a domain you dont know much about. This is what I want to focus on.  

Over 50% of the questions I ended up with regarded microcontrollers. During my degree in computer engineering I was no stranger to working with microcontrollers but its been a while and it definitely was not my expertise. **So how can I help a group of students if I dont know the answer myself?** Luckily accross many domains alot of the same debugging procedures apply. These include the usual suspects  

 * Is it software? You have a debugger?
     - No debugger means use print statements (or get a debugger if possible) 
     - Sometimes print statements may just be easier (I know this is sacriledge to say, but its case to case in my opinion) 
     - Check versioning 
     - Test components seperately if possible with drivers/stubs (this may be expensive given the time frame, but sometimes you can build these as you go)
  
  
 * Google the error
     - Include errors that link to the exernal issues
     - Include version / meta info depending on the error  
     - Use possible solutions / tutorials found   


 * When using a tutorial   
     - Make sure anything you do is reversable! (because it worked for someone else doesnt mean it will for you, and you maybe could make it worse!)  
     - If one tutorial does not solve it for you, undo the changes before finding another -- sometimes solutions conflict  
     
     
 * Could it be the hardware?  
     - Noone wants to blame the hardware, seems like a cowardly scapegoat, but sometimes it really is!
     - Check components one by one if possible (drivers/stubs, voltmeter, ampmeter, replace the part, etc..)  
     
These are just a handful of paths, but alot of the time its adaptive. Errors can by tricky and sometimes multi faceted, so it is case by case but alot of the time this pseudo-road map will get you far.   

Also as a mentor, sometimes adding a new perspective adds something. Im sure everyone has done this: you begin a group project and everyone has their own ideas, but eventually you converge on a concept and a path but once there, you all are so focused on that, that you dont pay attention to some obvious mistakes/solutions-- same is true for any form of group based problem solving.  

Overall it was a great weekend. I got a chance to goto pita pocket (best restaurant in all of amherst with greatest falafel on this side of the planet) and saw my friends while meeting a ton of awesome people. Seeing so many people working on such amazing projects is always inspiring and always gets you revved up! It was a wonderful experience and I hope to be there again next year.