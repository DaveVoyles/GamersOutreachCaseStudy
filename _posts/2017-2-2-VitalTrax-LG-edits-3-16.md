---
layout: post
title:  Xamarin.Forms application, "Wing Enterprise"
author: DaveVoyles
author-link: http://www.DaveVoyles.com
author-image: {{ site.baseurl }}/images/davevoyles-headshot.jpg
date:   2017-01-20
categories: [Mobile Application Development with Xamarin]
color: "blue"
image: {{ site.baseurl }}/images/screens/vitaltrax-logo.png
excerpt: VitalTrax has delivered a platform for healthcare administrators to offer clinical trials in a modern way.
language:  [English]
verticals: [Healthcare]
---
Microsoft Technical Evangelists and VitalTrax, a healthcare startup based out of Philadelphia worked remotely and on-site to build a mobile application 
and improved their DevOps experience. 

For the mobile solution, Xamarin.Forms (a cross-platform C# platform) was used to create applications 
on iOS, Android, and Windows, with most of the codebase living within a single project. 

We also walked the VitalTrax leadership through a Value Stream Mapping (VSM) process, to map out
their existing software lifecycle so they could better understand their process as well 
as apply DevOps best practices for process improvement. 
 

  ![Dev Ops Diagram]({{site.baseurl}}/screens/VitalTrax-VSM.jpg) 

**Key technologies used:**  

* Xamarin
* Visual Studio Team Services
* HockeyApp
* Azure App Services
* Powershell


 **Core Team:**

  * [Andy Reitano](http:/www.twitter.com/BatslyAdams) - Technical Evangelist, MSFT
  * [Adina Shanholtz](http:/www.twitter.com/FeyTechnologist) - Technical Evangelist, MSFT
  * [Jerry Nixon](http:/www.twitter.com/JerryNixon) - Technical Evangelist, MSFT
  * [Kevin Remde](http:/www.twitter.com/kevinremde) - Technical Evangelist, MSFT
  * [Dave Voyles](http:/www.twitter.com/DaveVoyles) - Technical Evangelist, MSFT
  * Todd Kueny - CTO, VitalTrax
  * Penn Keuny - Product Designer, VitalTrax
  * Michael COllier - Cloud Infrastructure Engineer, VitalTrax

 
## Customer profile ##
- [VitalTrax](http://vitaltrax.net/)
- Philadelphia, PA

VitalTrax delivers a complete patient engagement solution for today's clinical trials comprising of a cloud based enterprise platform and a mobile application for patients. Their slogan and goal is Making Clinical Trials Simple for Everyone.

Patients play a very important role in clinical trials. Yet, the general public is still unsure about participating in one. 
One of VitalTrax's main goals is to increase patient awareness in clinical trials. Without patients, clinical trials do not happen.

For clinical study teams, VitalTrax created an enterprise platform to run trials efficiently.
For site teams, they've making it easier to recruit and communicate with patients during the trial.

Clinical trials can be simple and approachable. VitalTrax's passionate team works very hard to make this crucial process 
easy for everyone involved.

VitalTrax is comprised of remote and on-site employees, who work out of ic3401, a Philadelphia incubator
run by the University City Science Center, whom Microsoft has partnered with to create the [Mirosoft
Reactor](microsoftreactor.com/venue/phl-reactor/), a free community space for developers to work together. 

 
## Problem statement ##

VitalTrax had a web based solution for healthcare companies to create and administer patients
trials, but nothing on the mobile front. To differentiate their product from the rest of the market and retain a competative advantage, VitalTrax wannted to give their customers a native mobile experience. 

Healthcare companies can generate surveys and trials from their VitalTrax web portal, which was
built on Microsoft's open source .NET core. Patients log into the portal through a browser
and fill out the forms, but VitalTrax was interested in giving companies mobile web access. 

CTO Todd Kueny wanted to notify users when a new trial was available, or if the user needed to be informed
when one was nearing a completion deadline.  With push notifications, this can happen. 

A dynamic UI was necessary for each healthcare company administering trials so companies could have their unique
fonts, colors, and images. By dynamically generating markup (XAML) with C# and Xamarin, VitalTrax can do this. Simply read a JSON file from the server, parse the key/value pairs, and then--generate widgets!

 
## Solution, steps, and delivery ##

### Xamarin ###

 ![Visio Overview]({{site.baseurl}}/images/screens/VitalTrax-visio-overview.png)

Our solution had to fulfill four requirements:

1.	Push notifications to alert the patient when a trial needed to be filled
2.	Offline sync / Local storage to store data on the device when connectivity is limited
3.	Dynamically generate UI based on a JSON file from the server
4.	Allow for web-based trials to still be accessed within the application

Xamarin is a cross-platform C# framework for creating applications. It isn’t limited to
mobile devices either; it also supports creating apps for Mac as well as Universal Windows Platform (UWP) apps, which includes Xbox. 


One of Xamarin's greatest benefits is its large reusable code base.
In the past, developers created unique applications for each mobile platform, and 
wrote those apps in the platform’s native language. For iOS, it would be Objective-C or Swift,
Java or C++ for Android, and C# or C++ for Windows Mobile. 

  ![Image of solution]({{site.baseurl}}/images/screens/VT-Project.png)

Xamarin.Forms allows developers to write platform specific code for a 
project, but share most of the UI within a single set of files. Forms is a new library
that enables developers to build native UIs for iOS, Android and Windows Phone from a single, shared C# codebase.
It provides more than 40 cross-platform controls and layouts which are mapped to native controls at runtime; 
the user interfaces are fully native.

The login page we create for VitalTrax is one C# script, and will work for any of the
platforms mentioned above. For platform-distinct features, simply wrap
that code in a processor direction (#IF statement), and Xamarin.Forms will ignore it on builds for
platforms outside of the #IF statement. Here's an example: 

```C#
#if WINDOWS_UWP
        public static string HockeyAppId = "";
#elif __ANDROID__
        public static string HockeyAppId = "";
#elif __IOS__
        public static string HockeyAppId = "";
#endif
```


### WebView ###

  ![WebView]({{site.baseurl}}/images/screens/WebView.png)

Xamarin has a built-in WebView solution which allows the developer to access  application webpages by embedding the device's native browser via a Xamarin control. This keeps the user engaged within the application; they don't have to leave to complete the task, because once 
they do leave, it's difficult to get them to return. 

With WebView, the healthcare surveys VitalTrax created for the browser can be reused,
saving both time and money.

This page was generated with C# and included no XAML. When WebPageView.xaml.cs is initialized, 
the constructor begins building a Label (dubbed the header) in addition to a WebView (which has a property
titled source) and allows us to pass in the URL of a clinical trial. From there, we create buttons to navigate forward and back, and attach event handlers to perform those operations.

```C#
  Button backButton = new Button()
  {
      Text = "Back",
      HorizontalOptions = LayoutOptions.Start
  };


      backButton.Clicked += delegate
  {
          // Check to see if there is anywhere to go back to
          if (webView.CanGoBack)
          {
              webView.GoBack();
          }
          else
          { // If not, leave the view
              Navigation.PopAsync();
          }
      };
```

Finally, we create a new StackLayout, set its children property to be header, WebView, 
and buttons we created earlier, and set that as the Content element in the view (XAML). 
We now have an entire UI generated from C#.


```C#
    // Build the page and add it to the XAML
    this.Content = new StackLayout
    {
        Children =
        {
            header,
            webView,
            backButton,
            forwardButton
        }
    };
```

### Auth Page ###
  ![Auth Page Android]({{site.baseurl}}/images/screens/LandingPage-Android.png)


An alternate approach to building the UI in C# would be to use XAML and the intellisense Xamarin
offers. On pages where the experience will not change radically, or we know how many elements
we will need to draw on a page, we find that this works very well.

When users first load the VitalTrax application, they are greeted with the Landing Page, which prompts 
users to login. Once authenticated, they can begin to search for trials or continue ones already in progress. 

To handle authentication, we used RestSharp, a C# framework for performing REST operations. The healthcare 
industry has strict regulations; we can’t simply login with a user name and password. Instead,
we’ll need to make one request for an access token, which we can then pass around on subsequent requests.
This token is essentially our key for accessing patient data, such as trials and  profiles. 

We’ve illustrated how that works in [this GitHub repository.](https://github.com/DaveVoyles/Xamarin-Rusable-Components)


### Offline Sync / Local Storage ###

We've included notes within our code to clarify exactly how this works:

```C#
/*
 * Overview
 * --------
 * Offline sync allows end users to interact with a mobile app--viewing, adding, or modifying data--even
 * when there is no network connection. Changes are stored in a local database. 
 * Once the device is back online, these changes are synced with the remote service.
 * 
 * To add Offline Sync Support:
 *  1) Add the NuGet package Microsoft.Azure.Mobile.Client.SQLiteStore (and dependencies) to all client projects
 *  2) Uncomment the #define OFFLINE_SYNC_ENABLED
 *  
 * Use this package:          https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
 * Step-by-step instructions: http://go.microsoft.com/fwlink/?LinkId=620342
 * 
 * To initiate a sync in an Android or iOS app:
 *  1) pull down on the items list.
 *   
 */
#define OFFLINE_SYNC_ENABLED
        const string offlineDbPath = @"localstore.db";

        /// <summary>
        /// Creates a new local SQLite database using the MobileServiceSQLiteStore class.
        /// </summary>
        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);

#if OFFLINE_SYNC_ENABLED
            // Init local storage. Creates a table in the local store that matches the fields in the provided type. 
            var store = new MobileServiceSQLiteStore(offlineDbPath);
            store.DefineTable<TodoItem>();

            // Initializes the SyncContext using the default IMobileServiceSyncHandler.
            this.client.SyncContext.InitializeAsync(store);

            // Uses the local database for all create, read, update, and delete(CRUD) table operations.
            this.todoTable = client.GetSyncTable<TodoItem>();
#else
            this.todoTable = client.GetTable<TodoItem>();
#endif
```

When users have limited connectivity, VitalTrax still wants them to be able to fill out forms without
losing their work. With Azure Mobile Applications (formerly Services), we can create a 
backend in either NodeJS or ASP.NET to broker CRUD operations between the mobile device and our database
in the cloud. Azure Mobile Applications gives you this service out of the box. 


Simply spin up a new Mobile Application in the Azure portal, select the backend you'd like to use,
and select a database to connect to your account. The quick start button can  also generate
a mobile solution on a variety of platforms (Xamarin, iOS, Android, Cordova) for you, or provide a few 
lines of code to add to your existing application and get started immediately. 


Working in tandem with SQLite, an in-process library that implements a self-contained transactional
SQL database engine, we can temporarily store data on the device; when network connectivity resumes,
compare our local database values with those stored in the cloud, and then synchronize them. 


### Push Notifications ###
  ![Android push notification]({{site.baseurl}}/images/screens/Android-Push-Notification.png)

Push notifications can be used to alert a user of an event, whether or not the application is running.
There are a number of ways it can be implemented on each platform; we had several evangelists tackle this problem. 

We reached out to Xamarin Evangelist James Montemagno for help and learned that
since Xamarin.Forms uses Android Support Libraries 23.3.0 we were restricted to going through the standard
Google Cloud Messaging route for Xamarin on Android. You can see additional information and James' implementation in the [Xamarin Evolve 2016 app]( https://github.com/xamarinhq/app-evolve).

Here's a description of the Android Support Libraries: 

> When developing apps that support multiple API versions, you may want a standard way to provide newer features on earlier 
> versions of Android or gracefully fall back to equivalent functionality. Rather than building code to handle earlier 
> versions of the platform, you can leverage these libraries to provide that compatibility layer. In addition, the Support 
> Libraries provide additional convenience classes and features not available in the standard Framework API for easier 
> development and support across more devices.
> [Support Library Docs](https://developer.android.com/topic/libraries/support-library/index.html)


These two recently updated Xamarin docs helped us set up push 
notifications using Firebase Cloud Messaging (FCM), instead of (soon to be retired) Google 
Cloud Messaging (GCM). You can find them [here](https://developer.xamarin.com/guides/android/application_fundamentals/notifications/firebase-cloud-messaging/)
 and [here](https://developer.xamarin.com/guides/android/application_fundamentals/notifications/firebase-cloud-messaging/).

Setting up the application, we ran into some incompatibility issues between NuGet packages,
requiring us to use older beta versions. It was difficult to get Google Play services (needed for FCM) installed onto the Visual Studio Emulator via Gapps. Only [this guide](http://blog.ostebaronen.dk/2016/04/installing-gapps-in-visual-studio.html) had a
package that managed to work with a specific emulator. 

We took two approaches here. In one approach Adina Shanholtz used [Google’s Firebase platform](https://firebase.google.com/docs/notifications/) 
to trigger the notification from a web portal; in another approach, Andy Reitano used [GCM (Google Cloud Messaging)](https://developers.google.com/cloud-messaging/)
to send push notifications from the cloud. 


### Dynamic UI ###

A dynamic UI is used to generate a distinct look for each of your customers when there are 
no pre-defined items to draw on the screen. 

VitalTrax was looking for a solution where each user's survey could have a unique logo, color scheme, and questions. We created modular components, or widgets, in C#, 
which generate the XAML, or markup (UI) for our page. 

For example, to create a text Label, we’d write a function like this:


```C#
		  /// <summary>
			/// Empty container to hold a person's attributes: Name, Role, Image, etc.,
			/// </summary>
			public StackLayout personContainer = new StackLayout()
			{
			};
		

			/// <summary>
			/// Creates a Label based on the user's name
			/// </summary>
			/// <returns>Label w/ user's name</returns>
			/// <param name="name">What is the name of this person?</param>
			public Label nameComp(string name)
			{
				Label _nameComp = new Label()
				{
					Text 				    = name,
					HorizontalTextAlignment = TextAlignment.Center,
					TextColor			    = Color.Black,
					Margin				    = new Thickness(0, 20, 0, 5),
					FontSize 				= 18
				};
				return _nameComp;
			}


	  	/// <summary>
			/// Create a person (StackLayout) from local data
			/// </summary>
			/// <returns>The person.</returns>
			private StackLayout createPerson() 
			{ 
				// Create a new component from Component.cs library
				_component   = new Component();
				var myPerson = _component.personContainer;
	

				// Add all of the components needed for a person
				myPerson.Children.Add(_component.nameComp(_component._name	   ));
				myPerson.Children.Add(_component.roleComp(_component._role	   ));
				myPerson.Children.Add(_component.photoComp(_component._imageUri));
	

				return myPerson;
			}
```

C# has default parameters; functions use pre-defined values if we don't pass one in as an argument 
so that the various UI elements would have always have an initial template. 
To change pre-defined values, we made a REST request to VitalTrax's server and pulled down the 
JSON file that defines the font style, size, along with any questionnaire images or questions.

### DevOps ###
  ![Kevin Dev Ops Overview]({{site.baseurl}}/images/8.jpg)

Along with our mobile app work, we also examined VitalTrax's entire product lifecycle and recommended 
several DevOps best practice improvements.  Although VitalTrax already had an in-depth 
solution in place and were using Visual Studio Team Services (VSTS) for Continuous Integration
(CI) and Continuous Deployment (CD), we still found opportunities for improvement.  


**The Findings**

VitalTrax is comfortable with a three-month Wing Enterprise and new Wing application product development lifecycle. 
Their pharmaceutical industry customers like, and would prefer, fewer updates. 
For each process in their product lifecycle, VitalTrax estimated that their new feature development time 
was about 34.5 days (6 weeks);  their leadership was not surprised by this.
VitalTrax tries to complete two or three of these sprints for each quarterly release of their products.

The value stream map shows six main processes that make up their product development lifecycle:

1.	Iterative Planning
2.	Local 
3.	Dev
4.	QA (Quality Assurance)
5.	UAT (User Acceptance Testing)
6.	Production


**Iterative Planning** - This is where product invention, features, and fixes are defined and prototyped.
Because this is an iterative process, no estimates were given about long each specific process would take.  The process end involves sprint planning and mock-up development.

Although the goal is to have mock-ups done before development starts, sometimes that's not realistic. 
One person does all the mock-up work; and there have been problems when
developers start work based on partially-done mock-ups.

**Local** - This is where the developers work on product on their local workstations. VitalTrax wants 
to include more local unit or automated testing during this stage. This could reduce scrap-rates--the percentage
 of times the code has to be handed back after it reaches the Dev and QA smoke testing.

**Dev** - VitalTrax has a "Dev" environment to host the testing-ready application versions.  
Developers, who work from a git fork off a dev branch, do a pull request; features going into stories
and stories making up the release.  Code is reviewed before the pull request is approved and merged. 
All features and stories for a release must be complete and approved before moving to this Dev step. 
Once approved, VitalTrax has implemented Continuous Integration and Continuous Deployment in VSTS.

  ![Dev Ops Build Def]({{site.baseurl}}/screens/VSTS-Build-Definition.jpg)
  ![Dev Ops Release Def]({{site.baseurl}}/screens/VSTS-Release-Definition.jpg)


The Dev, QA, and UAT environments are hosted on Azure virtual machines. 
They are built and configured using PowerShell scripts, but are not rebuilt with every new product release lifecycle.  

Todd Kueny on Continuous Integration, Continuous Deployment and Release Management in VSTS: 

 > “All I can say is, I love this process.  This makes it so much simpler and so much easier to understand.. 
 > So much easier 1) to deploy and 2) to know what’s in the deployment.  You know, I’ve done this a lot at
 > a lot of different companies, and this is the best process I’ve had by far, because it
 > provide the complete traceability and it’s like just a one-click thing.”

Todd Kueny on Visual Studio Team Services:

> “There’s just so many cool things you can do with this. I don’t even know how to explain it.  
> This is just one of the coolest tools I’ve used in a long time.”


Isaac Collier on moving to ARM template deployments (IaC):

> “I had started to look into using the resource manager templates instead. It was a little bit 
> overwhelming at first just because you look at ‘em and it’s just like pages and pages of text.  
> So figuring out what I need from those is still something that needs to be done.  But I would like
> to switch to that because there are some pieces right now that I’m doing manually that could certainly 
> be automated.  


> The other goal from my perspective is around looking into processes for being able to continually do 
> this because what we’d like to be able to do is to continue to iterate on our infrastructure, and 
> continue to improve it.  And make changes in a way that… I’d like to do this in less of a manual way.  
> If there’s a way that we can make changes in a more structured way; meaning that we’re changing some
> code or a template or something like that and then redeploying it, or the changes being deployed 
> through a script of some kind, I’d prefer to do that.”

  ![Azure-Resource-Groups]({{site.baseurl}}/screens/Azure-Resource-Groups.jpg)

After Value Stream Mapping, VitalTrax wanted to use an Azure Resource Manager template 
to configure their environments. They also wanted  Azure Automation Desired State Configuration (DSC) 
to add configuration management functionality and Azure Scale Sets for virtual machine performance scaling.

**QA** – A “test” environment (virtual machines in Azure) is where the product goes through additional
smoke tests and validation.  This testing is done by a small group, and involves a few scripts.  VitalTrax would like to add more automation to their testing
during both the Dev and QA product lifecycle stages; they feel there is currently too much manual effort here.    

**UAT** – The customer contributes to the user acceptance test plan earlier in the process (ideally as part of iterative planning); during this stage these contributions will be tested by both
VitalTrax and the customer.  If there isn't enough early customer 
contribution, they may not see what they expect, causing
re-work.  
VitalTrax had trouble estimating a scrap rate here; they've only done this once and it wasn't a particularly pleasant experience. At some point in the future, VitalTrax could use HockeyApp to support the mobile application UAT.

As Isaac worked on a basic template deployment, Kevin implemented a proof-of-concept
template (for load balancing and scale sets) that quickly deploys an Azure Scale Set of virtual machines. 
This [sample](https://github.com/KevinRemde/VTestDSCSS) demonstrates how to create an Azure Automation service and uses Azure Automation 
Desired State Configuration (DSC) to
deploy a sample web server configuration to scale set members.


## Conclusion ##

With a mobile application, VitalTrax has the opportunity to embed HTML5 based questionnaires, or create them 
natively with C#, offering their customers greater flexibility than ever before. Each company can have a 
distinct look and feel, with a customized color scheme, font, and logo. 

Now that VitalTrax has offline sync. their users can fill out a questionnaire whether they do or don't currently have
internet connectivity. Along with that and push notifications, 
users can be notified when a trial is due, set to expire, or a new one arrives.

These key factors allow VitalTrax's Wing Enterprise application to stand out from the competition
and break through an industry not typically known for adopting the latest technical feats. 

### General lessons / Challenges ###

Push notifications proved to be as difficult as anticipated. Xamarin wanted to combine three mobile 
platforms into one, and each of those platforms has distinctive ways to implement some of their features. Push
notifications is one of those features. 

Additionally, because the mobile world moves quickly, documentation becomes out of date soon after it is created, and finding relevant tutorials and NuGet packages can be a challenge. 

In terms of reusable code, the [Xamarin Reusable Components](https://github.com/DaveVoyles/Xamarin-Rusable-Components)
we created offer a lesson in how to authenticate against a RESTful API with RESTsharp, and provide a
sample on how to generate XAML from C# code. The developer can use this if they don't know how many elements will be drawn on a page. 

### Opportunities going forward: ###

VitalTrax would like to flesh out the Xamarin.Forms application to include push notifications for 
iOS. They would like to have healthcare companies create a questionnaire in their web portal backend, 
which would generate a JSON file with those questions, and dynamically generate a form in C# for the Xamarin application
when the end user logs in to fill out a survey. 

Isaac and Kevin came up with four areas to address; some while they were together and others for
future investigation. 

1.	Ongoing – source/change control
2.	[High Availability](https://github.com/KevinRemde/VTestDSCSS ) (Load Balancing and Scale Sets) 
3.	[SQL Server Scale](https://github.com/Azure/azure-quickstart-templates/tree/master/sql-server-2014-alwayson-existing-vnet-and-ad) (database storage) 
4.	[Set up VMs with encrypted VHDs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) 


> “Microsoft won the technology battle fair and square. We looked at all of the options available to us,
> and they simply offered the best.”

 **– Syed Zikria, Founder/CEO**


## Additional resources ##

### GitHub Repositorites ###

- [Xamarin Reusable Components](https://github.com/DaveVoyles/Xamarin-Rusable-Components)
- [VitalTrax's repository for this project](https://github.com/DaveVoyles/xam-test-andy)

### Useful Xamarin Documentation ###

- [SQLite for Xamarin.Forms](http://go.microsoft.com/fwlink/?LinkId=620342)

### Misc ###

- [Zeplin.io](http://www.Zeplin.io) - Useful for mocking up UI on mobile platforms
- [Xamarin app from Evolve conference]( https://github.com/xamarinhq/app-evolve)