---
title: aaaAzure Cosmos DB .NET Core API SDK & resurser | Microsoft Docs
description: "Läs mer om hello .NET Core-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB .NET Core SDK."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a>Azure Cosmos DB .NET Core SDK: Viktig information och resurser
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

<tr><td>**SDK-hämtningen**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-dokumentationen**</td><td>[.NET API-referensdokumentation](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Exempel**</td><td>[.NET-kodexempel](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Kom igång**</td><td>[Kom igång med hello Azure Cosmos DB .NET Core SDK](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td>**Självstudier för Web app**</td><td>[Utveckling av webbappar med Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuella framework som stöds**</td><td>[.NET standard 1.6 och .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Viktig information

hello Azure Cosmos DB .NET Core SDK har funktionsparitet med hello senaste versionen av hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).

> [!NOTE] 
> hello Azure Cosmos DB .NET Core SDK är inte kompatibel med den universella Windowsplattformen (UWP) appar ännu. Om du är intresserad av hello .NET Core SDK som stöder UWP-appar kan skicka e-post för[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Stöd för PartitionKeyRangeId som en FeedOption scope frågan resultat tooa specifika viktiga partitionsintervallvärdet har lagts till. 
* Stöd har lagts till för StartTime som en ChangeFeedOption toostart söker efter hello ändringar efter den tidsperioden. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Ett problem har åtgärdats i hello JsonSerializable klass som kan orsaka en dataspillsundantag.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Tillagt stöd för att ange anpassade JsonSerializerSettings när en [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instans.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Stöder .NET Standard 1.5 som en hello mål ramverk.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Ett problem som påverkade x64 har åtgärdats datorer som inte stöder SSE4 instruktion och utlösa SEHException när du kör Azure DB som Cosmos-frågor.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.
*   Stöd har lagts till för frågan måtten för enskilda partitioner.
*   Stöd har lagts till för att begränsa hello storleken på hello fortsättningstoken för frågor.
*   Tillagt stöd för mer detaljerad spårning för misslyckade begäranden.
*   Gjorts prestanda förbättras i hello SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Ett problem som ignoreras hello PartitionKey värdet som angetts i FeedOptions för sammanställd frågor har åtgärdats.
* Ett problem har åtgärdats i transparent hantering av partition hantering under halva rör sig över flera partitioner Order By Frågekörningen.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Ett problem som orsakade deadlocks i vissa hello asynkrona API: er när den används i ASP.NET-kontexten har åtgärdats.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Åtgärdar toomake SDK mer flexibel tooautomatic växling vid fel under vissa förhållanden.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Korrigering för ett problem som ibland medför en WebException: hello fjärrnamnet kunde inte matchas.
* Tillagda hello stöd för direkt läsa ett skrivet dokument genom att lägga till nya överlagringar tooReadDocumentAsync API.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* LINQ-stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) lagts till.
* Åtgärda för en minnesläcka för hello ConnectionPolicy objektet beror på hello användning av händelsehanterare.
* Korrigering för ett problem där UpsertAttachmentAsync inte fungerade när ETag har använts.
* Korrigering för ett problem där mellan partition ordning av frågan fortsättning inte arbetar vid sortering i string-fält.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till. Se [aggregering support](documentdb-sql-query.md#Aggregates).
* Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

hello Azure Cosmos DB .NET Core SDK kan du toobuild snabb, plattformsoberoende [ASP.NET Core](https://www.asp.net/core) och [.NET Core](https://www.microsoft.com/net/core#windows) toorun för appar i Windows, Mac och Linux. hello senaste versionen av hello Azure Cosmos DB .NET Core SDK är fullständigt [Xamarin](https://www.xamarin.com) kompatibel och använda toobuild program som är riktade till iOS, Android och Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

hello Azure Cosmos DB .NET Core Preview SDK kan du toobuild snabb, plattformsoberoende [ASP.NET Core](https://www.asp.net/core) och [.NET Core](https://www.microsoft.com/net/core#windows) toorun för appar i Windows, Mac och Linux.

hello Azure Cosmos DB .NET Core Preview SDK har funktionsparitet med hello senaste versionen av hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) och stöder hello följande:
* Alla [anslutningsläge](performance-tips.md#networking): Gateway läge, direkt TCP och direkt HTTPs. 
* Alla [konsekvensnivåer](consistency-levels.md): stark, Session, begränsat föråldrad och Eventual.
* [Partitionerade samlingar](partition-data.md). 
* [Flera regioner databasen konton och geo-replikering](distribute-data-globally.md).

Om du har frågor relaterade toothis SDK, efter för[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), eller ett problem i hello [github-lagringsplatsen](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Versionen & pensionering datum

| Version | Utgivningsdatum | Datumet för tillbakadragandet |
| --- | --- | --- |
| [1.5.0](#1.5.0) |10 augusti 2017 |--- | 
| [1.4.1](#1.4.1) |07 augusti 2017 |--- |
| [1.4.0](#1.4.0) |02 augusti 2017 |--- |
| [1.3.2](#1.3.2) |12 juni 2017 |--- |
| [1.3.1](#1.3.1) |23 maj 2017 |--- |
| [1.3.0](#1.3.0) |10 maj 2017 |--- |
| [1.2.2](#1.2.2) |19 april 2017 |--- |
| [1.2.1](#1.2.1) |Den 29 mars 2017 |--- |
| [1.2.0](#1.2.0) |25 mars 2017 |--- |
| [1.1.2](#1.1.2) |20 mars 2017 |--- |
| [1.1.1](#1.1.1) |14 mars 2017 |--- |
| [1.1.0](#1.1.0) |16 februari 2017 |--- |
| [1.0.0](#1.0.0) |21 december 2016 |--- |
| [0.1.0-Preview](#0.1.0-preview) |15 november 2016 |Den 31 december 2016 |

## <a name="see-also"></a>Se även
toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida. 

