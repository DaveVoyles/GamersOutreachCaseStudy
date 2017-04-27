---
layout: post
title:  "Volunteer web portal with Gamers Outreach"
author: "Dave Voyles"
author-link: "http://www.twitter.com/DaveVoyles"
#author-image: "{{ site.baseurl }}/images/authors/photo.jpg"
date:   2017-04-04
categories: [Azure App Service]
color: "blue"
#image: "{{ site.baseurl }}/images/imagename.png" #should be ~350px tall
excerpt: With ASP.NET core on Microsoft Azure, we can quickly spin up a website and backend for charity volunteers to create a profile, and the charity to easily
reach out to volunteers when their assistance is needed."
language: The language of the article (e.g.: [English])
verticals: The vertical markets this article has focus on (e.g.: [Other])
---

## Volunteer web portal with Gamers Outreach ##

A team from Microsoft and Gamers Outreach set out to create a web portal that would improve the process of Gamers Outreach in connecting volunteers to opportunities in specific locations aross the US.  

This engagement was executed to highlight how easy it was to deliver an ASP.NET core solution and host it as an Azure App Service. This consists of several parts, notably:

- Portal for volunteers to build a profile including interests, time availability and skills.
- Ability for staff to input location and track carts.
- Ability for staff to locate and connect with volunteers that are close to opportunities in select cities.
- Ability for staff to use the number of volunteers in an area to decide on new locations.

# Solution overview with technologies leveraged:
- Azure App Service
- ASP.NET core
- Azure DocumentDB
- Visual Studio 2015
- Visual Studio Team Services

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

![Hopital Visit](/images/gamersoutreach/go-hospital-visit.jpg)
 
## Problem statement ##

Gamer's Outreach was having volunteers fill out a Google Form which would save the data to a spreadsheet. Once the form is filled out, if their availability or contact information changes, they would have to refill out the form.

![Google Form](/images/gamersoutreach/go-volunteer-googleform.png)

Later, Gamer's Outreach staff would perform the arduous task of sifting through all of the collected data to determine who would be the best volunteers to reach out to in a region when a Kart arrived. Finding volunteers in a given region was challenging because each address had to be reviewed by a staff member.

>	*"Over the years, we've had a number of gaming enthusiasts from around the country express interest in supporting our efforts. However, communicating volunteer opportunities, and subsequently managing volunteerism in multiple regions, is difficult to scale. At the moment, we aggregate all our volunteer data through simple excel spreadsheets that are shared with our team members. However, whenever we actually want to activate our volunteer network in a particular region - someone has to manually scrub / localize the data, and then issue messaging to a select group of people. As you can imagine - this could be very difficult to scale. Imagine if we had 10,000 or 100,000 people sign up to volunteer. It's impractical to manually find people living in towns throughout the radius of a major city. This is a problem I believe could easily be solved by with a web application."*  - **Zach Wigal - Founder, Gamer's Outreach**

**Project scope**

The project goal was to replace the static Google Form with an easy to extend web application that would not only collect the volunteer information, but also manage and visualize the data in order to improve Gamer's Outreach staff processes.

## Solution, steps, and delivery ##
The team used the existing form as the basis for the information that is collected from the volunteers. This new way of managing profiles allows the user to update their contact information or change their availability after the initial contact. 

They also decided to use a DocumentDB database for storing the data so that data schema would be flexible for the volunteer developers to easily extend what data is collected in the future without having to do complex database updates.

![App Service Architecture Diagram](/images/gamersoutreach/go-webapp-arch.png)

*App Service Architecture Diagram*

When a user submits their profile data, their address is automatically geolocated using the Bing Maps API and updated in the database. The volunteer lists can be pulled using the native GeoJSON support in DocumentDB by doing a simple spatial query on the data.
```
 client.CreateDocumentQuery<MyDocument>(collection.SelfLink)
    .Where(x => x.Location.Distance(cartLocation) < distance))
```

Using the Bing Maps API, the volunteer data is then overlayed on a map pinpointing the location and showing nearby volunteers.

![Volunteer Location Map](/images/gamersoutreach/location-map.png)

*Volunteer Location Map*

The team created the solution with ASP.NET MVC and DocumentDB to create a small and fast website for collecting the volunteer information. Work items were created and tracked in Visual Studio Team System (VSTS). As features were completed and checked into the Git source repository, VSTS automatically built and deployed the latest version of the code to the Azure App Service. The website will be maintained by volunteer staff for Gamer's Outreach so it was important to make the build and deploy process as automated as possible to minimize the steps required to update content on the site. 

## Conclusion ##

**Measurable impact/benefits**

With this ASP.NET solution, volunteers can quickly build and update a profile to detail their availablity as well as how they can assist. Additonally, the Gamer's Outreach staff can easily identify volunteers that are near any of the locations where carts are deployed. When reviewing the final website, CEO Zach Wigal said:

>"This will be incredibly helpful for us, especially when it comes time to scaling. We've been overwhelmed with having to manage this all by hand from an excel spreadsheet, so this will save us a huge amount of time."

The site is set up with continuous deployment, so when volunteer developers make changes, these can be easily deployed to the production website.

**Going Forward**

There are a number of opportunities to build on this project. Currently, the project takes locations and finds nearby volunteers. Future features include mapping all volunteers to find if there are new opportunities to place carts in areas with a number of volunteers.

The current scope of the project was to identify and list the volunteers that are near a location. In the future, sending an e-mail or SMS text messages to volunteers when a GO Kart arrives in their region could simplify the contact process.

GoKarts are currently set up and deployed by Gamer's Outreach staff. Since this website also tracks carts deployed at specific locations, tickets could be added to track cart maintenance and notify nearby volunteers that have particular skills with installing and troubleshooting issues.

Zack is already thinking of ways that this new website can be a way for him to add gamification for the volunteers with badges and volunteer hours tracking.


## Additional resources ##


### Documentation ###
- [Gamers Outreach Code](http://github.com/...)
- [Azure App Service Docs](https://docs.microsoft.com/en-us/azure/app-service/)
- [Intro to ASP.NET Core Docs](https://docs.microsoft.com/en-us/aspnet/core/)