---
title: aaaHow tootroubleshoot Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooresolve vanliga problem med Azure Redis-Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: 4e736fce2b6d5200a2a8d802f3f1384b63458cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-azure-redis-cache"></a>Hur tootroubleshoot Azure Redis-Cache
Den här artikeln innehåller riktlinjer för felsökning hello följande typer av problem med Azure Redis-Cache.

* [Felsökning på klienten](#client-side-troubleshooting) – det här avsnittet innehåller riktlinjer för att identifiera och lösa problem orsakade av hello program ansluter tooAzure Redis-Cache.
* [Felsökning på Server](#server-side-troubleshooting) – det här avsnittet innehåller riktlinjer för att identifiera och lösa problem orsakade på hello Azure Redis-Cache på serversidan.
* [StackExchange.Redis tidsgränsfel](#stackexchangeredis-timeout-exceptions) – det här avsnittet innehåller information om felsökning av problem när du använder hello StackExchange.Redis klienten.

> [!NOTE]
> Flera hello felsökning i den här guiden innehåller instruktioner toorun Redis kommandon och övervaka olika prestandamått. Mer information och instruktioner finns hello artiklar i hello [ytterligare information](#additional-information) avsnitt.
> 
> 

## <a name="client-side-troubleshooting"></a>Felsökning på klienten
Det här avsnittet beskrivs felsöker problem som uppstår på grund av ett villkor på hello-klientprogrammet.

* [Minnesbelastning på hello-klient](#memory-pressure-on-the-client)
* [Burst av trafik](#burst-of-traffic)
* [Klienten hög CPU-användning](#high-client-cpu-usage)
* [Klienten sida bandbredd överskreds](#client-side-bandwidth-exceeded)
* [Storlek för stora frågor och svar](#large-requestresponse-size)
* [Vad hände toomy data i Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-hello-client"></a>Minnesbelastning på hello-klient
#### <a name="problem"></a>Problem
Minnesbelastning på klientdatorn hello leder tooall typer av problem med prestanda som kan fördröja behandlingen av data som skickades av hello Redis instans utan fördröjning. När minnesbelastning träffar har hello system toopage data från fysiskt minne toovirtual minne som finns på disken. Detta *sidan felaktigt* orsaker hello system tooslow ned avsevärt.

#### <a name="measurement"></a>Mätning
1. Övervaka minnesanvändning på datorn toomake till att det inte överstiger tillgängligt minne. 
2. Övervakaren hello `Page Faults/Sec` prestandaräknare. De flesta datorer ska ha vissa sidfel även under normal drift, så bevaka toppar i den här sidan fel prestandaräknaren som motsvarar timeout.

#### <a name="resolution"></a>Lösning
Uppgradera din klient tooa större klient VM-storlek med mer minne eller gå till din minne användningsmönster tooreduce minne consuption.

### <a name="burst-of-traffic"></a>Burst av trafik
#### <a name="problem"></a>Problem
Belastning av trafik i kombination med dålig `ThreadPool` inställningar kan orsaka försening i bearbetning av data som redan har skickats som hello Redis Server men har ännu inte har förbrukats på hello på klientsidan.

#### <a name="measurement"></a>Mätning
Övervaka hur din `ThreadPool` statistik förändras över tid genom att använda code [som detta](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Du kan också titta på hello `TimeoutException` meddelande från StackExchange.Redis. Här är ett exempel:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Det finns några problem som är intressanta i hello ovan meddelande:

1. Observera att i hello `IOCP` avsnittet och hello `WORKER` avsnitt som du har en `Busy` värde som är större än hello `Min` värde. Detta innebär att din `ThreadPool` inställningarna måste du justera.
2. Du kan också se `in: 64221`. Detta anger att 64211 byte har tagits emot på hello kernel socket layer men ännu inte har lästs av hello program (t.ex. StackExchange.Redis). Detta innebär oftast att programmet inte läsa data från hello nätverket snabbt hello-servern skickar den tooyou.

#### <a name="resolution"></a>Lösning
Konfigurera din [arbetstråd inställningar](https://gist.github.com/JonCole/e65411214030f0d823cb) toomake till att din trådpoolen skalar snabbt under burst-scenarier.

### <a name="high-client-cpu-usage"></a>Klienten hög CPU-användning
#### <a name="problem"></a>Problem
Hög CPU-användning på hello-klient är en indikation på att hello systemet inte håller hello arbete att den har ombetts tooperform. Detta innebär att hello-klientdatorer kan misslyckas tooprocess svar i tid från Redis trots att Redis skickade hello svar snabbt.

#### <a name="measurement"></a>Mätning
Övervakaren hello System Wide processoranvändning via hello Azure-portalen eller hello associerade prestandaräknare. Var noga med att inte toomonitor *processen* CPU eftersom en enda process kan ha låg CPU-belastning på hello samma tid att hela systemets CPU kan vara hög. Håll utkik efter toppar i CPU-användning som motsvarar timeout. På grund av hög CPU, du kan också se högt `in: XXX` värdena i `TimeoutException` felmeddelanden som beskrivs i hello [Burst av trafik](#burst-of-traffic) avsnitt.

> [!NOTE]
> StackExchange.Redis 1.1.603 och senare inkluderar hello `local-cpu` mått i `TimeoutException` felmeddelanden. Se till att du använder hello senaste versionen av hello [StackExchange.Redis NuGet-paketet](https://www.nuget.org/packages/StackExchange.Redis/). Det finns fel som ständigt fast i hello kod toomake den mer robust tootimeouts så att hello senaste version är viktigt.
> 
> 

#### <a name="resolution"></a>Lösning
Uppgradera tooa VM större med mer CPU-kapaciteten eller undersöka vad som orsakar processoranvändning. 

### <a name="client-side-bandwidth-exceeded"></a>Klienten sida bandbredd överskreds
#### <a name="problem"></a>Problem
Klientdatorer med olika storlek har begränsningar på hur mycket bandbredd som de finns tillgängliga. Om klienten hello överskrider hello bandbredd och sedan data kommer inte att bearbetas på klientsidan för hello snabbt hello-servern skickar den. Detta kan leda till tootimeouts.

#### <a name="measurement"></a>Mätning
Övervaka hur bandbreddsanvändningen förändras över tid genom att använda code [som detta](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Observera att den här koden inte kan köras i vissa miljöer med begränsad behörighet (till exempel Azure webbplatser).

#### <a name="resolution"></a>Lösning
Öka klienten VM-storlek eller minska nätverksbandbredden.

### <a name="large-requestresponse-size"></a>Storlek för stora frågor och svar
#### <a name="problem"></a>Problem
Tidsgränser kan medföra att en stor begäran och svar. Anta exempelvis din timeout-värde konfigureras på din klient är 1 sekund. Ditt program begär två nycklar (t.ex.) ”A” och ”B”) på hello samtidigt (med hjälp av hello samma fysiska nätverksanslutningen). De flesta klienter stöder ”Pipelining” begäranden, så att båda begäranden ”A” och ”B” skickas på hello överföring toohello servern en efter hello andra utan att vänta på hello svar. hello servern skickar hello svar tillbaka i hello samma ordning. Om svaret ”A” är stor äta nog den större delen av hello tidsgräns för efterföljande förfrågningar. 

hello som följande exempel visar det här scenariot. I det här scenariot hello begäran ”A” och ”B” skickas snabbt hello servern börjar skicka snabbt svar ”A” och ”B”, men på grund av data överföringstiden, ”B” fastna bakom andra begäran och gånger ut även om hello servern svarade snabbt.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Mätning
Det här är en svårt en toomeasure. Du har i princip tooinstrument din kod tootrack stora klientbegäranden och svar. 

#### <a name="resolution"></a>Lösning
1. Redis är optimerad för ett stort antal små värden i stället för ett par stora värden. hello önskade lösningen är toobreak upp data till relaterade mindre värden. Se hello [vad är hello perfekt storlek värdeintervallet för redis? Är 100KB för stor? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) efter information kring varför mindre värden rekommenderas.
2. Öka hello storleken på den virtuella datorn (för klient och Server för Redis-Cache) tooget högre bandbredd-funktioner, vilket minskar data transfer gånger större svar. Observera att få mer bandbredd på bara hello servern eller bara på hello klienten kanske inte är tillräckligt. Mäter bandbreddsanvändningen och jämför den toohello funktionerna i hello storlek för den virtuella datorn du har för närvarande.
3. Öka hello antalet `ConnectionMultiplexer` objekt användning och resursallokering begäranden via olika anslutningar.

### <a name="what-happened-toomy-data-in-redis"></a>Vad hände toomy data i Redis?
#### <a name="problem"></a>Problem
Förväntat för vissa data toobe i min Azure Redis-Cache-instans, men det verkar vara inte toobe det.

#### <a name="resolution"></a>Lösning
Se [vad hände toomy data i Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) för möjliga orsaker och lösningar.

## <a name="server-side-troubleshooting"></a>Felsökning på Server
Det här avsnittet beskrivs felsöker problem som uppstår på grund av ett villkor på hello cache-server.

* [Minnesbelastning på hello-server](#memory-pressure-on-the-server)
* [Hög processoranvändning / Server att läsa in](#high-cpu-usage-server-load)
* [Serverns sida bandbredd överskreds](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-hello-server"></a>Minnesbelastning på hello-server
#### <a name="problem"></a>Problem
Minnesbelastning på serversidan hello leder tooall typer av problem med prestanda som kan fördröja behandlingen av begäran. När minnesbelastning träffar har hello system toopage data från fysiskt minne toovirtual minne som finns på disken. Detta *sidan felaktigt* orsaker hello system tooslow ned avsevärt. Det finns flera möjliga orsaker till detta minnesbelastning: 

1. Du har fyllt hello cache toofull kapacitet med data. 
2. Redis ser hög minnesfragmenteringen - orsakas normalt genom att lagra stora objekt (Redis är optimerad för en små objekt - Se hello [vad är hello perfekt storlek värdeintervallet för redis? Är 100KB för stor? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) efter mer information). 

#### <a name="measurement"></a>Mätning
Redis visar två mått som kan hjälpa dig att identifiera problemet. hello först är `used_memory` och hello andra `used_memory_rss`. [De här måtten](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) är tillgängliga i hello Azure-portalen eller via hello [Redis INFO](http://redis.io/commands/info) kommando.

#### <a name="resolution"></a>Lösning
Det finns flera möjliga ändringar som du kan göra toohelp Se minnesanvändningen felfri:

1. [Konfigurera en princip för minne](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) och Ställ in förfallodatum gånger på dina nycklar. Observera att det räcker kanske inte om du har fragmentering.
2. [Anger värdet maxmemory-reserverade](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) som är tillräckligt stor för toocompensate för minnesfragmentering.
3. Dela upp stora cachelagrade objekten i mindre relaterade objekt.
4. [Skala](cache-how-to-scale.md) tooa större cachestorlek.
5. Om du använder en [premium-cache med Redis cluster aktiverat](cache-how-to-premium-clustering.md) kan du [öka hello antalet shards](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hög processoranvändning / Server att läsa in
#### <a name="problem"></a>Problem
Hög CPU-användning kan innebära att hello på klientsidan kan inte tooprocess svar i tid från Redis trots att Redis skickade hello svar snabbt.

#### <a name="measurement"></a>Mätning
Övervakaren hello System Wide processoranvändning via hello Azure-portalen eller hello associerade prestandaräknare. Var noga med att inte toomonitor *processen* CPU eftersom en enda process kan ha låg CPU-belastning på hello samma tid att hela systemets CPU kan vara hög. Håll utkik efter toppar i CPU-användning som motsvarar timeout.

#### <a name="resolution"></a>Lösning
[Skala](cache-how-to-scale.md) tooa större cache tjänstnivån med mer CPU-kapaciteten eller undersöka vad som orsakar processoranvändning. 

### <a name="server-side-bandwidth-exceeded"></a>Serverns sida bandbredd överskreds
#### <a name="problem"></a>Problem
Cacheinstanser med olika storlek har begränsningar på hur mycket bandbredd som de finns tillgängliga. Om hello server överskrider den tillgängliga bandbredden för hello, sedan skickas data inte toohello klienten så snabbt. Detta kan leda till tootimeouts.

#### <a name="measurement"></a>Mätning
Du kan övervaka hello `Cache Read` mått, som är hello mängd data läses från hello cacheminnet i megabyte per sekund (MB/s) under hello angivna reporting intervallet. Det här värdet motsvarar toohello nätverksbandbredd som används av det här cacheminnet. Om du vill använda tooset in aviseringar för server side nätverket bandbreddsgränser kan du skapa dem med hjälp av det här `Cache Read` räknaren. Jämför din avläsningar med hello värdena i [tabellen](cache-faq.md#cache-performance) för hello observerats bandbreddsgränser för olika cache priser nivåer och storlekar.

#### <a name="resolution"></a>Lösning
Om du närmar sig konsekvent hello observerats Maximal bandbredd för din prisnivå nivå och cache-storlek, bör du [skalning](cache-how-to-scale.md) tooa priser nivå eller storlek som har större nätverksbandbredd, med hello värden i [tabellen](cache-faq.md#cache-performance) som en vägledning.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis tidsgränsfel
StackExchange.Redis använder en konfigurationsinställning namngivna `synctimeout` för synkrona åtgärder som har ett standardvärde på 1 000 ms. Om ett synkrona anrop inte slutförs i fastställs hello tid, hello StackExchange.Redis klienten returnerar en timeout fel liknande toohello följande exempel.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Det här felmeddelandet innehåller mått som kan hjälpa dig att peka toohello orsaken och möjlig lösning av hello problemet. hello innehåller följande tabell information om hello fel meddelande mått.

| Fel meddelande mått | Information |
| --- | --- |
| PL |I hello senaste tidsintervallet: 0-kommandon som har utfärdats. |
| hanterare av |hello socket manager utför `socket.select` så att den frågar hello OS tooindicate en socket som har något toodo; i princip: hello läsaren inte aktivt läsning från hello nätverk eftersom det inte tror att det är något toodo |
| Kön |Totalt antal åtgärder pågående har 73 |
| citattecken kring |6 hello pågående åtgärder finns i hello unsent kö och har inte skrivits ännu toohello utgående nätverk |
| Qs |67 han pågående åtgärder har skickats toohello server men svaret är inte tillgänglig ännu. hello svaret kan bli `Not yet sent by hello server` eller`sent by hello server but not yet processed by hello client.` |
| QC |0 hello pågående åtgärder har sett svar men har ännu inte markerats som slutförd på grund av toowaiting på hello slutförande loop |
| WR |Det finns en aktiv skrivaren (vilket innebär att hello 6 ej skickade begäranden inte ignoreras) byte/activewriters |
| i |Det finns ingen aktiv läsare och noll byte är tillgängliga toobe läsa på hello NIC byte/activereaders |

### <a name="steps-tooinvestigate"></a>Steg tooinvestigate
1. Som bästa praxis se till att använder du hello efter mönster tooconnect när hello StackExchange.Redis-klienten.

    ```c#
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    Mer information finns i [ansluta toohello cacheminne med hjälp av StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Se till att din Azure Redis-Cache och hello klientprogram hello samma region i Azure. Till exempel du kanske att få timeout när ditt cacheminne är i östra USA men hello klienten finns i USA, västra och hello begäran inte slutförs inom hello `synctimeout` intervall eller så kan du får ett timeout när du felsöker från lokala utvecklingsdatorn. 
   
    Det har rekommenderar toohave hello cache och hello i hello-klienten i samma Azure-region. Om du har ett scenario som innehåller mellan region anrop, bör du ange hello `synctimeout` tooa intervallvärdet högre än hello 1 000 ms standardintervallet genom att inkludera en `synctimeout` egenskap i hello anslutningssträngen. hello följande exempel visas en StackExchange.Redis cache anslutning sträng fragment med en `synctimeout` av 2000 ms.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Se till att du använder hello senaste versionen av hello [StackExchange.Redis NuGet-paketet](https://www.nuget.org/packages/StackExchange.Redis/). Det finns fel som ständigt fast i hello kod toomake den mer robust tootimeouts så att hello senaste version är viktigt.
3. Om det finns begäranden som är komma bundna av bandbreddsbegränsningar på hello server eller klient, tar längre tid för dem toocomplete och därmed orsakar timeout. toosee om din tidsgränsen på grund av toonetwork bandbredd hello server, se [Server side bandbredd överskridit](#server-side-bandwidth-exceeded). toosee om din tidsgränsen på grund av tooclient nätverksbandbredd finns [klienten sida bandbredd överskridit](#client-side-bandwidth-exceeded).
4. Är du komma CPU bundna på hello server eller hello-klient?
   
   * Kontrollera om du är komma bundna av processor på klienten som kan orsaka hello begäran toonot att behandlas inom hello `synctimeout` intervall, vilket kan orsaka en tidsgräns. Flytta tooa större storlek på klientens eller distribuerar hello belastningen hjälper toocontrol detta. 
   * Kontrollera om du får CPU bunden på hello-servern genom att övervaka hello `CPU` [cachelagra prestanda mått](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Begäranden som kommer medan Redis CPU-bundna kan orsaka sådana begäranden tootimeout. tooaddress detta kan du distribuera hello ladda över flera delar i premium-cache eller uppgradera tooa större storlek eller prisnivån. Mer information finns i [servern sida bandbredd överskridit](#server-side-bandwidth-exceeded).
5. Finns det kommandon som tar lång tid tooprocess på hello-servern? Tidskrävande kommandon som tar lång tid tooprocess på hello redis-servern kan orsaka timeout. Är några exempel på tidskrävande kommandon `mget` med ett stort antal nycklar, `keys *` eller felaktiga lua skript. Du kan ansluta tooyour Azure Redis-Cache-instans med hello redis-cli klienten eller använda hello [Redis-konsolen](cache-configure.md#redis-console) och kör hello [SlowLog](http://redis.io/commands/slowlog) kommandot toosee om begäran tar längre tid än förväntat. Redis-servern och StackExchange.Redis är optimerad för många små begäranden i stället för färre stora begäranden. Dela upp dina data i mindre segment kan du förbättra här saker. 
   
    Information om hur du ansluter toohello Azure Redis-Cache SSL-slutpunkten med hjälp av redis-cli och stunnel finns hello [om ASP.NET Sessionstillståndsprovider för Redis-förhandsversionen](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogginlägg. Mer information finns i [SlowLog](http://redis.io/commands/slowlog).
6. Hög Redis-serverbelastning kan orsaka timeout. Du kan övervaka hello belastningen på servern genom att övervaka hello `Redis Server Load` [cachelagra prestanda mått](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). En serverbelastningen 100 (högsta värde) innebär att hello redis-servern har varit upptagen med ingen inaktivitetstid bearbetning av begäranden. toosee om vissa begäranden tar upp alla hello serverfunktioner kör hello SlowLog kommando, enligt beskrivningen i hello föregående stycke. Mer information finns i [hög processoranvändning / Server att läsa in](#high-cpu-usage-server-load).
7. Var det andra händelser på hello på klientsidan som kan ha orsakat ett nätverk blip? Kontrollera på hello-klient (webb, arbetsrollen eller en Iaas VM) om det fanns en händelse som skalning hello antalet instanser som klienten upp eller ned eller distribuerar en ny version av klienten hello eller Autoskala aktiveras? I våra tester som vi har hittat att Autoskala eller skala upp/ned kan utgående nätverksanslutningen gå förlorade i några sekunder. StackExchange.Redis koden är flexibel toosuch händelser och återupprättas. Under den här tiden på ny anslutning kan alla begäranden i kö för hello timeout.
8. Har det skett en stor begäran föregående flera små begäran toohello Redis-Cache som gjort timeout? Hej parametern `qs` hello fel meddelandet anger hur många begäranden har skickats från hello klient toohello server, men ännu inte har bearbetat svaret. Det här värdet kan hålla växer eftersom StackExchange.Redis använder en TCP-anslutning och kan endast läsa ett svar i taget. Även om hello första åtgärden orsakade timeout, slutar inte hello data som skickas till eller från hello server och andra begäranden blockeras tills det är klart, orsakar timeout. En lösning är toominimize hello risken för timeout genom att säkerställa att din cache är tillräckligt stor för din arbetsbelastning och dela upp stora värden i mindre segment. En annan möjlig lösning är toouse en pool av `ConnectionMultiplexer` objekt i din klient och välj hello minst inlästa `ConnectionMultiplexer` när du skickar en ny begäran. Detta bör förhindra att en enda tidsgräns orsakar andra tooalso timeout för begäranden.
9. Om du använder `RedisSessionStateprovider`, se till att du har angett korrekt hello gör timeout. `retrytimeoutInMilliseconds`bör vara högre än `operationTimeoutinMilliseonds`, annars inga nya försök utförs. I följande exempel hello `retrytimeoutInMilliseconds` too3000 anges. Mer information finns i [ASP.NET-Sessionstillståndsprovider för Azure Redis-Cache](cache-aspnet-session-state-provider.md) och [hur toouse hello konfigurationsparametrar för Sessionstillståndsprovider och Utdatacacheprovider](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Kontrollera minnesanvändning på hello Azure Redis-Cache-server genom att [övervakning](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` och `Used Memory`. Om en princip för borttagning är på plats, Redis börjar ta bort nycklar när `Used_Memory` når hello cachestorlek. Vi rekommenderar `Used Memory RSS` ska bara vara något högre än `Used memory`. En stor skillnad innebär att det minnesfragmenteringen (interna eller externa. När `Used Memory RSS` är mindre än `Used Memory`, innebär det en del av hello cacheminne har bytts hello-operativsystemet. Om detta inträffar kan du förväntar dig några betydande fördröjningar. Eftersom Redis inte har kontroll över hur dess allokeringar mappas toomemory sidor, hög `Used Memory RSS` ofta är en topp i minnesanvändning hello resultat. När Redis Frigör minne, hello minne ges tillbaka toohello allokeraren och hello allokeraren kanske eller kanske inte ger hello minne tillbaka toohello system. Det kan finnas en avvikelse mellan hello `Used Memory` förbrukning av värdet och minne som rapporteras av hello-operativsystemet. Det kan vara toohello fakta minne har används och publicerat som Redis, men inte angivna tillbaka toohello system. toohelp minska minnesproblem kan du utföra följande steg hello.
   
   * Uppgradera hello tooa större cachestorleken så att du inte kör visa begränsningar i minnet på hello system.
   * Ange förfallodatum gånger på hello nycklar så att äldre värden har avlägsnats proaktivt.
   * Övervakaren hello hello `used_memory_rss` cachelagra mått. När det här värdet närmar sig hello sina cachens storlek, är förmodligen toostart ser prestandaproblem. Distribuera hello data över flera delar om du använder en premium-cache eller uppgradera tooa större cachestorlek.
   
   Mer information finns i [minnesbelastning på hello server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Ytterligare information
* [Vilket erbjudande och vilken storlek ska jag använda för Redis-cache?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Hur kan jag mäta och testa hello prestanda för Mina cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Hur kör Redis kommandon?](cache-faq.md#how-can-i-run-redis-commands)
* [Hur toomonitor Azure Redis-Cache](cache-how-to-monitor.md)

