<properties 
   pageTitle="Create a business-to-employee multi-channel app" 
   description="This tutorial shows you how to consume a SaaS API App from an ASP.NET hosted in Azure App Service Web Apps." 
   services="app-service\web" 
   documentationCenter="" 
   authors="syntaxc4" 
   manager="yochayk" 
   editor=""/>

<tags
   ms.service="app-service-web"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="web" 
   ms.date="03/24/2015"
   ms.author="cfowler"/>

# Create a business-to-employee multi-channel app

This tutorial shows you how to consume an API App from an ASP.NET hosted in [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

## Overview

You will learn:

- How to consume an API app from ASP.NET hosted in Web Apps.

## Prerequisites

This tutorial builds upon the series from the API App tutorials:

1. [Create an Azure API App](app-service-dotnet-create-api-app)
2. [Publish an Azure API App](app-service-dotnet-publish-api-app)
3. [Deploy an Azure API App](app-service-dotnet-deploy-api-app)
4. [Debug an Azure API App](app-service-dotnet-remotely-debug-api-app)

## Make the API app publicly accessible

In the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), select the API App. Click on the **Settings** button in the command bar. Under the **Basic Settings** blade, change the **Access Level** to **Public (anonymous)**.

![](./media/app-service-web-connect-web-app-to-saas-api/4-5-Change-Access-Level-To-Public.png)

## Create an ASP.NET MVC Application in Visual Studio

1. Open Visual Studio. Use the **New Project** dialogue to add a new **ASP.NET Web Application**. Click **OK**.

	![File > New > Web > ASP.NET Web Application](./media/app-service-web-connect-web-app-to-saas-api/1-Create-New-MVC-App-For-Consumption.png)

1. Select the **MVC** template. Click **Change Authentication**, then select **No Authentication**, and then click **OK** twice.

	![New ASP.NET Application](./media/app-service-web-connect-web-app-to-saas-api/2-Change-Auth-To-No-Auth.png)

1. In Solution Explorer, right-click the newly created Web Application project and select **Add Azure App Reference**.

	![Add Azure API App Reference...](./media/app-service-web-connect-web-app-to-saas-api/3-Add-Azure-API-App-SDK.png)

1. In the **Existing API Apps** dropdown list, select the API App you would like to connect to.

	![Select Existing API App](./media/app-service-web-connect-web-app-to-saas-api/4-Add-Azure-API-App-SDK-Dialog.png)

	>[AZURE.NOTE] The client code for connecting to the API app will be automatically generated from a Swagger API endpoint.

1. To leverage the generated API code, open the HomeController.cs file and replace the `Contact` action with the following:

	    public async Task<ActionResult> Contact()
	    {
	        ViewBag.Message = "Your contact page.";
	
	        var contacts = new ContactsList();
	        var response = await contacts.Contacts.GetAsync();
	        var contactList = response.Body;
	
	        return View(contactList);
	    }

	![HomeController.cs Code Updates](./media/app-service-web-connect-web-app-to-saas-api/5-Write-Code-Which-Leverages-Swagger-Generated-Code.png)

1. Update the `Contact` view to reflect the dynamic list of contacts with the code below:  
	<pre>// Add to the very top of the view file
	@model IList&lt;MyContactsList.Web.Models.Contact&gt;
	
	// Replace the default email addresses with the following
    &lt;h3&gt;Public Contacts&lt;/h3&gt;
    &lt;ul&gt;
        @foreach (var contact in Model)
        {
            &lt;li&gt;&lt;a href=&quot;mailto:@contact.EmailAddress&quot;&gt;@contact.Name &amp;lt;@contact.EmailAddress&amp;gt;&lt;/a&gt;&lt;/li&gt;
        }
    &lt;/ul&gt; 
	</pre>

	![Contact.cshtml Code Updates](./media/app-service-web-connect-web-app-to-saas-api/6-Update-View-To-Reflect-Changes.png)

## Deploy the web application to Web Apps in App Service

Follow the instructions available at [How to Deploy an Azure web app](web-sites-deploy.md).

>[AZURE.NOTE] If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](http://go.microsoft.com/fwlink/?LinkId=523751), where you can immediately create a short-lived starter web app in App Service. No credit cards required; no commitments.

## What's changed
* For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)
* For a guide to the change of the old portal to the new portal see: [Reference for navigating the preview portal](http://go.microsoft.com/fwlink/?LinkId=529715)
