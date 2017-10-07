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
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET Utdatacacheprovider för Azure Redis-Cache
Hej Redis Utdatacacheprovider är en mekanism för lagring av out-of-process för cache-utdata. Informationen är specifikt för fullständig HTTP-svar (sidan cachelagring av utdata). hello providern ansluts till hello nya utdata cache providern utökningspunkt som introducerades i ASP.NET 4.

toouse hello Redis Utdatacacheprovider först konfigurera din cache och sedan konfigurera ASP.NET-program med hjälp av hello Redis utdata Cache providern NuGet-paketet. Det här avsnittet innehåller anvisningar om hur du konfigurerar ditt program toouse hello Redis Utdatacacheprovider. Mer information om hur du skapar och konfigurerar Azure Redis-Cache-instansen finns [skapa ett cacheminne](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-hello-cache"></a>Lagra ASP.NET sidutdata i hello cacheminne
tooconfigure ett klientprogram i Visual Studio med hello Redis-Cache Session tillstånd NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.

Kör hello följande kommando från hello `Package Manager Console` fönster.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Hej Redis utdata Cache providern NuGet-paketet har ett beroende på hello StackExchange.Redis.StrongName paketet. Om hello StackExchange.Redis.StrongName paketet inte finns i ditt projekt, installeras. Mer information om hello Redis utdata Cache providern NuGet-paketet finns hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet-sidan.

>[!NOTE]
>I tillägg toohello starkt krypterat namn StackExchange.Redis.StrongName paket finns det också hello StackExchange.Redis icke-starkt krypterat namn version. Om ditt projekt använder hello icke-starkt krypterat namn StackExchange.Redis version måste du avinstallera den, hämta annars du namnkonflikter i projektet. Mer information om dessa paket finns [konfigurerar .NET-cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Hej NuGet paket hämtar och lägger till hello krävs för sammansättningen refererar till och lägger till hello följande avsnitt i web.config-filen. Det här avsnittet innehåller hello nödvändig konfiguration av din ASP.NET-program toouse hello Redis Utdatacacheprovider.

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

hello kommenterats avsnittet innehåller ett exempel på hello attribut och exempel på inställningar för varje attribut.

Konfigurera hello attribut med hello värden från din cachebladet i hello Microsoft Azure-portalen och konfigurera hello andra värden efter behov. Anvisningar för att komma åt egenskaper för cache finns [konfigurera Redis cacheinställningarna](cache-configure.md#configure-redis-cache-settings).

* **värden** – ange cache-slutpunkten.
* **port** – använda icke-SSL-port eller SSL-port, beroende på hello ssl-inställningar.
* **accessKey** – använda antingen hello primära och sekundära nycklarna för ditt cacheminne.
* **SSL** – SANT om du vill toosecure klient och cache-kommunikation med ssl, annars false. Vara säker på att toospecify hello rätt port.
  * hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen. Ange true för den här inställningen toouse hello SSL-port. Mer information om hur du aktiverar hello icke-SSL-porten finns hello [Åtkomstportar](cache-configure.md#access-ports) avsnitt i hello [konfigurera en cache](cache-configure.md) avsnittet.
* **databaseId** – angivna vilken databas toouse för cache-utdata. Om inget anges används hello standardvärdet 0.
* **applicationName** – nycklar lagras i redis som `<AppName>_<SessionId>_Data`. Den här namngivningsschemat gör det möjligt för flera program tooshare hello samma nyckel. Den här parametern är valfri och om du inte anger den ett standardvärde används.
* **connectionTimeoutInMilliseconds** – den här inställningen kan toooverride hello connectTimeout i hello StackExchange.Redis klienten. Om inget anges används hello connectTimeout standardinställningen 5000. Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – den här inställningen kan toooverride hello syncTimeout i hello StackExchange.Redis klienten. Om inget anges används hello syncTimeout standardinställningen 1000. Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).

Lägg till en OutputCache direktivet tooeach sida som du vill att toocache hello utdata.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

I föregående exempel hello hello cachelagras sidan data blir kvar hello-cache under 60 sekunder och en annan version av hello sidan cachelagras för varje parameter-kombination. Läs mer om hello-direktivet OutputCache [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

När dessa steg utförs är konfigurerade toouse hello Redis Utdatacacheprovider i ditt program.

## <a name="next-steps"></a>Nästa steg
Kolla in hello [ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache](cache-aspnet-session-state-provider.md).

