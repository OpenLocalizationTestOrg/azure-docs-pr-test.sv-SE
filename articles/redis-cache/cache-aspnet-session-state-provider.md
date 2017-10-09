---
title: "aaaCache ASP.NET-Sessionstillståndsprovider | Microsoft Docs"
description: "Lär dig hur toostore ASP.NET Session tillstånd med hjälp av Azure Redis-Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: sdanie
ms.openlocfilehash: 9ea84cf67b9314b15dce696f596d399921194510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Provider av ASP.NET-sessionstillstånd för Azure Redis-cache
Azure Redis-Cache ger en sessionstillståndsprovider för att du kan använda toostore din sessionstillstånd i en cache i stället för i minnet eller i en SQL Server-databas. toouse hello sessionstillståndsprovider för cachelagring, först konfigurera din cache och sedan konfigurera ASP.NET-program för cache med hello Redis-Cache Session tillstånd NuGet-paketet.

Är det ofta inte praktiska i en verklig cloud app tooavoid lagra någon form av tillståndet för en användarsession, men vissa metoder påverkar prestanda och skalbarhet mer än andra. Om du har toostore tillstånd hello bästa lösningen är tookeep hello mängden tillstånd små och lagra den på cookies. Om det är inte möjligt är hello nästa bästa lösningen toouse ASP.NET session state med en provider för distribuerade, InMemory-cachen. hello sämsta lösning från en prestanda och skalbarhet synvinkel är toouse en databas säkerhetskopieras sessionstillståndsprovider. Det här avsnittet innehåller riktlinjer om hur du använder hello ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache. Information om andra alternativ för sessionen tillstånd finns [sessionstillståndet ASP.NET alternativ](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-hello-cache"></a>Lagra sessionstillstånd för ASP.NET i hello cache
tooconfigure ett klientprogram i Visual Studio med hello Redis-Cache Session tillstånd NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.

Kör hello följande kommando från hello `Package Manager Console` fönster.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Om du använder hello funktionen kluster från hello premium-nivån, måste du använda [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 eller högre eller ett undantagsfel genereras. Flytta too2.0.1 eller högre är en ny ändring; Mer information finns i [v2.0.0 information om ändringar i bryter](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). Hello nuvarande version av det här paketet är 2.2.3 samtidigt hello av den här artikeln uppdateringen.
> 
> 

Hej Redis Session tillstånd providern NuGet-paketet har ett beroende på hello StackExchange.Redis.StrongName paketet. Om hello StackExchange.Redis.StrongName paketet inte finns i ditt projekt, installeras.

>[!NOTE]
>I tillägg toohello starkt krypterat namn StackExchange.Redis.StrongName paket finns det också hello StackExchange.Redis icke-starkt krypterat namn version. Om ditt projekt använder hello icke-starkt krypterat namn StackExchange.Redis version måste du avinstallera den, hämta annars du namnkonflikter i projektet. Mer information om dessa paket finns [konfigurerar .NET-cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

Hej NuGet paket hämtar och lägger till hello krävs för sammansättningen refererar till och lägger till hello följande avsnitt i web.config-filen. Det här avsnittet innehåller hello nödvändig konfiguration av din ASP.NET-program toouse hello Redis-Cache Sessionstillståndsprovider.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

hello kommenterats avsnittet innehåller ett exempel på hello attribut och exempel på inställningar för varje attribut.

Konfigurera hello attribut med hello värden från din cachebladet i hello Microsoft Azure-portalen och konfigurera hello andra värden efter behov. Anvisningar för att komma åt egenskaper för cache finns [konfigurera Redis cacheinställningarna](cache-configure.md#configure-redis-cache-settings).

* **värden** – ange cache-slutpunkten.
* **port** – använda icke-SSL-port eller SSL-port, beroende på hello ssl-inställningar.
* **accessKey** – använda antingen hello primära och sekundära nycklarna för ditt cacheminne.
* **SSL** – SANT om du vill toosecure klient och cache-kommunikation med ssl, annars false. Vara säker på att toospecify hello rätt port.
  * hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen. Ange true för den här inställningen toouse hello SSL-port. Mer information om hur du aktiverar hello icke-SSL-porten finns hello [Åtkomstportar](cache-configure.md#access-ports) avsnitt i hello [konfigurera en cache](cache-configure.md) avsnittet.
* **throwOnError** – SANT om du vill att ett undantag toobe genereras om det finns ett fel eller false om du vill hello åtgärden toofail tyst. Du kan söka efter ett fel genom att kontrollera hello statisk Microsoft.Web.Redis.RedisSessionStateProvider.LastException egenskap. hello standardvärdet är true.
* **retryTimeoutInMilliseconds** – åtgärder som misslyckas igen under den här intervall som anges i millisekunder. hello första återförsök görs efter 20 millisekunder och sedan återförsök sker varje sekund tills hello retryTimeoutInMilliseconds intervallet har gått ut. Omedelbart efter det här intervallet försöks hello åtgärden en sista gång. Om hello åtgärden fortfarande misslyckas undantag hello tillbaka toohello anroparen, beroende på hello throwOnError inställningen. hello standardvärdet är 0, vilket innebär att inga nya försök.
* **databaseId** – anger vilken databas toouse för cache-utdata. Om inget anges används hello standardvärdet 0.
* **applicationName** – nycklar lagras i redis som `{<Application Name>_<Session ID>}_Data`. Den här namngivningsschemat gör det möjligt för flera program tooshare hello samma Redis-instans. Den här parametern är valfri och om du inte anger den ett standardvärde används.
* **connectionTimeoutInMilliseconds** – den här inställningen kan toooverride hello connectTimeout i hello StackExchange.Redis klienten. Om inget anges används hello connectTimeout standardinställningen 5000. Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – den här inställningen kan toooverride hello syncTimeout i hello StackExchange.Redis klienten. Om inget anges används hello syncTimeout standardinställningen 1000. Mer information finns i [StackExchange.Redis Konfigurationsmodell](http://go.microsoft.com/fwlink/?LinkId=398705).

Mer information om de här egenskaperna finns hello ursprungliga blogginlägget vid [om ASP.NET Sessionstillståndsprovider för Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Glöm inte toocomment ut hello standard InProc session tillstånd providern avsnitt i filen web.config.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

När dessa steg utförs är konfigurerade toouse hello Redis-Cache Sessionstillståndsprovider för ditt program. När du använder sessionstillstånd i ditt program, är den lagrad i en Azure Redis-Cache-instans.

> [!IMPORTANT]
> Data som lagras i cacheminnet hello måste kunna serialiseras, till skillnad från hello data som kan lagras i hello i minnet ASP.NET-Sessionstillståndsprovider som standard. När hello Sessionstillståndsprovider för Redis används, se till att hello-datatyper som lagras i sessionens tillstånd är serialiseras.
> 
> 

## <a name="aspnet-session-state-options"></a>Sessionstillståndet ASP.NET-alternativ
* Den här providern lagrar minne Sessionstillståndsprovider - hello sessionstillstånd i minnet. hello fördelen med att använda den här providern är det är snabbt och enkelt. Men du kan skala Web Apps om du använder i minnet providern eftersom den inte distribueras.
* SQL Server-Sessionstillståndsprovider - providern lagrar hello sessionstillstånd i Sql Server. Använd den här providern om du vill toostore hello sessionstillstånd i permanent lagringsutrymme. Du kan skala ditt webbprogram, men använder Sql Server för sessionen har påverka prestanda på ditt webbprogram.
* Distribuerade i minnet Sessionstillståndsprovider som Redis-Cache Sessionstillståndsprovider - providern kan du hello bäst hos båda. Ditt webbprogram kan ha en enkel, snabb och skalbar Sessionstillståndsprovider. Eftersom den här providern lagrar hello sessionstillstånd i en Cache, din app har tootake hänsyn hello alla egenskaper när man talar tooa distribuerade i Cache-minnet, till exempel tillfälliga nätverksfel. Metodtips om hur du använder Cache finns [cachelagring vägledning](../best-practices-caching.md) från Microsoft Patterns & Practices [Azure Cloud programdesign och implementering vägledning](https://github.com/mspnp/azure-guidance).

Mer information om sessionens tillstånd och andra metodtips finns [Web Development Best Practices (skapa verkliga Molnappar med Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Nästa steg
Kolla in hello [ASP.NET Utdatacacheprovider för Azure Redis-Cache](cache-aspnet-output-cache-provider.md).

