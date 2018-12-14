---
layout: post
title: Migrating from SOAP to REST using Azure API Management
tags: []
date: 2018-12-14 00:00:00 +0000
permalink: migrating-from-soap-to-rest-using-azure-api-management

---
If you've got legacy applications written in WSDL or SOAP, and you are planning to expose it as a rest endpoint, Azure API Management seems to be good option at the moment. Another option is apigee; Although, it seems to miss some of the features thats available through Azure API management. AWS doesnt seem to have any support out of the box for this particular scenario.

I will use a public calculator wcf service available [**here**](http://www.dneonline.com/calculator.asmx?wsdl) for showing whats involved in setting it up in Azure API Management.

## Azure API Management - SOAP to REST

From the Azure Portal, Search for API management and select Create.
![Search for API Management](/uploads/Screen Shot 2018-12-14 at 10.15.42.png)Give your API management service a name, select appropriate subscription, resource group, location, org name, etc in the following form.

The process of actually activating the service creation took about 30 minutes for me.

After the creation and activation of the service, navigate to the API tab, select add API and select WSDL tile.

![Create new WSDL API](/uploads/Screen Shot 2018-12-14 at 11.03.00.png)

In the following screen, select Full option and enter fill out the form appropriately.

![Full create from WSDL wizard](/uploads/Screen Shot 2018-12-14 at 14.16.31.png)Notice that the import method can be either [SOAP pass-through](https://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) and [SOAP to REST](https://blogs.msdn.microsoft.com/apimanagement/2016/12/14/soap-to-rest/) and I have selected "SOAP to Rest". Selecting the "Full" option has also given me option to configure versioning for the API as well. There are 3 options for Versioning schema. "Path" (which I have chosen here), "Header" and "Query string".

![SOAP to REST Azure API management design screen](/uploads/Screen Shot 2018-12-14 at 14.28.14.png)

As part of the import process, Azure API Management is generating special policies that to the inbound transform from JSON to a SOAP envelope, and vice versa on the outbound flow.

To test the restful apis, let's go to the "developer portal" by clicking on the link at the top left hand corner. And navigate to "Calculator API". Once you are at the 'Calculator API v1\` documentation site, select "Try It". You can also use **"Test"** tab from with Azure Portal do the same steps below.

![Azure API Management - Making a request](/uploads/Screen Shot 2018-12-14 at 14.33.53.png)In the screenshot above, I am making the `POST` request to the `Add` operation `1+1`. In the response you can see, I got `2` back as the answer. Notice in url, the version `v1` is in the path. Azure API Management has added the headers `Ocp-Apim-Trace: true` and `Ocp-Apim-Subscription-Key` for me. `Ocp-Apim-Subscription-Key` is used to authenticate the request.

You can disable the need to require a subscription key for making the requests through the Product the API is associated with from the Azure Portal. However, please be aware that doing so, you will loose all metrics associated with the different users and any other functionalities that you may wish to apply differently to different users. This might be a good option for you if you plan to call the Calculator API from a SPA application. You can then choose to authenticate the user using **OAuth 2.0** or **OpenID Connect**. These options are available under Settings from the Azure Poral.

`Ocp-Apim-Trace: true` header allows you to diagnose the API requests, such as the policies, transformations, and any errors in the pipeline. You can select the **"Trace"** tab to take a look at the response.

### Authentication

I have already mentioned the ability to choose an authentication option above. The possible options are **None**, **OAuth 2.0**, and **OpenID Connect**. There is also a policy that you can add to the api endpoints that can validate JWT Token as well.

### Application Insights

You can configure to use an Application Insights instance for monitoring the API. And this could be an individual Application Insight instance per API version.

![Azure API Management - Application Insight settings](/uploads/Screen Shot 2018-12-14 at 15.17.12.png)

Advanced options in the screenshot above allows you to set Headers and First bytes of body per pipeline.

![Application Insight - Application map](/uploads/Screen Shot 2018-12-14 at 15.35.50.png)

### Next steps

You can image rewriting each API endpoint to point to different backend one at a time. Azure API Management makes it easy to do this. From the **API Design view**, You can select individual operation and edit the backend for that operation.
![Migrate API Management Operation](/uploads/Screen Shot 2018-12-14 at 15.55.36.png)
You can overwrite the http(s) endpoint and make the operation point to a totally new endpoint. An Azure Function endpoint perhaps?. The Azure Resource option only seems to allow you to select a Logic App. But, would make sense to allow other resources as well.

## Additional Resources

* [Mock API Responses](https://docs.microsoft.com/en-gb/azure/api-management/mock-api-responses)
* [Add caching to improve performance](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-cache)
* [Use an external Azure Cache for Redis](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-cache-external)
* [Sync Azure API Management with a GIT repository](https://docs.microsoft.com/en-us/azure/api-management/api-management-configuration-repository-git)
* [Pricing](https://azure.microsoft.com/en-gb/pricing/details/api-management/)