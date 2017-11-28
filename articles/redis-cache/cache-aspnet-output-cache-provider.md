---
title: "aaaCache Utdatacacheprovider för ASP.NET"
description: "Lär dig hur toocache utdata ASP.NET-sida med hjälp av Azure Redis-Cache"
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
ms.openlocfilehash: fc38cc657604b351f55ad8febac383783ac29700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a><span data-ttu-id="763cc-103">ASP.NET Utdatacacheprovider för Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="763cc-103">ASP.NET Output Cache Provider for Azure Redis Cache</span></span>
<span data-ttu-id="763cc-104">Hej Redis Utdatacacheprovider är en mekanism för lagring av out-of-process för cache-utdata.</span><span class="sxs-lookup"><span data-stu-id="763cc-104">hello Redis Output Cache Provider is an out-of-process storage mechanism for output cache data.</span></span> <span data-ttu-id="763cc-105">Informationen är specifikt för fullständig HTTP-svar (sidan cachelagring av utdata).</span><span class="sxs-lookup"><span data-stu-id="763cc-105">This data is specifically for full HTTP responses (page output caching).</span></span> <span data-ttu-id="763cc-106">hello providern ansluts till hello nya utdata cache providern utökningspunkt som introducerades i ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="763cc-106">hello provider plugs into hello new output cache provider extensibility point that was introduced in ASP.NET 4.</span></span>

<span data-ttu-id="763cc-107">toouse hello Redis Utdatacacheprovider först konfigurera din cache och sedan konfigurera ASP.NET-program med hjälp av hello Redis utdata Cache providern NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="763cc-107">toouse hello Redis Output Cache Provider, first configure your cache, and then configure your ASP.NET application using hello Redis Output Cache Provider NuGet package.</span></span> <span data-ttu-id="763cc-108">Det här avsnittet innehåller anvisningar om hur du konfigurerar ditt program toouse hello Redis Utdatacacheprovider.</span><span class="sxs-lookup"><span data-stu-id="763cc-108">This topic provides guidance on configuring your application toouse hello Redis Output Cache Provider.</span></span> <span data-ttu-id="763cc-109">Mer information om hur du skapar och konfigurerar Azure Redis-Cache-instansen finns [skapa ett cacheminne](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="763cc-109">For more information about creating and configuring an Azure Redis Cache instance, see [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

## <a name="store-aspnet-page-output-in-hello-cache"></a><span data-ttu-id="763cc-110">Lagra ASP.NET sidutdata i hello cacheminne</span><span class="sxs-lookup"><span data-stu-id="763cc-110">Store ASP.NET page output in hello cache</span></span>
<span data-ttu-id="763cc-111">tooconfigure ett klientprogram i Visual Studio med hello Redis-Cache Session tillstånd NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.</span><span class="sxs-lookup"><span data-stu-id="763cc-111">tooconfigure a client application in Visual Studio using hello Redis Cache Session State NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>

<span data-ttu-id="763cc-112">Kör hello följande kommando från hello `Package Manager Console` fönster.</span><span class="sxs-lookup"><span data-stu-id="763cc-112">Run hello following command from hello `Package Manager Console` window.</span></span>
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

<span data-ttu-id="763cc-113">Hej Redis utdata Cache providern NuGet-paketet har ett beroende på hello StackExchange.Redis.StrongName paketet.</span><span class="sxs-lookup"><span data-stu-id="763cc-113">hello Redis Output Cache Provider NuGet package has a dependency on hello StackExchange.Redis.StrongName package.</span></span> <span data-ttu-id="763cc-114">Om hello StackExchange.Redis.StrongName paketet inte finns i ditt projekt, installeras.</span><span class="sxs-lookup"><span data-stu-id="763cc-114">If hello StackExchange.Redis.StrongName package is not present in your project, it is installed.</span></span> <span data-ttu-id="763cc-115">Mer information om hello Redis utdata Cache providern NuGet-paketet finns hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-sidan.</span><span class="sxs-lookup"><span data-stu-id="763cc-115">For more information about hello Redis Output Cache Provider NuGet package, see hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet page.</span></span>

>[!NOTE]
><span data-ttu-id="763cc-116">I tillägg toohello starkt krypterat namn StackExchange.Redis.StrongName paket finns det också hello StackExchange.Redis icke-starkt krypterat namn version.</span><span class="sxs-lookup"><span data-stu-id="763cc-116">In addition toohello strong-named StackExchange.Redis.StrongName package, there is also hello StackExchange.Redis non-strong-named version.</span></span> <span data-ttu-id="763cc-117">Om ditt projekt använder hello icke-starkt krypterat namn StackExchange.Redis version måste du avinstallera den, hämta annars du namnkonflikter i projektet.</span><span class="sxs-lookup"><span data-stu-id="763cc-117">If your project is using hello non-strong-named StackExchange.Redis version you must uninstall it, otherwise you get naming conflicts in your project.</span></span> <span data-ttu-id="763cc-118">Mer information om dessa paket finns [konfigurerar .NET-cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span><span class="sxs-lookup"><span data-stu-id="763cc-118">For more information about these packages, see [Configure .NET cache clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).</span></span>
>
>

<span data-ttu-id="763cc-119">Hej NuGet paket hämtar och lägger till hello krävs för sammansättningen refererar till och lägger till hello följande avsnitt i web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="763cc-119">hello NuGet package downloads and adds hello required assembly references and adds hello following section into your web.config file.</span></span> <span data-ttu-id="763cc-120">Det här avsnittet innehåller hello nödvändig konfiguration av din ASP.NET-program toouse hello Redis Utdatacacheprovider.</span><span class="sxs-lookup"><span data-stu-id="763cc-120">This section contains hello required configuration for your ASP.NET application toouse hello Redis Output Cache Provider.</span></span>

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

<span data-ttu-id="763cc-121">hello kommenterats avsnittet innehåller ett exempel på hello attribut och exempel på inställningar för varje attribut.</span><span class="sxs-lookup"><span data-stu-id="763cc-121">hello commented section provides an example of hello attributes and sample settings for each attribute.</span></span>

<span data-ttu-id="763cc-122">Konfigurera hello attribut med hello värden från din cachebladet i hello Microsoft Azure-portalen och konfigurera hello andra värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="763cc-122">Configure hello attributes with hello values from your cache blade in hello Microsoft Azure portal, and configure hello other values as desired.</span></span> <span data-ttu-id="763cc-123">Anvisningar för att komma åt egenskaper för cache finns [konfigurera Redis cacheinställningarna](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="763cc-123">For instructions on accessing your cache properties, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

* <span data-ttu-id="763cc-124">**värden** – ange cache-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="763cc-124">**host** – specify your cache endpoint.</span></span>
* <span data-ttu-id="763cc-125">**port** – använda icke-SSL-port eller SSL-port, beroende på hello ssl-inställningar.</span><span class="sxs-lookup"><span data-stu-id="763cc-125">**port** – use either your non-SSL port or your SSL port, depending on hello ssl settings.</span></span>
* <span data-ttu-id="763cc-126">**accessKey** – använda antingen hello primära och sekundära nycklarna för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="763cc-126">**accessKey** – use either hello primary or secondary key for your cache.</span></span>
* <span data-ttu-id="763cc-127">**SSL** – SANT om du vill toosecure klient och cache-kommunikation med ssl, annars false.</span><span class="sxs-lookup"><span data-stu-id="763cc-127">**ssl** – true if you want toosecure cache/client communications with ssl; otherwise false.</span></span> <span data-ttu-id="763cc-128">Vara säker på att toospecify hello rätt port.</span><span class="sxs-lookup"><span data-stu-id="763cc-128">Be sure toospecify hello correct port.</span></span>
  * <span data-ttu-id="763cc-129">hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="763cc-129">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="763cc-130">Ange true för den här inställningen toouse hello SSL-port.</span><span class="sxs-lookup"><span data-stu-id="763cc-130">Specify true for this setting toouse hello SSL port.</span></span> <span data-ttu-id="763cc-131">Mer information om hur du aktiverar hello icke-SSL-porten finns hello [Åtkomstportar](cache-configure.md#access-ports) avsnitt i hello [konfigurera en cache](cache-configure.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="763cc-131">For more information about enabling hello non-SSL port, see hello [Access Ports](cache-configure.md#access-ports) section in hello [Configure a cache](cache-configure.md) topic.</span></span>
* <span data-ttu-id="763cc-132">**databaseId** – angivna vilken databas toouse för cache-utdata.</span><span class="sxs-lookup"><span data-stu-id="763cc-132">**databaseId** – Specified which database toouse for cache output data.</span></span> <span data-ttu-id="763cc-133">Om inget anges används hello standardvärdet 0.</span><span class="sxs-lookup"><span data-stu-id="763cc-133">If not specified, hello default value of 0 is used.</span></span>
* <span data-ttu-id="763cc-134">**applicationName** – nycklar lagras i redis som `<AppName>_<SessionId>_Data`.</span><span class="sxs-lookup"><span data-stu-id="763cc-134">**applicationName** – Keys are stored in redis as `<AppName>_<SessionId>_Data`.</span></span> <span data-ttu-id="763cc-135">Den här namngivningsschemat gör det möjligt för flera program tooshare hello samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="763cc-135">This naming scheme enables multiple applications tooshare hello same key.</span></span> <span data-ttu-id="763cc-136">Den här parametern är valfri och om du inte anger den ett standardvärde används.</span><span class="sxs-lookup"><span data-stu-id="763cc-136">This parameter is optional and if you do not provide it a default value is used.</span></span>
* <span data-ttu-id="763cc-137">**connectionTimeoutInMilliseconds** – den här inställningen kan toooverride hello connectTimeout i hello StackExchange.Redis klienten.</span><span class="sxs-lookup"><span data-stu-id="763cc-137">**connectionTimeoutInMilliseconds** – This setting allows you toooverride hello connectTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="763cc-138">Om inget anges används hello connectTimeout standardinställningen 5000.</span><span class="sxs-lookup"><span data-stu-id="763cc-138">If not specified, hello default connectTimeout setting of 5000 is used.</span></span> <span data-ttu-id="763cc-139">Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="763cc-139">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>
* <span data-ttu-id="763cc-140">**operationTimeoutInMilliseconds** – den här inställningen kan toooverride hello syncTimeout i hello StackExchange.Redis klienten.</span><span class="sxs-lookup"><span data-stu-id="763cc-140">**operationTimeoutInMilliseconds** – This setting allows you toooverride hello syncTimeout setting in hello StackExchange.Redis client.</span></span> <span data-ttu-id="763cc-141">Om inget anges används hello syncTimeout standardinställningen 1000.</span><span class="sxs-lookup"><span data-stu-id="763cc-141">If not specified, hello default syncTimeout setting of 1000 is used.</span></span> <span data-ttu-id="763cc-142">Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).</span><span class="sxs-lookup"><span data-stu-id="763cc-142">For more information, see [StackExchange.Redis configuration model](http://go.microsoft.com/fwlink/?LinkId=398705).</span></span>

<span data-ttu-id="763cc-143">Lägg till en OutputCache direktivet tooeach sida som du vill att toocache hello utdata.</span><span class="sxs-lookup"><span data-stu-id="763cc-143">Add an OutputCache directive tooeach page for which you wish toocache hello output.</span></span>

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

<span data-ttu-id="763cc-144">I föregående exempel hello hello cachelagras sidan data blir kvar hello-cache under 60 sekunder och en annan version av hello sidan cachelagras för varje parameter-kombination.</span><span class="sxs-lookup"><span data-stu-id="763cc-144">In hello previous example, hello cached page data remains in hello cache for 60 seconds, and a different version of hello page is cached for each parameter combination.</span></span> <span data-ttu-id="763cc-145">Läs mer om hello-direktivet OutputCache [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).</span><span class="sxs-lookup"><span data-stu-id="763cc-145">For more information about hello OutputCache directive, see [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).</span></span>

<span data-ttu-id="763cc-146">När dessa steg utförs är konfigurerade toouse hello Redis Utdatacacheprovider i ditt program.</span><span class="sxs-lookup"><span data-stu-id="763cc-146">Once these steps are performed, your application is configured toouse hello Redis Output Cache Provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="763cc-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="763cc-147">Next steps</span></span>
<span data-ttu-id="763cc-148">Kolla in hello [ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache](cache-aspnet-session-state-provider.md).</span><span class="sxs-lookup"><span data-stu-id="763cc-148">Check out hello [ASP.NET Session State Provider for Azure Redis Cache](cache-aspnet-session-state-provider.md).</span></span>

