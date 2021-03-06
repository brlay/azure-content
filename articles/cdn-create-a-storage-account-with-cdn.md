<properties 
	pageTitle="How to use CDN - Azure feature guide" 
	description="Learn how to use the Azure Content Delivery Network (CDN) to deliver high-bandwidth content by caching blobs and static content." 
	services="cdn" 
	documentationCenter=".net" 
	authors="zhangmanling" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="cdn" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/05/2015" 
	ms.author="mazha"/>


# Integrate a Storage Account with CDN

CDN can be enabled to cache content from your Azure storage. It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in the United States, Europe, Asia, Australia and South America.


## Step 1: Create a storage account

Use the following procedure to create a new storage account for a
Azure subscription. A storage account gives access to 
Azure storage services. The storage account represents the highest level
of the namespace for accessing each of the Azure storage service
components: Blob services, Queue services, and Table services. For more
information about the Azure storage services, see [Using the
Azure Storage Services](http://msdn.microsoft.com/library/azure/gg433040.aspx).

To create a storage account, you must be either the service
administrator or a co-administrator for the associated subscription.

> [AZURE.NOTE] For information about performing this operation by using the
Azure Service Management API, see the [Create Storage Account](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) reference topic.

**To create a storage account for an Azure subscription**

1.  Log into the [Azure Management Portal].
2.  In the lower left corner, click **New**. In the **New** Dialog, select **Data Services**, then click **Storage**, then **Quick Create**.

    The **Create Storage Account** dialog appears.

    ![Create Storage Account][create-new-storage-account]

4. In the **URL** field, type a subdomain name. This entry can contain from 3-24 lowercase letters and numbers.

    This value becomes the host name within the URI that is used to
    address Blob, Queue, or Table resources for the subscription. To
    address a container resource in the Blob service, you would use a
    URI in the following format, where *&lt;StorageAccountLabel&gt;* refers
    to the value you typed in **Enter a URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*

    **Important:** The URL label forms the subdomain of the storage
    account URI and must be unique among all hosted services in 
    Azure.

	This value is also used as the name of this storage account in the portal, or when accessing this account programmatically.

5.  From the **Region/Affinity Group** drop-down list, select a region or affinity group for the storage account. Select an affinity group instead of a region if you want your storage services to be in the same data center with other Microsoft Azure services that you are using. This can improve performance, and no charges are incurred for egress.  

    **Note:** To create an affinity group, open the **Settings** area of the Management Portal, click **Affinity Groups**, and then click either **Add an affinity group** or **Add**. You can also create and manage affinity groups using the Microsoft Azure Service Management API. For more information, see [Operations on Affinity Groups].

6. From the **Subscription** drop-down list, select the subscription that the storage account will be used with.
7.  Click **Create Storage Account**. The process of creating the storage account might take several minutes to complete.
8.  To verify that the storage account was created successfully, verify that the account appears in the items listed for **Storage** with a status of **Online**.


## Step 2: Create a new CDN endpoint for your storage account

Once you enable CDN access to a storage account or hosted service, all
publicly available objects are eligible for CDN edge caching. If you
modify an object that is currently cached in the CDN, the new content
will not be available via the CDN until the CDN refreshes its content
when the cached content time-to-live period expires.

**To create a new CDN endpoint for your storage account**

1. In the [Azure Management Portal], in the navigation pane, click **CDN**.

2. On the ribbon, click **New**. In the **New** dialog, select **App Services**, then **CDN**, then **Quick Create**.

3. In the **Origin Domain** dropdown, select the storage account you created in the previous section from the list of your available storage accounts. 

4. Click the **Create** button to create the new endpoint.

5. Once the endpoint is created, it appears in a list of endpoints for the subscription. The list view shows the URL to use to access cached content, as well as the origin domain. 

	The origin domain is the location from which the CDN caches
    content. The origin domain can be either a storage account or a cloud service; a storage account is used for the purposes of this example. Storage content is cached to edge servers according either to a cache-control setting that you specify, or to the default heuristics of the caching network. See [How to Manage Expiration of Blob Content](http://msdn.microsoft.com/library/gg680306.aspx) for more information. 


    > [AZURE.NOTE] The configuration created for the endpoint will not
    immediately be available; it can take up to 60 minutes for the
    registration to propagate through the CDN network. Users who try to
    use the CDN domain name immediately may receive status code 400
    (Bad Request) until the content is available via the CDN.


## Step 3: Access CDN content

To access cached content on the CDN, use the CDN URL provided in the portal. The address for a cached blob will be similar to the following:

http://<*CDNNamespace*\>.vo.msecnd.net/<*myPublicContainer*\>/<*BlobName*\>


## Step 4: Remove content from the CDN

If you no longer wish to cache an object in the Azure Content
Delivery Network (CDN), you can take one of the following steps:

-   You can make the container private instead of public. See [Restrict Access to Containers and Blobs](http://msdn.microsoft.com/library/dd179354.aspx) for more information.
-   You can disable or delete the CDN endpoint using the Management
    Portal.
-   You can modify your hosted service to no longer respond to requests for the
    object.

An object already cached in the CDN will remain cached until the
time-to-live period for the object expires. When the time-to-live period
expires, the CDN will check to see whether the CDN endpoint is still
valid and the object still anonymously accessible. If it is not, then
the object will no longer be cached.

The ability to immediately purge content is currently not supported on Azure Management Portal. Please contact [Azure support](http://azure.microsoft.com/support/options/)  if you need to immediately purge content. 

## Additional resources

-   [How to Create an Affinity Group in Azure]
-   [How to: Manage Storage Accounts for an Azure Subscription]
-   [How to Map CDN Content to a Custom Domain]

[Create Storage Account]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure CDN Node Locations]: cdn-pop-locations.md
[Azure Management Portal]: https://manage.windowsazure.com/
[billing plan]: /pricing/calculator/?scenario=full
[How to Register a Custom Subdomain Name for Accessing Blobs in Azure]: storage-custom-domain-name.md
[How to Create an Affinity Group in Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
[Overview of the Azure CDN]: cdn-overview.md
[How to: Manage Storage Accounts for an Azure Subscription]: https://msdn.microsoft.com/en-us/library/azure/hh531793.aspx
[How to Map CDN Content to a Custom Domain]: cdn-map-content-to-custom-domain.md


[create-new-storage-account]: ./media/cdn-how-to-use/CDN_CreateNewStorageAcct.png

