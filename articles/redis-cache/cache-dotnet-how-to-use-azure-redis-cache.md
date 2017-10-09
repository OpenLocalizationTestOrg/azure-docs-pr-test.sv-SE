---
title: aaaHow tooUse Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooimprove hello prestandan för din Azure-program med Azure Redis-Cache"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 763d70c10972eec9a1885969e8da5bf1c4084727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache"></a>Hur tooUse Azure Redis-Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Den här guiden visar hur tooget igång med **Azure Redis-Cache**. Microsoft Azure Redis-Cache är baserad på hello populära öppen källkod Redis-Cache. Den ger dig åtkomst till tooa säker, dedikerad Redis-cache, hanteras av Microsoft. Ett cacheminne som har skapats med Azure Redis Cache är tillgängligt från alla program i Microsoft Azure.

Microsoft Azure Redis-Cache är tillgängliga i hello följande nivåer:

* **Grundläggande** – En nod. Flera storlekar upp too53 GB.
* **Standard** – Primär/replik med två noder. Flera storlekar upp too53 GB. 99,9 % SLA.
* **Premium** – två noder primära/replik med upp too10 delar. Flera storlekar från 6 GB too530 GB. Alla standardnivåfunktioner med mera, med stöd för [Redis-kluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md) och [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 % SLA.

Varje nivå har olika funktioner och priser. Mer information om priser finns i [prisinformationen för Cache][Cache Pricing Details].

Den här guiden visar hur toouse hello [StackExchange.Redis] [ StackExchange.Redis] klienten med hjälp av C\# kod. hello beskrivs scenarier där **skapar och konfigurerar en cache**, **konfigurera cacheklienter**, och **att lägga till och ta bort objekt från cache hello**. Mer information om hur du använder Azure Redis Cache finns i [Nästa steg][Next Steps]. Stegvisa självstudier för att skapa en ASP.NET MVC-webbapp med Redis-Cache finns [hur toocreate ett webbprogram med Redis-Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Kom igång med Azure Redis Cache
Att komma igång med Azure Redis Cache är enkelt. tooget igång kan du etablera och konfigurera en cache. Därefter konfigurerar du hello cache-klienter så att de kan komma åt hello cache. Du kan börja arbeta med dem. när hello cacheklienter har konfigurerats.

* [Skapa hello-cache][Create hello cache]
* [Konfigurera hello cache-klienter][Configure hello cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Skapa en cache
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a>tooaccess ditt cacheminne efter att det har skapats
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Mer information om hur du konfigurerar ditt cacheminne finns [hur tooconfigure Azure Redis-Cache](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a>Konfigurera hello cache-klienter
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Du kan använda hello tekniker som beskrivs i följande avsnitt för att arbeta med ditt cacheminne hello när klientprojektet är konfigurerad för cachelagring.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Arbeta med cacheminnen
hello steg i det här avsnittet beskriver hur tooperform vanliga uppgifter med Cache.

* [Ansluta toohello cache][Connect toohello cache]
* [Lägg till och hämta objekt från hello cache][Add and retrieve objects from hello cache]
* [Arbeta med .NET-objekt i hello cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a>Ansluta toohello cache
tooprogrammatically arbete med en cache, behöver du en referens toohello cache. Lägg till hello följande toohello upp en fil som du vill toouse hello StackExchange.Redis klienten tooaccess ett Azure Redis-Cache.

    using StackExchange.Redis;

> [!NOTE]
> Hej StackExchange.Redis klienten kräver .NET Framework 4 eller senare.
> 
> 

Hej anslutning toohello Azure Redis-Cache hanteras av hello `ConnectionMultiplexer` klass. Den här klassen bör delas återanvändas i hela ditt klientprogram och behöver inte toobe som skapats på grundval av per igen. 

tooconnect tooan Azure Redis-Cache och returneras en instans av en ansluten `ConnectionMultiplexer`, anrop hello statiska `Connect` metoden och skicka hello cachelagra slutpunkt och nyckel. Använd hello nyckeln genereras från hello Azure-portalen som hello lösenord parameter.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Varning: Lagra aldrig autentiseringsuppgifterna i källkoden. tookeep det här exemplet enkel jag visar dem i hello källkoden. Se [hur programmet strängar och anslutningen strängar arbete] [ How Application Strings and Connection Strings Work] information om hur toostore autentiseringsuppgifter.
> 
> 

Om du inte vill toouse SSL kan antingen ange `ssl=false` eller utelämna hello `ssl` parameter.

> [!NOTE]
> hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen. Anvisningar om hur du aktiverar hello icke-SSL-porten finns [Åtkomstportar](cache-configure.md#access-ports).
> 
> 

En metod som toosharing en `ConnectionMultiplexer` instans i ditt program är toohave en statisk egenskap som returnerar en ansluten instans, liknande toohello följande exempel. Den här metoden ger ett trådsäkert sätt tooinitialize ett enda anslutet `ConnectionMultiplexer` instans. I de här exemplen `abortConnect` är uppsättningen toofalse, vilket innebär att hello anropet lyckas även om det inte är att upprätta en anslutning toohello Azure Redis-Cache. En nyckelfunktion i `ConnectionMultiplexer` är att automatiskt återställs anslutningen toohello cache när hello nätverksfel eller andra orsaker är löst.

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

Mer information om avancerade alternativ för anslutningskonfiguration finns i [Konfigurationsmodell för StackExchange.Redis][StackExchange.Redis configuration model].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

När hello anslutning har upprättats kan returnera en referens toohello redis cache-databas genom att anropa hello `ConnectionMultiplexer.GetDatabase` metod. hello-objektet som returnerades från hello `GetDatabase` metoden är ett enkelt direkt objekt och behöver inte toobe lagras.

    // Connection refers tooa property that returns a ConnectionMultiplexer
    // as shown in hello previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using hello cache object...
    // Simple put of integral data types into hello cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from hello cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Azure Redis-cache har en konfigurerbar antalet databaser (standard 16) som kan använda toologically separat hello data i ett Redis-cache. Mer information finns i [What are Redis databases?](cache-faq.md#what-are-redis-databases) (Vad är Redis-databaser?) och [Default Redis server configuration](cache-configure.md#default-redis-server-configuration) (Standardkonfiguration av Redis-server).

Nu när du vet hur tooconnect tooan Azure Redis-Cache-instansen och returnerar en referens toohello cachelagra databasen ska vi titta på arbeta med hello cache.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a>Lägg till och hämta objekt från hello cache
Objekt som kan lagras i och hämtas från en cache med hjälp av hello `StringSet` och `StringGet` metoder.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis lagrar de flesta data som Redis strängar, men dessa strängar kan innehålla flera typer av data, inklusive serialiserade binära data som kan användas när lagra .NET objekt i hello-cachen.

När du anropar `StringGet`, om det inte finns hello objekt returneras, och om inte `null` returneras. Om `null` returneras kan du hämta hello värdet från hello önskad datakälla och lagra den på hello cache för senare användning. Den här användningsmönstret kallas hello cache-reservera mönster.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Du kan också använda `RedisValue`som visas i följande exempel hello. `RedisValue` har implicita operatorer för att hantera integrala datatyper och kan vara användbart om `null` är ett förväntat värde för ett cachelagrat objekt.


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


toospecify hello giltighetstid för ett objekt i hello cache, Använd hello `TimeSpan` -parametern för `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a>Arbeta med .NET-objekt i hello cache
Azure Redis Cache kan cachelagra både .NET-objekt och basdatatyper, men .NET-objekt måste serialiseras innan de kan cachelagras. Det här objektet .NET-serialisering är hello programutvecklaren hello ansvar och ger hello developer flexibilitet i hello valet av hello serialiseraren.

Ett enkelt sätt tooserialize objekt är toouse hello `JsonConvert` serialisering metoder i [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) och serialisera tooand från JSON. hello följande exempel visas en get och set med hjälp av en `Employee` objektinstansen.

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store toocache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna i hello följa dessa länkar toolearn mer om Azure Redis-Cache.

* Checka ut hello ASP.NET-providers för Azure Redis-Cache.
  * [Sessionstillståndsprovider för Azure Redis](cache-aspnet-session-state-provider.md)
  * [Utdatacacheprovider för Azure Redis Cache ASP.NET](cache-aspnet-output-cache-provider.md)
* [Aktivera cache diagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervakaren](cache-how-to-monitor.md) hello hälsotillståndet för ditt cacheminne. Du kan visa hello mått i hello Azure-portalen och du kan också [ladda ned och granska](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) dem med hello-verktyg.
* Kolla in hello [StackExchange.Redis cache klienten dokumentationen][StackExchange.Redis cache client documentation].
  * Azure Redis Cache kan nås från många Redis-klienter och programmeringsspråk. Mer information finns på [http://redis.io/clients][http://redis.io/clients].
* Azure Redis Cache kan också användas med tjänster och verktyg från tredje part som Redsmin och Redis Desktop Manager.
  * Läs mer om Redsmin [hur tooretrieve en anslutning för Azure Redis-sträng och använda den med Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].
  * Komma åt och granska dina data i Azure Redis Cache med ett grafiskt användargränssnitt med hjälp av [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
* Se hello [redis] [ redis] dokumentation och Läs om [redis datatyper] [ redis data types] och [en introduktion 15 minut tooRedis datatyper][a fifteen minute introduction tooRedis data types].

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction tooAzure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project tooUse Azure Caching]: #prepare-vs
[Configure Your Application tooUse Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create hello cache]: #create-cache
[Configure hello cache]: #enable-caching
[Configure hello cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect toohello cache]: #connect-to-cache
[Add and retrieve objects from hello cache]: #add-object
[Specify hello expiration of an object in hello cache]: #specify-expiration
[Store ASP.NET session state in hello cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How tooretrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How tooConfigure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set hello Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in hello cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate tooAzure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups toomanage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction tooRedis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


