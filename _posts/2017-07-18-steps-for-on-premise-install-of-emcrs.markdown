---
layout: post
title: Steps for Installing SharePoint Provider Hosted App On Premise
date: '2017-07-18 22:37:00'
tags:
- sharepoint
- sharepoint-2013
---

1. Create SharePoint 2013 Farm
1. Create SharePoint site collection with development template.
1. Upload app into "app in testing" library.
1. Create new site using IIS on the vm where the SharePoint is installed.
1. Assign identity for application pool for database connection string to use windows authentication.
1. Install App Management Shared Service Proxy.
1. Miscellaneous Findings
1. Useful Resources

### Create SharePoint 2013 Farm
This step is easy when you use the [new azure portal](http://portal.azure.com "New Azure Portal"). Which allows you to provision a new farm with a click of a button (and fill in some setup configurations.
### Create Certificate
Follow the instruction in the below link.
[http://msdn.microsoft.com/en-us/library/office/fp179901.aspx](http://msdn.microsoft.com/en-us/library/office/fp179901.aspx)
### Assign identity for application pool for database connection string to use windows authentication
Assigned identity *sp_setup* for the application pool and changed *web.config* to use the sql server, and integrated security.
### App Management Shared Service Proxy is not installed.
This error is because you do not have an App Management Service created and or the Subscription Settings Service in your Service Applications. To resolve this issue go to Central Administration and click the link "Manage service applications" under Application Management. Create the App Management Service application by clicking New → App Management Service. Type an applicable name of the service and application pool ([reference](http://www.mavention.com/blog/error-occurred-in-deployment-step-install-app-for-sharepoint-app-management-shared-service-proxy-is-not-installed)).
### Create DNS CNAME entry for the app domain
From cmd.exe, enter these commands:
```
dnscmd.exe . /ZoneAdd ContosoApps.com /dsprimary
dnscmd.exe . /RecordAdd contosoapps.com * CNAME contoso.com
```
### Miscellaneous Findings
If you get the following error:
> Settings or services required to complete this request are not currently available.  Try this operation again later.  If the problem persists, contact your administrator. 

Start the Managed Meta data Web Service from the services on server page in central admin.

The SharePoint web application needs to support https ssl. If not setup, you may get following error messages
> SEC7111: HTTPS security is compromised by http://sharepoint.contoso.com/sites/dev/_layouts/15/SP.Runtime.js?=1416849419923

### Useful resources
- [http://msdn.microsoft.com/en-us/library/office/fp179901.aspx](http://msdn.microsoft.com/en-us/library/office/fp179901.aspx)
- [http://blogs.technet.com/b/mspfe/archive/2013/01/31/configuring-sharepoint-on-premise-deployments-for-apps.aspx](http://blogs.technet.com/b/mspfe/archive/2013/01/31/configuring-sharepoint-on-premise-deployments-for-apps.aspx)
- [http://technet.microsoft.com/en-us/library/fp161236%28v=office.15%29.aspx](http://technet.microsoft.com/en-us/library/fp161236%28v=office.15%29.aspx)
- [http://sharepointchick.com/archive/2012/07/29/setting-up-your-app-domain-for-sharepoint-2013.aspx](http://technet.microsoft.com/en-us/library/fp161236%28v=office.15%29.aspx)