---
title: "Hantera förfallotiden för webbinnehåll i Azure CDN | Microsoft Docs"
description: "Lär dig hur du hanterar du förfallodatum för Azure Web Apps/Cloud Services, ASP.NET och IIS innehållet i Azure CDN."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="2db9e-103">Hantera förfallodatum för Azure Web Apps/Cloud Services, ASP.NET och IIS innehåll i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="2db9e-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2db9e-104">Azure Web Apps/molntjänster, ASP.NET och IIS</span><span class="sxs-lookup"><span data-stu-id="2db9e-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="2db9e-105">Azure Storage blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="2db9e-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="2db9e-106">Filer från en offentligt tillgänglig ursprung webbserver kan cachelagras i Azure CDN tills dess time to live (TTL) går ut.</span><span class="sxs-lookup"><span data-stu-id="2db9e-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="2db9e-107">TTL-värdet bestäms av den [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) i HTTP-svaret från den ursprungliga servern.</span><span class="sxs-lookup"><span data-stu-id="2db9e-107">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from the origin server.</span></span>  <span data-ttu-id="2db9e-108">Den här artikeln beskriver hur du ställer in `Cache-Control` huvuden för Azure-Webbappar, Azure Cloud Services, ASP.NET-program och Internet Information Services-platser, som är konfigurerade på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="2db9e-108">This article describes how to set `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="2db9e-109">Du kan välja att ange inga TTL-värde för en fil.</span><span class="sxs-lookup"><span data-stu-id="2db9e-109">You may choose to set no TTL on a file.</span></span>  <span data-ttu-id="2db9e-110">I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.</span><span class="sxs-lookup"><span data-stu-id="2db9e-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="2db9e-111">Mer information om hur Azure CDN fungerar för att påskynda åtkomst till filer och andra resurser finns i [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2db9e-111">For more information about how Azure CDN works to speed up access to files and other resources, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="2db9e-112">Inställningen Cache-Control huvuden i konfigurationen</span><span class="sxs-lookup"><span data-stu-id="2db9e-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="2db9e-113">För statiskt innehåll, till exempel bilder och formatmallar, kan du styra uppdateringsfrekvensen genom att ändra den **applicationHost.config** eller **web.config** filer för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2db9e-113">For static content, such as images and style sheets, you can control the update frequency by modifying the **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="2db9e-114">Den **system.webServer\staticContent\clientCache** element i konfigurationsfilen anger den `Cache-Control` -huvud för ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="2db9e-114">The **system.webServer\staticContent\clientCache** element in the configuration file will set the `Cache-Control` header for your content.</span></span> <span data-ttu-id="2db9e-115">För **web.config**, konfigurationsinställningarna påverkar allt i mappen och alla undermappar såvida åsidosätts på nivån undermappen.</span><span class="sxs-lookup"><span data-stu-id="2db9e-115">For **web.config**, the configuration settings will affect everything in the folder and all subfolders, unless overridden at the subfolder level.</span></span>  <span data-ttu-id="2db9e-116">Du kan exempelvis ange en standard time to live rot ha alla statiskt innehåll som cachelagrats för 3 dagar, men har en undermapp som har fler variabeln innehåll med en cache-inställningen 6 timmar.</span><span class="sxs-lookup"><span data-stu-id="2db9e-116">For example, you can set a default time-to-live at the root to have all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="2db9e-117">För **applicationHost.config**, alla program på platsen kommer att påverkas, men kan åsidosättas i **web.config** filer i program.</span><span class="sxs-lookup"><span data-stu-id="2db9e-117">For **applicationHost.config**, all applications on the site will be affected, but can be overridden in **web.config** files in the applications.</span></span>

<span data-ttu-id="2db9e-118">Följande XML visar ett exempel på hur **clientCache** att ange en maximal ålder för 3 dagar:</span><span class="sxs-lookup"><span data-stu-id="2db9e-118">The following XML shows an example of setting **clientCache** to specify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="2db9e-119">Ange **UseMaxAge** lägger till en `Cache-Control: max-age=<nnn>` huvudet i svaret baserat på värdet som anges i den **CacheControlMaxAge** attribut.</span><span class="sxs-lookup"><span data-stu-id="2db9e-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header to the response based on the value specified in the **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="2db9e-120">Formatet för timespan är för den **cacheControlMaxAge** attributet <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="2db9e-120">The format of the timespan is for the **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="2db9e-121">Mer information om den **clientCache** nod, se [klientcachen <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="2db9e-121">For more information on the **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="2db9e-122">Inställningen Cache-Control huvuden i koden</span><span class="sxs-lookup"><span data-stu-id="2db9e-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="2db9e-123">För ASP.NET-program kan du ange CDN cachelagring av frågesträngar via programmering genom att ange den **HttpResponse.Cache** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2db9e-123">For ASP.NET applications, you can set the CDN caching behavior programmatically by setting the **HttpResponse.Cache** property.</span></span> <span data-ttu-id="2db9e-124">Mer information om den **HttpResponse.Cache** egenskap, se [HttpResponse.Cache egenskapen](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) och [HttpCachePolicy klassen](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="2db9e-124">For more information on the **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="2db9e-125">Om du vill programmässigt cache-innehåll i ASP.NET, kontrollera att innehållet är cachelagringsbara genom att ange HttpCacheability *offentliga*.</span><span class="sxs-lookup"><span data-stu-id="2db9e-125">If you want to programmatically cache application content in ASP.NET, make sure that the content is marked as cacheable by setting HttpCacheability to *Public*.</span></span> <span data-ttu-id="2db9e-126">Kontrollera också att en cache-verifieraren har angetts.</span><span class="sxs-lookup"><span data-stu-id="2db9e-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="2db9e-127">Cache-verifieraren kan vara en senast ändrad tidsstämpel som anropar SetLastModified eller ett etag-värde som angetts genom att anropa SetETag.</span><span class="sxs-lookup"><span data-stu-id="2db9e-127">The cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="2db9e-128">Alternativt kan du kan även ange en cache-förfallotid genom att anropa SetExpires eller du förlita dig på standard cache heuristik som beskrivits tidigare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="2db9e-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on the default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="2db9e-129">Till exempel cache innehållet för en timme, lägga till följande:</span><span class="sxs-lookup"><span data-stu-id="2db9e-129">For example, to cache content for one hour, add the following:</span></span>  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="2db9e-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2db9e-130">Next Steps</span></span>
* [<span data-ttu-id="2db9e-131">Läs mer information om den **clientCache** element</span><span class="sxs-lookup"><span data-stu-id="2db9e-131">Read details about the **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="2db9e-132">Läs i dokumentationen för den **HttpResponse.Cache** egenskapen</span><span class="sxs-lookup"><span data-stu-id="2db9e-132">Read the documentation for the **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="2db9e-133">[Läs i dokumentationen för den **HttpCachePolicy klassen**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="2db9e-133">[Read the documentation for the **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

