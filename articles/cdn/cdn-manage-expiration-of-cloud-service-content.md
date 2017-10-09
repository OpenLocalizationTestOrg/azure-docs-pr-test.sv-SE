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
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Hantera förfallodatum för Azure Web Apps/Cloud Services, ASP.NET och IIS innehåll i Azure CDN
> [!div class="op_single_selector"]
> * [Azure Web Apps/molntjänster, ASP.NET och IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage blob-tjänst](cdn-manage-expiration-of-blob-content.md)
> 
> 

Filer från en offentligt tillgänglig ursprung webbserver kan cachelagras i Azure CDN tills dess time to live (TTL) går ut.  hello TTL bestäms av hello [ *Cache-Control* huvud](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) i hello HTTP-svar från hello ursprungsservern.  Den här artikeln beskriver hur tooset `Cache-Control` huvuden för Azure-Webbappar, Azure Cloud Services, ASP.NET-program och Internet Information Services-platser, som är konfigurerade på samma sätt.

> [!TIP]
> Du kan välja tooset inga TTL-värde på en fil.  I det här fallet gäller Azure CDN automatiskt en standard-TTL sju dagar.
> 
> Mer information om hur fungerar Azure CDN toospeed åtkomst toofiles och andra resurser finns hello [översikt över Azure CDN](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Inställningen Cache-Control huvuden i konfigurationen
För statiskt innehåll, till exempel bilder och formatmallar, kan du styra hello uppdateringsfrekvens genom att ändra hello **applicationHost.config** eller **web.config** filer för ditt webbprogram.  Hej **system.webServer\staticContent\clientCache** elementet i konfigurationsfilen för hello anger hello `Cache-Control` huvud för ditt innehåll. För **web.config**, hello konfigurationsinställningar påverkar allt i hello-mappen och alla undermappar såvida åsidosätts på hello undermapp nivå.  Du kan till exempel standard time to live på hello rot toohave alla statiskt innehåll cachelagras för tre dagar, men har en undermapp som har flera variabeln innehåll med en cache-inställningen 6 timmar.  För **applicationHost.config**, alla program på hello platsen kommer att påverkas, men kan åsidosättas i **web.config** filer i hello program.

hello följande XML visar ett exempel på hur **clientCache** toospecify en maximal ålder för 3 dagar:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Ange **UseMaxAge** lägger till en `Cache-Control: max-age=<nnn>` toohello huvudsvar baserat på hello-värdet som anges i hello **CacheControlMaxAge** attribut. hello-formatet för timespan hello är för hello **cacheControlMaxAge** attributet <days>.<hours>:<min>:<sec>. Mer information om hello **clientCache** nod, se [klientcachen <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Inställningen Cache-Control huvuden i koden
För ASP.NET-program kan du ange hello CDN cachebeteende via programmering genom att ange hello **HttpResponse.Cache** egenskapen. Mer information om hello **HttpResponse.Cache** egenskap, se [HttpResponse.Cache egenskapen](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) och [HttpCachePolicy klassen](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Om du vill tooprogrammatically cache-innehåll i ASP.NET, se till att hello innehåll är cachelagringsbara genom att ange HttpCacheability för*offentliga*. Kontrollera också att en cache-verifieraren har angetts. hello cache verifieraren kan vara en senast ändrad tidsstämpel som anropar SetLastModified eller ett etag-värde som angetts genom att anropa SetETag. Alternativt kan du kan även ange en cache-förfallotid genom att anropa SetExpires eller du förlita dig på hello standard cache heuristik beskrivs tidigare i det här dokumentet.  

Till exempel toocache innehåll under en timme, lägga till hello följande:  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Nästa steg
* [Läs mer om hello **clientCache** element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Dokumentationen hello hello **HttpResponse.Cache** egenskapen](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Dokumentationen hello hello **HttpCachePolicy klassen**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

