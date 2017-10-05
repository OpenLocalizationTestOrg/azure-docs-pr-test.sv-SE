---
title: "Cache-Utdatacacheprovider för ASP.NET"
description: "Lär dig att cachelagra ASP.NET sidutdata med hjälp av Azure Redis-Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: sdanie
ms.openlocfilehash: 845f25637a0e48460fc76c1ee36060274b3cec38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="a0862-103">ASP.NET Utdatacacheprovider för Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="a0862-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="a0862-104">Redis Utdatacacheprovider är en mekanism för lagring av out-of-process för cache-utdata.</span><span class="sxs-lookup"><span data-stu-id="a0862-104">The Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="a0862-105">Informationen är specifikt för fullständig HTTP-svar (sidan cachelagring av utdata).</span><span class="sxs-lookup"><span data-stu-id="a0862-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="a0862-106">Providern ansluts till den nya utdata cache providern utökningspunkt som introducerades i ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="a0862-106">The provider plugs into the new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="a0862-107">Om du vill använda Redis Utdatacacheprovider först konfigurera din cache och sedan konfigurera ASP.NET-program med hjälp av Redis utdata Cache providern NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="a0862-107">To use the Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using the Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="a0862-108">Det här avsnittet ger vägledning om hur du konfigurerar programmet att använda Redis Utdatacacheprovider.</span><span class="sxs-lookup"><span data-stu-id="a0862-108">This topic provides guidance on configuring your application to use the Redis Output Cache Provider.</span></span> <span data-ttu-id="a0862-109">Mer information om hur du skapar och konfigurerar Azure Redis-Cache-instansen finns [skapa ett cacheminne](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="a0862-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-the-cache"></a><span data-ttu-id="a0862-110">Lagra ASP.NET sidutdata i cacheminnet</span><span class="sxs-lookup"><span data-stu-id="a0862-110">Store ASP.NET page output in the cache</span></span>
<span data-ttu-id="a0862-111">Om du vill konfigurera ett klientprogram i Visual Studio med Redis-Cache Session tillstånd NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från den **verktyg**menyn.</span><span class="sxs-lookup"><span data-stu-id="a0862-111">To configure a client application in Visual Studio using the Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu.</span></span>

<span data-ttu-id="a0862-112">Kör följande kommando från fönstret `Package Manager Console`.</span><span class="sxs-lookup"><span data-stu-id="a0862-112">Run the following command from the `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="a0862-113">Redis utdata Cache providern NuGet-paketet har ett beroende på StackExchange.Redis.StrongName-paketet.</span><span class="sxs-lookup"><span data-stu-id="a0862-113">The Redis Output Cache Provider NuGet package has a dependency on the StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="a0862-114">Om paketet StackExchange.Redis.StrongName inte finns i ditt projekt, installeras.</span><span class="sxs-lookup"><span data-stu-id="a0862-114">If the StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="a0862-115">Mer information om Redis utdata Cache providern NuGet-paketet finns i [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-sidan.</span><span class="sxs-lookup"><span data-stu-id="a0862-115">For more information about the Redis Output Cache Provider NuGet package, see the [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="a0862-116">Förutom starkt krypterat namn StackExchange.Redis.StrongName paket finns det också StackExchange.Redis icke-starkt krypterat namn version.</span><span class="sxs-lookup"><span data-stu-id="a0862-116">In addition to the strong-named StackExchange.Redis.StrongName package, there is also the StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="a0862-117">Om ditt projekt använder icke-starkt krypterat namn StackExchange.Redis versionen måste du avinstallera den, hämta annars du namnkonflikter i projektet.</span><span class="sxs-lookup"><span data-stu-id="a0862-117">If your project is using the non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="a0862-118">Mer information om dessa paket finns [konfigurerar .NET-cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="a0862-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="a0862-119">NuGet-paketet hämtar och lägger till de nödvändiga sammansättningsreferenser och lägger till följande avsnitt i web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="a0862-119">The NuGet package downloads and adds the required assembly references and adds the following section into your web.config file.</span></span> <span data-ttu-id="a0862-120">Det här avsnittet innehåller nödvändig konfiguration för ASP.NET-program att använda Redis Utdatacacheprovider.</span><span class="sxs-lookup"><span data-stu-id="a0862-120">This section contains the required configuration for your ASP.NET application to use the Redis Output Cache Provider.</span></span>

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

<span data-ttu-id="a0862-121">Kommenterade avsnittet ger ett exempel på de attribut och exempel på inställningar för varje attribut.</span><span class="sxs-lookup"><span data-stu-id="a0862-121">The commented section provides an example of the attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="a0862-122">Konfigurera attribut med värden från din cachebladet i Microsoft Azure-portalen och konfigurera andra värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="a0862-122">Configure the attributes with the values from your cache blade in the Microsoft Azure portal, and configure the other values as desired.</span></span> <span data-ttu-id="a0862-123">Anvisningar för att komma åt egenskaper för cache finns [konfigurera Redis cacheinställningarna](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="a0862-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="a0862-124">**värden** – ange cache-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a0862-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="a0862-125">**port** – använda icke-SSL-port eller SSL-port, beroende på ssl-inställningar.</span><span class="sxs-lookup"><span data-stu-id="a0862-125">**port** – use either your non-SSL port or your SSL port, depending on the ssl settings.</span></span>
* <span data-ttu-id="a0862-126">**accessKey** – Använd den primära eller sekundära nyckeln för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="a0862-126">**accessKey** – use either the primary or secondary key for your cache.</span></span>
* <span data-ttu-id="a0862-127">**SSL** – SANT om du vill skydda klient och cache-kommunikation med ssl, annars false.</span><span class="sxs-lookup"><span data-stu-id="a0862-127">**ssl** – true if you want to secure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="a0862-128">Se till att ange rätt port.</span><span class="sxs-lookup"><span data-stu-id="a0862-128">Be sure to specify the correct port.</span></span>
  * <span data-ttu-id="a0862-129">Observera att icke-SSL-porten är inaktiverad som standard för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="a0862-129">The non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="a0862-130">Ange true för den här inställningen att använda SSL-porten.</span><span class="sxs-lookup"><span data-stu-id="a0862-130">Specify true for this setting to use the SSL port.</span></span> <span data-ttu-id="a0862-131">Mer information om hur du aktiverar icke-SSL-porten finns i [Åtkomstportar](cache-configure.md#access-ports) under den [konfigurera en cache](cache-configure.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a0862-131">For more information about enabling the non-SSL port, see the [Access Ports](cache-configure.md#access-ports) section in the [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="a0862-132">**databaseId** – angivna vilken databas som ska användas för cache-utdata.</span><span class="sxs-lookup"><span data-stu-id="a0862-132">**databaseId** – Specified which database to use for cache output data.</span></span> <span data-ttu-id="a0862-133">Om inget anges används standardvärdet 0.</span><span class="sxs-lookup"><span data-stu-id="a0862-133">If not specified, the default value of 0 is used.</span></span>
* <span data-ttu-id="a0862-134">**applicationName** – nycklar lagras i redis som `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="a0862-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="a0862-135">Den här namngivningsschemat kan flera program kan dela samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="a0862-135">This naming scheme enables multiple applications to share the same key.</span></span> <span data-ttu-id="a0862-136">Den här parametern är valfri och om du inte anger den ett standardvärde används.</span><span class="sxs-lookup"><span data-stu-id="a0862-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="a0862-137">**connectionTimeoutInMilliseconds** – den här inställningen kan du åsidosätta inställningen connectTimeout i StackExchange.Redis-klienten.</span><span class="sxs-lookup"><span data-stu-id="a0862-137">**connectionTimeoutInMilliseconds** – This setting allows you to override the connectTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="a0862-138">Om inget anges används standardinställningen connectTimeout 5000.</span><span class="sxs-lookup"><span data-stu-id="a0862-138">If not specified, the default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="a0862-139">Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="a0862-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="a0862-140">**operationTimeoutInMilliseconds** – den här inställningen kan du åsidosätta inställningen syncTimeout i StackExchange.Redis-klienten.</span><span class="sxs-lookup"><span data-stu-id="a0862-140">**operationTimeoutInMilliseconds** – This setting allows you to override the syncTimeout setting in the StackExchange.Redis client.</span></span> <span data-ttu-id="a0862-141">Om inget anges används standardinställningen för syncTimeout 1000.</span><span class="sxs-lookup"><span data-stu-id="a0862-141">If not specified, the default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="a0862-142">Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="a0862-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="a0862-143">Lägg till en direktivet OutputCache på varje sida som du vill cachelagra utdata.</span><span class="sxs-lookup"><span data-stu-id="a0862-143">Add an OutputCache directive to each page for which you wish to cache the output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="a0862-144">I föregående exempel cachelagrade siddata finns kvar i cacheminnet för 60 sekunder, och en annan version av sidan cachelagras för varje parameter-kombination.</span><span class="sxs-lookup"><span data-stu-id="a0862-144">In the previous example, the cached page data remains in the cache for 60 seconds, and a different version of the page is cached for each parameter combination.</span></span> <span data-ttu-id="a0862-145">Läs mer om direktivet OutputCache [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="a0862-145">For more information about the OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="a0862-146">Programmet är konfigurerat för att använda Redis Utdatacacheprovider när dessa steg utförs.</span><span class="sxs-lookup"><span data-stu-id="a0862-146">Once these steps are performed, your application is configured to use the Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0862-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0862-147">Next steps</span></span>
<span data-ttu-id="a0862-148">Kolla in den [ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="a0862-148">Check out the [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

