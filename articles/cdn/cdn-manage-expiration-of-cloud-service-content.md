---
title: "aaaManage förfallotiden för webbinnehåll i Azure CDN | Microsoft Docs"
description: "Lär dig hur Azure Web Apps/Cloud Services, ASP.NET och IIS innehållet i Azure CDN toomanage gälla."
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
ms.openlocfilehash: a0154236c3893b627f4480e0688f555964862556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="5da42-103">Hantera förfallodatum för Azure Web Apps/Cloud Services, ASP.NET och IIS innehåll i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="5da42-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5da42-104">Azure Web Apps/molntjänster, ASP.NET och IIS</span><span class="sxs-lookup"><span data-stu-id="5da42-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="5da42-105">Azure Storage blob-tjänst</span><span class="sxs-lookup"><span data-stu-id="5da42-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="5da42-106">Filer från en offentligt tillgänglig ursprung webbserver kan cachelagras i Azure CDN tills dess time to live (TTL) går ut.</span><span class="sxs-lookup"><span data-stu-id="5da42-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="5da42-107">hello TTL bestäms av hello [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) i hello HTTP-svar från hello ursprungsservern.</span><span class="sxs-lookup"><span data-stu-id="5da42-107">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from hello origin server.</span></span>  <span data-ttu-id="5da42-108">Den här artikeln beskriver hur tooset `Cache-Control` huvuden för Azure-Webbappar, Azure Cloud Services, ASP.NET-program och Internet Information Services-platser, som är konfigurerade på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="5da42-108">This article describes how tooset `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="5da42-109">Du kan välja tooset inga TTL-värde på en fil.</span><span class="sxs-lookup"><span data-stu-id="5da42-109">You may choose tooset no TTL on a file.</span></span>  <span data-ttu-id="5da42-110">I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.</span><span class="sxs-lookup"><span data-stu-id="5da42-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="5da42-111">Mer information om hur fungerar Azure CDN toospeed åtkomst toofiles och andra resurser finns hello [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5da42-111">For more information about how Azure CDN works toospeed up access toofiles and other resources, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="5da42-112">Inställningen Cache-Control huvuden i konfigurationen</span><span class="sxs-lookup"><span data-stu-id="5da42-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="5da42-113">För statiskt innehåll, till exempel bilder och formatmallar, kan du styra hello uppdateringsfrekvens genom att ändra hello **applicationHost.config** eller **web.config** filer för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5da42-113">For static content, such as images and style sheets, you can control hello update frequency by modifying hello **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="5da42-114">Hej **system.webServer\staticContent\clientCache** elementet i konfigurationsfilen för hello anger hello `Cache-Control` huvud för ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="5da42-114">hello **system.webServer\staticContent\clientCache** element in hello configuration file will set hello `Cache-Control` header for your content.</span></span> <span data-ttu-id="5da42-115">För **web.config**, hello konfigurationsinställningar påverkar allt i hello-mappen och alla undermappar såvida åsidosätts på hello undermapp nivå.</span><span class="sxs-lookup"><span data-stu-id="5da42-115">For **web.config**, hello configuration settings will affect everything in hello folder and all subfolders, unless overridden at hello subfolder level.</span></span>  <span data-ttu-id="5da42-116">Du kan till exempel standard time to live på hello rot toohave alla statiskt innehåll cachelagras för tre dagar, men har en undermapp som har flera variabeln innehåll med en cache-inställningen 6 timmar.</span><span class="sxs-lookup"><span data-stu-id="5da42-116">For example, you can set a default time-to-live at hello root toohave all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="5da42-117">För **applicationHost.config**, alla program på hello platsen kommer att påverkas, men kan åsidosättas i **web.config** filer i hello program.</span><span class="sxs-lookup"><span data-stu-id="5da42-117">For **applicationHost.config**, all applications on hello site will be affected, but can be overridden in **web.config** files in hello applications.</span></span>

<span data-ttu-id="5da42-118">hello följande XML visar ett exempel på hur **clientCache** toospecify en maximal ålder för 3 dagar:</span><span class="sxs-lookup"><span data-stu-id="5da42-118">hello following XML shows an example of setting **clientCache** toospecify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="5da42-119">Ange **UseMaxAge** lägger till en `Cache-Control: max-age=<nnn>` toohello huvudsvar baserat på hello-värdet som anges i hello **CacheControlMaxAge** attribut.</span><span class="sxs-lookup"><span data-stu-id="5da42-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header toohello response based on hello value specified in hello **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="5da42-120">hello-formatet för timespan hello är för hello **cacheControlMaxAge** attributet <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="5da42-120">hello format of hello timespan is for hello **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="5da42-121">Mer information om hello **clientCache** nod, se [klientcachen <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="5da42-121">For more information on hello **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="5da42-122">Inställningen Cache-Control huvuden i koden</span><span class="sxs-lookup"><span data-stu-id="5da42-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="5da42-123">För ASP.NET-program kan du ange hello CDN cachebeteende via programmering genom att ange hello **HttpResponse.Cache** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5da42-123">For ASP.NET applications, you can set hello CDN caching behavior programmatically by setting hello **HttpResponse.Cache** property.</span></span> <span data-ttu-id="5da42-124">Mer information om hello **HttpResponse.Cache** egenskap, se [HttpResponse.Cache egenskapen](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) och [HttpCachePolicy klassen](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="5da42-124">For more information on hello **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="5da42-125">Om du vill tooprogrammatically cache-innehåll i ASP.NET, se till att hello innehåll är cachelagringsbara genom att ange HttpCacheability för*offentliga*.</span><span class="sxs-lookup"><span data-stu-id="5da42-125">If you want tooprogrammatically cache application content in ASP.NET, make sure that hello content is marked as cacheable by setting HttpCacheability too*Public*.</span></span> <span data-ttu-id="5da42-126">Kontrollera också att en cache-verifieraren har angetts.</span><span class="sxs-lookup"><span data-stu-id="5da42-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="5da42-127">hello cache verifieraren kan vara en senast ändrad tidsstämpel som anropar SetLastModified eller ett etag-värde som angetts genom att anropa SetETag.</span><span class="sxs-lookup"><span data-stu-id="5da42-127">hello cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="5da42-128">Alternativt kan du kan även ange en cache-förfallotid genom att anropa SetExpires eller du förlita dig på hello standard cache heuristik beskrivs tidigare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5da42-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on hello default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="5da42-129">Till exempel toocache innehåll under en timme, lägga till hello följande:</span><span class="sxs-lookup"><span data-stu-id="5da42-129">For example, toocache content for one hour, add hello following:</span></span>  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="5da42-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5da42-130">Next Steps</span></span>
* [<span data-ttu-id="5da42-131">Läs mer om hello **clientCache** element</span><span class="sxs-lookup"><span data-stu-id="5da42-131">Read details about hello **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="5da42-132">Dokumentationen hello hello **HttpResponse.Cache** egenskapen</span><span class="sxs-lookup"><span data-stu-id="5da42-132">Read hello documentation for hello **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="5da42-133">[Dokumentationen hello hello **HttpCachePolicy klassen**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="5da42-133">[Read hello documentation for hello **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

