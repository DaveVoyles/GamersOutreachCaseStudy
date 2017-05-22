---
layout: post
title:  "Children's hospitals, charity, and gaming connect with Azure App Services"
author: "Dave Voyles"
author-link: "http://www.twitter.com/DaveVoyles"
#author-image: "{{ site.baseurl }}/images/authors/photo.jpg"
date:   2017-04-04
categories: [Azure App Service]
color: "blue"
#image: "{{ site.baseurl }}/images/imagename.png" #should be ~350px tall
excerpt: "Children's hospitals, charity, and gaming connect with Azure App Services"
language: The language of the article (e.g.: [English])
verticals: The vertical markets this article has focus on (e.g.: [Other])
---

## Volunteer web portal with Gamers Outreach ##

A team from Microsoft and Gamers Outreach set out to create a web interface to improve how Gamers Outreach connects volunteers to opportunities in children's hospitals throughout the US.  

This engagement illustrated how easy it was to deliver an ASP.NET core solution and host it as an Azure App Service. This included:

- Volunteer portal to build their profile including interests, time availability and skills.
- Staff portal to: 
	- Input location and track carts (portable displays with gaming devices).
	- Locate and connect with volunteers that are close to opportunities in select cities.
	- Use the number of volunteers in an area to decide on new locations.

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

- [Zach Wigal](http://wwww.twitter.com/ZachWigal) - Founder, Gamers Outreach
- Shaun Fyall - Web Developer, Gamers Outreach

## Customer profile ##

Los Angeles based [Gamers Outreach](http://www.gamersoutreach.com) was founded in 2007, and is a 501(c)(3) charity organization that provides equipment, technology, and software to help kids cope with treatment inside hospitals. Hospitals can often be a lonely, isolating, and scary places for young people. Gamers Outreach programs ease those burdens by providing equipment, technology, and software to help kids cope with long-term treatment.

Founder Zach Wigal is an [Xbox MVP](https://mvp.xbox.com/home) and recent recipient of the [Forbes 30 under 30](https://www.forbes.com/profile/zach-wigal/) award for his work with Gamers Outreach.

In 2009, Gamers Outreach began working with C.S. Mott Children’s Hospital of Ann Arbor, Michigan to provide video games to hospitalized children. Witnessing the frequent need for bedside activities, as well as the impact games were making in the medical environment, everyone quickly realized Gamers Outreach could provide something more specific: accessible recreation for kids during long-term stays.

After spending six months volunteering within the hospital, the first “GO Kart” was created – a portable, medical-grade video game kiosk that provided nurses and child life specialists with a way to transport games and entertainment to children who were unable to leave their rooms within the hospital.

[Hopital Visit](/images/gamersoutreach/go-hospital-visit.jpg)
 
## Problem statement ##

Gamers Outreach was having volunteers fill out a Google Form which saved their data to a spreadsheet. Once the form was filled out, if their availability or contact information changed, they would have to fill out a new form. There was no other way to update volunteer information.

![Google Form](/images/gamersoutreach/go-volunteer-googleform.png)

when a Kart arrived, Gamers Outreach staff would have to sift through all of the forms and the collected data to determine which volunteers would be the best match in a given region to reach out to patients and their families. This was arduous and challenging because each address had to be manually reviewed by a staff member.

>	*"Over the years, we've had a number of gaming enthusiasts from around the country express interest in supporting our efforts. However, communicating volunteer opportunities, and subsequently managing volunteerism in multiple regions, is difficult to scale. At the moment, we aggregate all our volunteer data through simple excel spreadsheets that are shared with our team members. However, whenever we actually want to activate our volunteer network in a particular region - someone has to manually scrub / localize the data, and then issue messaging to a select group of people. As you can imagine - this could be very difficult to scale. Imagine if we had 10,000 or 100,000 people sign up to volunteer. It's impractical to manually find people living in towns throughout the radius of a major city. This is a problem I believe could easily be solved by with a web application."*  - **Zach Wigal - Founder, Gamers Outreach**

**Project scope**

The project goal was to replace the static Google Form with an easy to extend web application that would not only collect the volunteer information, but also manage and visualize the data in order to improve Gamers Outreach staff processes.

Furthermore, Gamers Outreach is largely organized by volunteer services, so technical projects may change hands often. With that in mind, they needed a solution that could allow developers from anywhere in the world to quickly make changes to the web app, immediately scale the application to meet demand, and of course back have rednundacy in case the code every got wiped when changings hands. 

## Solution, steps, and delivery ##
The team used the existing form as the basis for the volunteer information. This new way of managing profiles allows the volunteer to update their contact information or change their availability once their information was stored. 

They also decided to use a DocumentDB database for storing the data.  The data schema would be flexible so the volunteer developers could easily extend what data is collected without having to do complex database updates.

When a user submits their profile data, their address is automatically geolocated using the Bing Maps API and updated in the database. The volunteer lists can be pulled using the native GeoJSON support in DocumentDB by doing a simple spatial query on the data.
```
 client.CreateDocumentQuery<MyDocument>(collection.SelfLink)
    .Where(x => x.Location.Distance(cartLocation) < distance))
```
![App Service Architecture Diagram](/images/gamersoutreach/go-webapp-arch.png)

*App Service Architecture Diagram*

Using the Bing Maps API, the volunteer data is then overlayed on a map pinpointing the location and showing nearby volunteers.

![Volunteer Location Map](/images/gamersoutreach/location-map.png)

*Volunteer Location Map*

The team created the solution with ASP.NET MVC and DocumentDB to create a small and fast website for collecting the volunteer information. Work items were created and tracked in Visual Studio Team System (VSTS). As features were completed and checked into the Git source repository, VSTS automatically built and deployed the latest version of the code to the Azure App Service. 

The website will be maintained by volunteer staff for Gamers Outreach so we automated as much of the build and deploy processas possible to minimize the steps required to update site content. 


*Redundancy* 

One benefit of App Services is the ability to backup not only the website's codebase, but also that of the databases the code relies on. The Back up and Restore feature in Azure App Service lets you easily create app backups manually or on a schedule. You can restore the app to a snapshot of a previous state by overwriting the existing app or restoring to another app. In total, we are backing up:

- App configuration
- File content
- Database connected to your app

[The documentation on the App Services website](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-backup) illustrates how to do this as well.


*Scaling*

To ensure a consistent experience regardless of the time of day or how many users are entering their info into our volunteer database, we needed to take advantage of App Service scaling. When it comes to scaling, there are two workflows for scaling, scale up and scale out. Because the Gamers Outreach website is not running and computationally intensive applications, there wasn't a need to scale up, but as traffic increases on the site, especially when the volume of volunteers grows, the need to scale out will soon come into play. 

Scaling out increases the number of Virtual Machine (VM) instances that run the Gamers Outreach app. You can scale out to as many as 20 instances, depending on your pricing tier. App Service Environments in Premium tier will further increase your scale-out count to 50 instances, which is more than would be need3ed here. Morevoer, we've enabled autoscaling, which is to scale instance count automatically based on predefined rules and schedules. This could be anything from CPU utilization, Disc Queue Length, etc.

We are scaling through the Azure portal, but you can also use the [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) or [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) to adjust scale manually or automatically.


*Continuous Deployment*

 App Service has built-in integration with a number of services, including BitBucket, GitHub, and Visual Studio Team Services (VSTS), which enables a continuous deployment workflow where Azure pulls in the most recent updates from your project published to one of these services. Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated, and considering this project will continue to be worked on by numerous volunteers overtime, it made for a perfect use case. 

 For Gamers Outreach, all of their code will be stored on GitHub, and any changes to the master branch of their repository will instantly be visible on the production website. Alternatively, the team could also create a staging branch, which allows for updates to be tested out before they go live. When they are satisfied with their work, they can set up [staging environments](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-staged-publishing), which allows for swapping between the two branches on the fly. 


## Conclusion ##

**Measurable impact/benefits**

With this ASP.NET solution, volunteers can quickly build a profile to detail their skills and availability. The Gamers Outreach staff can easily identify volunteers that are near any of the locations where carts are deployed. When reviewing the final website, CEO Zach Wigal said:

>"This will be incredibly helpful for us, especially when it comes time to scaling. We've been overwhelmed with having to manage this all by hand from an excel spreadsheet, so this will save us a huge amount of time."

The site is set up with continuous deployment, so when volunteer developers make changes, these can be easily deployed to the production website. Moreover, with scaling configured, the application can build out as more volunteers and staff continue to enter and access the volunteer portal 

**Going Forward**

There are a number of opportunities to build on this project. 

- The project takes locations and finds nearby volunteers. Future features include mapping all volunteers to find if there are new opportunities to place carts in areas with a number of volunteers.

- The project scope was to identify and list the volunteers that are near a location. In the future, Gamers Outreach could simplify the contact process by sending an e-mail or SMS text messages to volunteers when a GO Kart arrives in their region.

- GO Karts are currently set up and deployed by Gamers Outreach staff. Since this website also tracks where carts  are deployed by location, tickets could be added to track cart maintenance and notify nearby volunteers that have particular installing and troubleshooting skills.

Zack is already thinking of ways this new website can be used to add gamification for badged volunteers, and to track volunteer hours.


## Additional resources ##


### Documentation ###
- [Gamers Outreach Code](http://github.com/...)
- [Azure App Service Docs](https://docs.microsoft.com/en-us/azure/app-service/)
- [Intro to ASP.NET Core Docs](https://docs.microsoft.com/en-us/aspnet/core/)
- [App Services Backup Docs](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-backup)
- [Scaling in Azure Docs](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/insights-how-to-scale)
- [Continuous deployment App Service Docs](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-continuous-deployment)
- [Staging Environemnt App Service Docs](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-staged-publishing)
