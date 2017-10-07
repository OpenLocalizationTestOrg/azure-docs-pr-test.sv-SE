---
title: aaaAzure Cosmos DB .NET SDK & resurser | Microsoft Docs
description: "Läs mer om hello .NET-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB .NET SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 8e239217-9085-49f5-b0a7-58d6e6b61949
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0ec30e0130067a9b8d4c9176cf7465bac8925bf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-sdk-download-and-release-notes"></a>Azure DB .NET SDK för Cosmos: Hämta och viktig information
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET ändra Feed](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-resursprovider](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK-hämtningen**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>

<tr><td>**API-dokumentationen**</td><td>[.NET API-referensdokumentation](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Exempel**</td><td>[.NET-kodexempel](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Kom igång**</td><td>[Kom igång med hello Azure Cosmos DB .NET SDK](documentdb-get-started.md)</td></tr>

<tr><td>**Självstudier för Web app**</td><td>[Utveckling av webbappar med Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuella framework som stöds**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Viktig information

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Stöd för PartitionKeyRangeId som en FeedOption scope frågan resultat tooa specifika viktiga partitionsintervallvärdet har lagts till. 
* Stöd har lagts till för StartTime som en ChangeFeedOption toostart söker efter hello ändringar efter den tidsperioden.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Ett problem har åtgärdats i hello JsonSerializable klass som kan orsaka en dataspillsundantag.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   Ett problem har åtgärdats som krävs för kompilera av hello programmet på grund av toohello införandet av JsonSerializerSettings som en valfri parameter i hello DocumentClient konstruktor.
* Markerade hello DocumentClient konstruktor föråldrad som behövs JsonSerializerSettings hello den sista parametern tooallow för standardvärden för parametrar ConnectionPolicy och ConsistencyLevel när skickas i JsonSerializerSettings-parametern.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   Tillagt stöd för att ange anpassade JsonSerializerSettings när [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   Ett problem som påverkade x64 har åtgärdats datorer som inte stöder SSE4 instruktion och utlösa ett SEHException när du kör Azure Cosmos DB DocumentDB API-frågor.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.
*   Stöd har lagts till för frågan måtten för enskilda partitioner.
*   Stöd har lagts till för att begränsa hello storleken på hello fortsättningstoken för frågor.
*   Tillagt stöd för mer detaljerad spårning för misslyckade begäranden.
*   Gjorts prestanda förbättras i hello SDK.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* Funktionellt samma som 1.13.3. Gjort vissa ändringar för internt.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* Funktionellt samma som 1.13.2. Gjort vissa ändringar för internt.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Ett problem som ignoreras hello PartitionKey värdet som angetts i FeedOptions för sammanställd frågor har åtgärdats.
* Ett problem har åtgärdats i transparent hantering av partition hantering under halva rör sig över flera partitioner Order By Frågekörningen.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Ett problem som orsakade deadlocks i vissa hello asynkrona API: er när den används i ASP.NET-kontexten har åtgärdats.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Åtgärdar toomake SDK mer flexibel tooautomatic växling vid fel under vissa förhållanden.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Korrigering för ett problem som ibland medför en WebException: hello fjärrnamnet kunde inte matchas.
* Tillagda hello stöd för direkt läsa ett skrivet dokument genom att lägga till nya överlagringar tooReadDocumentAsync API.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* LINQ-stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) lagts till.
* Åtgärda för en minnesläcka för hello ConnectionPolicy objektet beror på hello användning av händelsehanterare.
* Korrigering för ett problem där UpsertAttachmentAsync inte fungerade när ETag har använts.
* Korrigering för ett problem där mellan partition ordning av frågan fortsättning inte arbetar vid sortering i string-fält.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till. Se [aggregering support](documentdb-sql-query.md#Aggregates).
* Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Korrigering för ett problem där vissa hello cross-partition frågor misslyckas i hello 32-bitars värdprocess.
* Korrigering för ett problem där hello session behållaren inte uppdaterades med hello token för misslyckade begäranden i Gateway-läge.
* Korrigering för ett problem där en fråga med UDF-anrop i projektionen misslyckas i vissa fall.
* Klienten sida prestandakorrigeringar för att öka hello läsa och skriva genomströmning hello begäranden.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Korrigering för ett problem där hello session behållaren inte uppdaterades med hello token för misslyckade begäranden.
* Stöd för hello SDK toowork i en 32-bitars värdprocess har lagts till. Observera att om du använder mellan partition frågor rekommenderas 64-bitars värden bearbetning för bättre prestanda.
* Förbättrad prestanda för scenarier som rör frågor med ett stort antal partition viktiga värden i uttryck.
* Fylls i olika resurs kvoten statistik i hello ResourceResponse för dokumentsamling diskläsningsbegäranden när PopulateQuotaInfo begäran om alternativet anges.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* Lägre prestanda korrigering för hello CreateDocumentCollectionIfNotExistsAsync API lanserades 1.11.0.
* Korrigera prestanda i hello SDK för scenarier för hög grad av samtidiga begäranden.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Stöd för nya klasser och metoder tooprocess hello [ändra feed](change-feed.md) av dokument i en samling.
* Stöd för cross-partitionsfrågan fortsättning och vissa perf förbättringar för cross-partition frågor.
* Tillägg av metoderna CreateDatabaseIfNotExistsAsync och CreateDocumentCollectionIfNotExistsAsync.
* LINQ-stöd för systemfunktioner: IsDefined, IsNull och IsPrimitive.
* Åtgärda för automatisk binplacing av Microsoft.Azure.Documents.ServiceInterop.dll och DocumentDB.Spatial.Sql.dll sammansättningar tooapplication's bin-mappen när med projekt som har project.json verktygsuppsättning hello Nuget-paketet.
* Stöd för sändning av klienten på klientsidan ETW spår som kan vara användbara vid felsökning av scenarier.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Tillagda direktanslutning stöd för partitionerade samlingar.
* Bättre prestanda för hello avgränsas föråldrad konsekvensnivå.
* Tillagda Polygon och LineString DataTypes när du anger samling indexering princip för geografiska avgränsningar spatial frågor.
* LINQ stöd har lagts till för StringEnumConverter, IsoDateTimeConverter och UnixDateTimeConverter vid översättning av predikat.
* Olika SDK felkorrigeringar.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Ett problem har åtgärdats som orsakade hello följande NotFoundException: hello läsa sessionen inte är tillgänglig för hello inkommande sessionstoken. Det här undantaget inträffade i vissa fall när du frågar efter hello Läs-region för fördelade-konto.
* Exponeras hello ResponseStream egenskap i hello ResourceResponse klass, vilket gör att direktåtkomst toohello underliggande ström från ett svar.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Ändrade hello ResourceResponse, FeedResponse, StoredProcedureResponse och MediaResponse klasser tooimplement hello motsvarande offentliga gränssnittet så att de kan mocked för drivs distribution (TDD).
* Ett problem som orsakade ett felaktigt partition viktiga huvud när du använder ett anpassat JsonSerializerSettings-objekt för serialisering av data har åtgärdats.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Ett problem som orsakade tidskrävande frågor toofail med felet har åtgärdats: autentiseringstoken är inte giltig på hello aktuell tid.
* Fast ett problem som bort hello ursprungliga SqlParameterCollection från mellan partition upp/Ordna efter-frågor.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Stöd har lagts till för parallella frågor för partitionerade samlingar.
* Tillagt stöd för mellan partition ORDER BY- och upp frågor för partitionerade samlingar.
* Fast hello saknas referenser tooDocumentDB.Spatial.Sql.dll och Microsoft.Azure.Documents.ServiceInterop.dll som krävs när du refererar till ett Azure DB som Cosmos-projekt med en referens toohello Azure Cosmos DB Nuget-paketet.
* Fast hello möjlighet toouse parametrar av olika typer när du använder användardefinierade funktioner i LINQ. 
* Fast ett programfel för globalt replikerade konton där Upsert anrop som riktade tooread platserna istället för att skriva platser.
* Tillagda metoder toohello IDocumentClient gränssnitt som saknas: 
  * UpsertAttachmentAsync-metod som använder mediaStream och alternativ som parametrar
  * CreateAttachmentAsync-metod som tar alternativ som en parameter
  * CreateOfferQuery metod som tar querySpec som en parameter.
* Oförseglat offentliga klasser som visas i hello IDocumentClient gränssnitt.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Tillagda hello stöd för flera regioner databasen konton.
* Stöd för nya försök för begränsad förfrågningar har lagts till.  Användare kan anpassa hello antal återförsök och Hej max väntetid genom att konfigurera hello ConnectionPolicy.RetryOptions egenskapen.
* Lägga till ett nytt IDocumentClient gränssnitt som definierar hello signaturer för alla DocumenClient egenskaper och metoder.  Som en del av den här ändringen kan också ändra tilläggsmetoder som skapar IQueryable och IOrderedQueryable toomethods på hello själva DocumentClient-klassen.
* Lägga till konfigurationen alternativet tooset hello ServicePoint.ConnectionLimit för en viss Azure Cosmos DB slutpunkten Uri.  Använd ConnectionPolicy.MaxConnectionLimit toochange hello standardvärdet, vilket anges too50.
* Föråldrad IPartitionResolver och dess implementering.  Stöd för IPartitionResolver är nu föråldrad. Du rekommenderas att använda partitionerade samlingar för högre lagring och genomflöde.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Lägga till en överlagring tooUri baserat ExecuteStoredProcedureAsync metod som tar RequestOptions som en parameter.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Extra tid toolive (TTL) stöd för dokument.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* Fast ett programfel i Nuget-paket för .NET SDK för att paketera den som en del av en Azure Cloud Service-lösning.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md). 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Fast]**  Frågar Azure Cosmos DB endpoint utlöser: ' System.Net.Http.HttpRequestException: fel vid kopiering av innehåll tooa dataströmmen '.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Utökade LINQ stöder inklusive nya operatorer för växling, villkorlig uttryck och intervallet jämförelse.
  * Ta operatorn tooenable Välj upp beteende i LINQ
  * CompareTo operatorn tooenable sträng intervallet jämförelser
  * Villkorlig (?) och slå samman operatorer (?)
* **[Fast]**  ArgumentOutOfRangeException när du kombinerar modellen projektion med var i i en LINQ-fråga. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Fast]**  Om väljer inte hello senaste uttryck hello LINQ-providern antas ingen projektion och producerade väljer * felaktigt.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Implementerad Upsert, läggs UpsertXXXAsync metoder
* Prestandaförbättringar för alla begäranden
* LINQ-providern stöd för villkorlig, slå samman, och CompareTo metoder för strängar
* **[Fast]**  LINQ-provider--> Implementera innehåller metoden i listan toogenerate hello samma SQL som på IEnumerable och matris
* **[Fast]**  BackoffRetryUtility använder hello samma HttpRequestMessage igen i stället för att skapa en ny på försök igen
* **[Inaktuell]**  UriFactory.CreateCollection--> bör nu använda UriFactory.CreateDocumentCollection

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Fast]**  Lokalisering problem när du använder inte en kultur info nl-NL, t.ex. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Lägga till ID-baserade Routning
  * Den nya UriFactory helper tooassist med konstruera ID-baserade resurslänkar
  * Nya överlagringar på DocumentClient tootake i URI: N
* Tillagda IsValid() och IsValidDetailed() i LINQ för geospatial
* Stöd för LINQ-providern förbättrad:
  * **Math** -Abs, ARCCOS, ARCSIN, ARCTAN, taket, Cos Exp, våning, Log, Log10, Pow, runda, signera, Sin, Sqrt, Tan, trunkera
  * **Strängen** -Concat, innehåller EndsWith, IndexOf, Count, ToLower, TrimStart, ersätta, omvänd TrimEnd, StartsWith, delsträngen, ToUpper
  * **Matrisen** -Concat, innehåller, antal
  * **I** operator

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Stöd har lagts till för att ändra principer för indexering.
  * Nya ReplaceDocumentCollectionAsync-metoden i DocumentClient
  * Ny IndexTransformationProgress egenskap i ResourceResponse<T> för spårning av procent förloppet för index principändringar
  * DocumentCollection.IndexingPolicy är nu föränderliga
* Stöd för indexering och fråga har lagts till.
  * Den nya Microsoft.Azure.Documents.Spatial namnområdet för Serialiserar/Deserialiserar spatialtyper som punkt och Polygon
  * Ny SpatialIndex klass för indexering GeoJSON data som lagras i Cosmos-DB
* **[Fast]**  Felaktig SQL-fråga som genereras av en LINQ-uttrycket [#38](https://github.com/Azure/azure-documentdb-net/issues/38).

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Lägga till ett beroende på Newtonsoft.Json v5.0.7.
* Ändringar som gjorts toosupport Order By:
  
  * LINQ-providerstöd för OrderBy() eller OrderByDescending()
  * IndexingPolicy toosupport Order By 
    
    **Möjliga senaste ändring** 
    
    Om du har befintliga kod som tillhandahåller samlingar med en anpassad indexprincip uppdatera din befintliga kod måste toobe toosupport hello ny IndexingPolicy klass. Om du har ingen anpassad indexprincip sedan påverkar den här ändringen inte du.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Stöd för partitionering av data med hjälp av hello nya HashPartitionResolver och RangePartitionResolver klasser och hello IPartitionResolver har lagts till.
* Tillagda DataContract-serialisering.
* Tillagda GUID stöder i LINQ-providern.
* Tillagda UDF stöd i LINQ.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK

## <a name="release--retirement-dates"></a>Versionen & pensionering datum
Microsoft meddelar minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.

Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som vi rekommenderar att du alltid uppgradera toohello senaste SDK version så snart som möjligt. 

Alla begäranden tooAzure Cosmos-databasen med en pensionerad SDK avvisas av hello-tjänsten.

<br/>

| Version | Utgivningsdatum | Datumet för tillbakadragandet |
| --- | --- | --- |
| [1.17.0](#1.17.0) |10 augusti 2017 |--- |
| [1.16.1](#1.16.1) |07 augusti 2017 |--- |
| [1.16.0](#1.16.0) |02 augusti 2017 |--- |
| [1.15.0](#1.15.0) |30 juni 2017 |--- |
| [1.14.1](#1.14.1) |23 maj 2017 |--- |
| [1.14.0](#1.14.0) |10 maj 2017 |--- |
| [1.13.4](#1.13.4) |09 kan 2017 |--- |
| [1.13.3](#1.13.3) |06 kan 2017 |--- |
| [1.13.2](#1.13.2) |19 april 2017 |--- |
| [1.13.1](#1.13.1) |Den 29 mars 2017 |--- |
| [1.13.0](#1.13.0) |24 mars 2017 |--- |
| [1.12.2](#1.12.2) |20 mars 2017 |--- |
| [1.12.1](#1.12.1) |14 mars 2017 |--- |
| [1.12.0](#1.12.0) |15 februari 2017 |--- |
| [1.11.4](#1.11.4) |06 februari 2017 |--- |
| [1.11.3](#1.11.3) |26 januari 2017 |--- |
| [1.11.1](#1.11.1) |21 december 2016 |--- |
| [1.11.0](#1.11.0) |08 december 2016 |--- |
| [1.10.0](#1.10.0) |27 september 2016 |--- |
| [1.9.5](#1.9.5) |01 september 2016 |--- |
| [1.9.4](#1.9.4) |24 augusti 2016 |--- |
| [1.9.3](#1.9.3) |15 augusti 2016 |--- |
| [1.9.2](#1.9.2) |23 juli 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.1](#1.7.1) |06 maj, 2016 |--- |
| [1.7.0](#1.7.0) |26 april 2016 |--- |
| [1.6.3](#1.6.3) |08 april 2016 |--- |
| [1.6.2](#1.6.2) |Den 29 mars 2016 |--- |
| [1.5.3](#1.5.3) |19 februari 2016 |--- |
| [1.5.2](#1.5.2) |14 december 2015 |--- |
| [1.5.1](#1.5.1) |23 november 2015 |--- |
| [1.5.0](#1.5.0) |05 oktober 2015 |--- |
| [1.4.1](#1.4.1) |25 augusti 2015 |--- |
| [1.4.0](#1.4.0) |13 augusti 2015 |--- |
| [1.3.0](#1.3.0) |05 augusti 2015 |--- |
| [1.2.0](#1.2.0) |06 juli 2015 |--- |
| [1.1.0](#1.1.0) |30 april 2015 |--- |
| [1.0.0](#1.0.0) |08 april 2015 |--- |


## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Se även
toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida. 

