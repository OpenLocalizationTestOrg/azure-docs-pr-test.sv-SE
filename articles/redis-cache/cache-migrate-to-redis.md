---
title: aaaMigrate Managed Cache Service program tooRedis - Azure | Microsoft Docs
description: "Lär dig hur toomigrate Managed Cache Service och cachelagring i Rollinstanser program tooAzure Redis-Cache"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: sdanie
ms.openlocfilehash: bd81722820acf0d2637828fbb6100c723aafeba5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-managed-cache-service-tooazure-redis-cache"></a>Migrera från Managed Cache Service tooAzure Redis-Cache
Migrera dina program som använder Azure Managed Cache Service tooAzure Redis-Cache kan åstadkommas med minimala ändringar tooyour program, beroende på hello Managed Cache Service funktioner som används av tillämpningsprogrammet cachelagring. Medan hello API: er är inte exakt hello samma som de är liknande och en stor del av din befintliga kod som använder Managed Cache Service tooaccess en cache kan återanvändas med minimala ändringar. Det här avsnittet visar hur toomake hello nödvändig konfiguration och program ändras toomigrate din Managed Cache Service program toouse Azure Redis-Cache och visar hur några av hello funktionerna i Azure Redis-Cache kan vara används tooimplement hello funktioner en Managed Cache Service-cache.

>[!NOTE]
>Hanterade Cache-tjänst och cachelagring i Rollinstanser har [dragits tillbaka](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30 November 2016. Om du har alla distributioner för cachelagring i Rollinstanser som du vill toomigrate tooAzure Redis-Cache kan följa du hello stegen i den här artikeln.

## <a name="migration-steps"></a>Migreringssteg
hello följande steg är nödvändiga toomigrate en Managed Cache Service programmet toouse Azure Redis-Cache.

* Mappa Managed Cache Service funktioner tooAzure Redis-Cache
* Välj ett erbjudande för Cache
* Skapa ett cacheminne
* Konfigurera hello Cache-klienter
  * Ta bort hello Managed Cache Service Configuration
  * Konfigurera en cacheklient som använder hello StackExchange.Redis NuGet-paketet
* Migrera koden Managed Cache Service
  * Ansluta toohello cacheminne med hjälp av hello ConnectionMultiplexer klass
  * Åtkomst primitiva datatyper i hello cache
  * Arbeta med .NET-objekt i hello cache
* Migrera sessionstillståndet ASP.NET och Utdatacachelagring tooAzure Redis-Cache 

## <a name="map-managed-cache-service-features-tooazure-redis-cache"></a>Mappa Managed Cache Service funktioner tooAzure Redis-Cache
Azure Managed Cache Service och Azure Redis-Cache är samma men implementerar en del av deras funktioner på olika sätt. Det här avsnittet beskrivs några av skillnaderna hello och vägledning om hur du implementerar hello funktionerna i Managed Cache Service i Azure Redis-Cache.

| Funktionen för hanterade Cache Service | Stöd för hanterade Cache Service | Stöd för Azure Redis-Cache |
| --- | --- | --- |
| Namngivna cacheminnen |En standard-cache är konfigurerad och hello Standard och Premium cache erbjudanden, in toonine ytterligare med namnet cacheminnen kan konfigureras om så önskas. |Azure Redis-cache har en konfigurerbar antalet databaser (standard 16) som kan använda tooimplement en liknande funktionalitet toonamed cachelagrar. Mer information finns i [What are Redis databases?](cache-faq.md#what-are-redis-databases) (Vad är Redis-databaser?) och [Default Redis server configuration](cache-configure.md#default-redis-server-configuration) (Standardkonfiguration av Redis-server). |
| Hög tillgänglighet |Ger hög tillgänglighet för objekt i hello-cachen i hello Standard och Premium cache-erbjudanden. Om objekt går förlorade på grund av fel tooa är säkerhetskopior av hello objekt i hello cache fortfarande tillgängliga. Skriver toohello sekundära cache görs synkront. |Hög tillgänglighet finns i hello Standard och Premium cache erbjudanden, som har en konfiguration för primär/replik av två noder (varje Fragmentera i Premium-cache har två primära/replik). Skrivningar toohello repliken görs asynkront. Mer information finns i [priser för Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/). |
| Meddelanden |Låter klienter tooreceive asynkrona meddelanden när en mängd olika cache-åtgärder sker på en namngiven cache. |Klientprogram kan använda Redis pub/sub eller [Keyspace-meddelanden](cache-configure.md#keyspace-notifications-advanced-settings) tooachieve en liknande funktionalitet toonotifications. |
| Lokal cache |Lagrar en kopia av cachelagrade objekt lokalt på hello klienten för extra snabb åtkomst. |Klientprogram måste tooimplement funktionen med hjälp av en ordlista eller liknande datastruktur. |
| Princip för borttagning |Ingen eller LRU. hello standardprincipen är LRU. |Hello efter borttagning principer har stöd för Azure Redis-Cache: obeständigt lru, allkeys lru, obeständigt slumpmässiga, allkeys slumpmässiga obeständigt-TTL-värde, noeviction. hello standardprincipen är obeständigt lru. Mer information finns i [standard Redis serverkonfiguration](cache-configure.md#default-redis-server-configuration). |
| Princip |hello standardprincip är absolut hello standardintervallet för förfallotid är tio minuter. Det finns också glidande och aldrig principer. |Som standard förfaller inte objekt i hello cache, men ett förfallodatum kan konfigureras på grundval av per skrivning med hjälp av cachen set överlagringar. Mer information finns i [Lägg till och hämta objekt från cache hello](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache). |
| Regioner och taggning |Regioner är undergrupper för cachelagrade objekt. Regioner stöder också hello anteckningen cachelagrade objekt med ytterligare beskrivande strängar som kallas taggar. Regioner stöder hello möjlighet tooperform sökningar på taggade objekt i den regionen. Alla objekt i en region finns inom en enskild nod hello cache-klustret. |Ett Redis-cache består av en enskild nod (om Redis cluster är aktiverad) så hello begreppet Managed Cache Service regioner inte är giltigt. Redis har stöd för sökning och jokertecken åtgärder vid hämtning av nycklar så beskrivande taggar kan vara inbäddat i hello nyckelnamn och användas tooretrieve hello objekt senare. Ett exempel på att implementera en märkning lösning med Redis finns [implementera märkning med Redis cache](http://stackify.com/implementing-cache-tagging-redis/). |
| Serialisering |Managed Cache stöder NetDataContractSerializer och BinaryFormatter hello användningen av anpassade serializers. hello standardvärdet är NetDataContractSerializer. |Det är hello ansvar hello klienten programmet tooserialize .NET objekt innan du monterar dem till hello cache med hello val av hello serialiseraren in toohello programutvecklaren för klienten. Mer information och exempelkod finns [arbeta med .NET-objekt i hello cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Cache-emulatorn |Hanterade cachen innehåller en lokal cache-emulatorn. |Azure Redis-Cache har inte en emulator, men du kan [kör hello MSOpenTech version av redis-server.exe lokalt](cache-faq.md#cache-emulator) tooprovide en emulator upplevelse. |

## <a name="choose-a-cache-offering"></a>Välj ett erbjudande för Cache
Microsoft Azure Redis-Cache är tillgängliga i hello följande nivåer:

* **Grundläggande** – En nod. Flera storlekar upp too53 GB.
* **Standard** – Primär/replik med två noder. Flera storlekar upp too53 GB. 99,9 % SLA.
* **Premium** – två noder primära/replik med upp too10 delar. Flera storlekar från 6 GB too530 GB. Alla standardnivåfunktioner med mera, med stöd för [Redis-kluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md) och [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA.

Varje nivå har olika funktioner och priser. hello funktioner beskrivs senare i den här guiden och mer information om priser finns i avsnittet [Cache-prisinformation](https://azure.microsoft.com/pricing/details/cache/).

En startpunkt för migrering är toopick hello storlek som matchar hello storlek tidigare Managed Cache Service och sedan skala upp eller ned beroende på hello krav för programmet. Mer information om att välja hello rätt Azure Redis-Cache erbjudande finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Skapa ett cacheminne
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-hello-cache-clients"></a>Konfigurera hello Cache-klienter
När hello cachen har skapats och konfigurerats, hello nästa steg är tooremove hello Managed Cache Service configuration och lägga till hello lägga till hello Azure Redis-Cache konfiguration och referenser så att cacheklienter kan komma åt hello cache.

* Ta bort hello Managed Cache Service Configuration
* Konfigurera en cacheklient som använder hello StackExchange.Redis NuGet-paketet

### <a name="remove-hello-managed-cache-service-configuration"></a>Ta bort hello Managed Cache Service Configuration
Innan du hello klientprogram kan konfigureras för Azure Redis-Cache, hello befintliga Managed Cache Service-konfigurationen och sammansättningsreferenser måste tas bort genom att avinstallera hello Managed Cache Service NuGet-paketet.

toouninstall hello Managed Cache Service NuGet-paketet, högerklicka på hello klientprojektet i **Solution Explorer** och välj **hantera NuGet-paket**. Välj hello **installerade paket** nod och typen W**indowsAzure.Caching** till hello installerat paket rutan. Välj **Windows** **Azure Cache** (eller **Windows** **Azure Caching** beroende på hello version av hello NuGet-paketet), klickar du på  **Avinstallera**, och klicka sedan på **Stäng**.

![Avinstallera NuGet-paketet för Azure Managed Cache Service](./media/cache-migrate-to-redis/IC757666.jpg)

Avinstallera hello Managed Cache Service NuGet-paketet tar bort hello Managed Cache Service sammansättningar och hello Managed Cache Service poster i hello app.config eller web.config i hello-klientprogrammet. Eftersom vissa anpassade inställningar tas inte bort när du avinstallerar hello NuGet-paketet, öppna web.config eller app.config och kontrollera att hello följande element har tagits bort.

Se till att hello `dataCacheClients` tas bort från hello `configSections` element. Ta inte bort hello hela `configSections` elementet; bara ta bort hello `dataCacheClients` post, om den finns.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Se till att hello `dataCacheClients` avsnittet tas bort. Hej `dataCacheClients` avsnittet kommer att liknande toohello följande exempel.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--toouse hello in-role flavor of Azure Cache, set identifier toobe hello cache cluster role name -->
    <!--toouse hello Azure Managed Cache Service, set identifier toobe hello endpoint of hello cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section toospecify security settings for connecting tooyour cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

När hello Managed Cache Service configuration har tagits bort, kan du konfigurera hello cacheklienten enligt beskrivningen i följande avsnitt hello.

### <a name="configure-a-cache-client-using-hello-stackexchangeredis-nuget-package"></a>Konfigurera en cacheklient som använder hello StackExchange.Redis NuGet-paketet
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Migrera koden Managed Cache Service
hello API för hello StackExchange.Redis cache-klienten är liknande toohello Managed Cache Service. Det här avsnittet innehåller en översikt över hello skillnader.

### <a name="connect-toohello-cache-using-hello-connectionmultiplexer-class"></a>Ansluta toohello cacheminne med hjälp av hello ConnectionMultiplexer klass
I Managed Cache Service anslutningar toohello cache hanterades av hello `DataCacheFactory` och `DataCache` klasser. I Azure Redis-Cache dessa anslutningar som hanteras av hello `ConnectionMultiplexer` klass.

Lägg till hello följande med instruktionen toohello upp i en fil som du vill tooaccess hello cache.

```c#
using StackExchange.Redis
```

Om inte går att lösa det här namnområdet är det viktigt att du har lagt till hello StackExchange.Redis NuGet-paketet enligt beskrivningen i [konfigurera hello cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

> [!NOTE]
> Observera att hello StackExchange.Redis klienten kräver .NET Framework 4 eller senare.
> 
> 

tooconnect tooan Azure Redis-cacheinstansen, anrop hello statiskt `ConnectionMultiplexer.Connect` metoden och skicka hello slutpunkt och nyckel. En metod som toosharing en `ConnectionMultiplexer` instans i ditt program är toohave en statisk egenskap som returnerar en ansluten instans, liknande toohello följande exempel. Detta ger ett trådsäkert sätt tooinitialize ett enda anslutet `ConnectionMultiplexer` instans. I det här exemplet `abortConnect` är uppsättningen toofalse, vilket innebär att hello anrop lyckas även om en toohello cache för anslutningen inte upprättas. En nyckelfunktion i `ConnectionMultiplexer` är att det automatiskt återställer anslutningen toohello cache när hello nätverksfel eller andra orsaker är löst.

```c#
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

hello cache-slutpunkten, nycklar och portar kan hämtas från hello **Redis-Cache** bladet för din cache-instans. Mer information finns i [Redis-Cache egenskaper](cache-configure.md#properties).

När hello anslutning har upprättats kan returnera en referens toohello Redis cache-databas genom att anropa hello `ConnectionMultiplexer.GetDatabase` metod. hello-objektet som returnerades från hello `GetDatabase` metoden är ett enkelt direkt objekt och behöver inte toobe lagras.

```c#
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using hello cache object...
// Simple put of integral data types into hello cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from hello cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

Hej StackExchange.Redis klienten använder hello `RedisKey` och `RedisValue` typer för att komma åt och lagra objekt i hello-cachen. Dessa typer av mappar till mest primitiva språktyper, inklusive sträng, och ofta används inte direkt. Redis-strängar är hello enklaste typen av Redis värde och kan innehålla flera typer av data, inklusive serialiserade binära dataströmmar och du kan inte använda hello typ direkt, du använder metoder som innehåller `String` i hello namn. För de flesta primitiva datatyper du lagra och hämta objekt från hello cacheminne med hjälp av hello `StringSet` och `StringGet` metoder, om du lagrar samlingar eller andra Redis-datatyper i hello-cachen. 

`StringSet`och `StringGet` är mycket lik toohello Managed Cache Service `Put` och `Get` metoder med en större skillnad är att innan du ställer in och få en .NET-objekt till cache hello du måste serialisera först. 

När du anropar `StringGet`om hello objektet finns returneras och om det inte är null returneras. I det här fallet kan du hämta hello värdet från hello önskad datakälla och lagra den på hello cache för senare användning. Detta kallas hello cache-reservera mönster.

toospecify hello giltighetstid för ett objekt i hello cache, Använd hello `TimeSpan` -parametern för `StringSet`.

```c#
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis-Cache kan arbeta med .NET-objekt som primitiva datatyper, men måste serialiseras innan ett .NET-objekt som kan cachelagras. Detta är hello programutvecklaren hello ansvar. Detta ger hello utvecklare möjlighet hello valet av hello serialiserare. Mer information och exempelkod finns [arbeta med .NET-objekt i hello cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-tooazure-redis-cache"></a>Migrera sessionstillståndet ASP.NET och Utdatacachelagring tooAzure Redis-Cache
Azure Redis-Cache har providers för både sessionstillståndet ASP.NET och cachelagringen av sidan. toomigrate ditt program som använder hello Managed Cache Service versioner av dessa providers först ta bort hello befintliga avsnitt från filen web.config och sedan konfigurera hello Azure Redis-Cache versioner av hello providers. För instruktioner om hur du använder hello Azure Redis-Cache ASP.NET-providers, se [ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache](cache-aspnet-session-state-provider.md) och [ASP.NET Utdatacacheprovider för Azure Redis-Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Nästa steg
Utforska hello [dokumentation för Azure Redis-Cache](https://azure.microsoft.com/documentation/services/cache/) för självstudier, exempel, videor och mycket mer.

