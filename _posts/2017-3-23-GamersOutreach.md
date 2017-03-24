---
layout: post
title:  "Volunteer portal with Gamers Outreach"
author: "Dave Voyles"
author-link: "http://www.twitter.com/DaveVoyles"
#author-image: "{{ site.baseurl }}/images/authors/photo.jpg"
date:   2016-09-23
categories: [Azure App Service]
color: "blue"
#image: "{{ site.baseurl }}/images/imagename.png" #should be ~350px tall
excerpt: With ASP.NET core on Microsoft Azure, we can quickly spin up a website and backend for charity volunteers to create a profile, and the charity to easily
reach out to volunteers when their assistance is needed."
language: The language of the article (e.g.: [English])
verticals: The vertical markets this article has focus on (e.g.: [Other])
---

Begin with an intro statement with the following details:

TODO:
- Solution overview

A team from Microsoft and Gamers Outreach set out to create a web portal that would improve the process of Gamers Outreach to request help from volunteers in specific regions aross the US.  

 This engagement was executed to highlight how easy it was to deliver an ASP.NET core solution and host it as an Azure App Service. This consists of several parts, notably:

 - Portal for volunteers to build a profile
 - Page for search through profiles and reach out to volunteers in select cities



# Solution overview with technologies leveraged:
- Visual Studio 2015
- Azure App Service
- ASP.NET core
- SQL database


# Core Project Team

- [Brian Sherwin](http://www.twitter.com/Bsherwin) – Technical Evangelist, Microsoft
- [Dave Voyles](http://www.twitter.com/DaveVoyles) – Technical Evangelist, Microsoft
- [Justine Cocchi](http://www.twitter.com/justinecocchi) – Technical Evangelist, Microsoft
- [Adam Tuliper](http://www.twitter.com/AdamTuliper) – Technical Evangelist, Microsoft


**Other contributors:**

- [Zach Wigal](http://wwww.twitter.com/ZachWigal) - Founder, Gamer's Outreach
- Shaun Fyall - Web Developer, Gamer's Outreach

## Customer profile ##

The Los Angeles based [Gamer's Outreach](http://www.gamersoutreach.com) was founded in 2007, and is a 501(c)(3) charity organization that provides equipment, technology, and software to help kids cope with treatment inside hospitals. Hospitalization can often be a lonely, isolating, and scary experience for young people. Gamers Outreach programs ease those burdens by providing equipment, technology, and software to help kids cope with long-term treatment.

Founder Zach Wigal is an [Xbox MVP](https://mvp.xbox.com/home) and recent recipient of the [Forbes 30 under 30](https://www.forbes.com/profile/zach-wigal/) award for his work with Gamer's Outreach.  

In 2009, Gamers Outreach began working with C.S. Mott Children’s Hospital of Ann Arbor, Michigan to provide video games to hospitalized children. Witnessing the frequent need for bedside activities, as well as the impact games were making in the medical environment, it was quickly realized Gamers Outreach could provide something more specific: accessible recreation for kids during long-term stays.

After spending six months volunteering within the hospital, the first “GO Kart” was created – a portable, medical-grade video game kiosk that provided nurses and child life specialists with a way to transport games and entertainment to children who were unable to leave their rooms within the hospital.

[Hopital Visit]({{ site.baseurl }}/images/gamersoutreach/go-hospital-visit.jpg)
 
## Problem statement ##

This section will define the problem(s)/challenges that the customer wants to address with an Azure App Service solution. Include things like costs, customer experience, etc.
 

>	*"Over the years, we've had a number of gaming enthusiasts from around the country express interest in supporting our efforts. However, communicating volunteer opportunities, and subsequently managing volunteerism in multiple regions, is difficult to scale. At the moment, we aggregate all our volunteer data through simple excel spreadsheets that are shared with our team members. However, whenever we actually want to activate our volunteer network in a particular region - someone has to manually scrub / localize the data, and then issue messaging to a select group of people. As you can imagine - this could be very difficult to scale. Imagine if we had 10,000 or 100,000 people sign up to volunteer. It's impractical to manually find people living in towns throughout the radius of a major city. This is a problem I believe could easily be solved by with a web application."*  - **Zach Wigal - Founder, Gamer's Outreach**




TODO:
## Solution, steps, and delivery ##
- What was worked on and what problem it helped solve.
- Description of the web app you built and what it did. What was the web app function? How was it different and compelling? How did it help you do more than using something else?
- Technical details of how this was implemented.
- Pointers to references or documentation.
- Learnings from the Microsoft team and the customer team.
- Architecture diagram/s (**required**). Example below:

 ![App Service Architecture Diagram](/images/templates/appservicearchitecture.png)



**Project scope**
TODO:

Gamer's Outreach was previously having volunteers fill out a Google Form which would save the data to an excel spreadsheet. They would then perform the arduous task of sifting through all of the data to determine who would be the best volunteers to reach out to in a region when a Kart arrived. 

[Google Form]({{ site.baseurl }}/images/gamersoutreach/go-volunteer-googleform.png)

Together, we set out to aimed to simplify this process with a web portal to ingest volunteer profiles and quickly sift through them. 

**Azure App Service**
TODO:




TODO:
## Conclusion ##

**Measurable impact/benefits**

With this ASP.NET solution, volunteers can quickly build a profile to detail their availablity as well as how they can assist. Additonally, the Gamer's Outreach staff can easily contact volunteers 


TODO:
- General lessons:
  - Insights the team came away with.
  - What can be applied or reused for other environments or customers.

**Going Forward**

There are a number of opportunities to build on this project. They include:

- Map to display the location of each volunteer
- Automatied e-mail and SMS text to volunteers when a GO Kart arrives in their region


## Additional resources ##


### Documentation ###
- [Azure App Service Docs](https://docs.microsoft.com/en-us/azure/app-service/)
- [Intro to ASP.NET Core Docs](https://docs.microsoft.com/en-us/aspnet/core/)