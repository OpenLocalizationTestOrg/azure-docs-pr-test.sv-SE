---
title: aaaAzure Cosmos DB Python API SDK & resurser | Microsoft Docs
description: "Läs mer om hello Python-API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB Python SDK."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a>Azure Cosmos DB Python SDK: Viktig information och resurser
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

<tr><td>**Ladda ned SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td>**API-dokumentationen**</td><td>[Python-API-referensdokumentation](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td>**Installationsinstruktioner för SDK**</td><td>[Installationsinstruktioner för Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td>**Bidra tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td>**Kom igång**</td><td>[Kom igång med hello Python SDK](documentdb-python-application.md)</td></tr>

<tr><td>**Aktuella plattform som stöds**</td><td>[Python 2.7](https://www.python.org/downloads/) och [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Viktig information
### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.


### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till.
* Lägga till ett alternativ för att inaktivera SSL-kontroll när du kör mot Cosmos DB-emulatorn.
* Ta bort hello begränsning för beroende begäranden modulen toobe exakt 2.10.0.
* Sänks minsta dataflöde på partitionerade samlingar från 10,100 RU/s too2500 RU/s.
* Stöd har lagts till för att aktivera loggning skript under körning av lagrad procedur.
* REST API-version för stötar ' 2017-01-19' med den här versionen.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* Gjort redigeringsändringar toodocumentation kommentarer.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Stöd för Python 3.5 har lagts till.
* Stöd för anslutningspooler modulen begäranden har lagts till.
* Stöd för sessionskonsekvens har lagts till.
* Stöd för upp/ORDERBY för partitionerade samlingar har lagts till.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Tillagda försök princip stöd för begränsad begäranden. (Begränsad begäranden får en förfrågan hastighet för stor undantag, felkod 429.) Standard Azure Cosmos DB återförsök nio gånger för varje begäran när felkoden 429 påträffas respektera hello retryAfter tid i hello-Svarsrubrik. En fast försök tidsintervall kan nu anges som en del av hello RetryOptions egenskapen på hello ConnectionPolicy objekt om du vill tooignore hello retryAfter tid mellan hello försök som returnerades av servern. Azure Cosmos-DB väntar nu högst 30 sekunder för varje begäran som har begränsats (oavsett antal försök) och returnerar hello svar med felkoden 429. Nu kan också åsidosättas i hello RetryOptions egenskapen i ConnectionPolicy-objektet.
* Cosmos DB Returnerar nu x-ms-begränsning--antal försök och x-ms-throttle-retry-wait-time-ms som hello svarshuvuden i varje begäran toodenote hello begränsning försök antal och hello cummulative hello begära väntat mellan hello försök.
* Borttagna hello RetryPolicy klass och motsvarande hello-egenskap (retry_policy) visas på hello document_client klass och introducerades i stället en RetryOptions klass exponera hello RetryOptions egenskapen ConnectionPolicy klass som kan vara används toooverride Vissa av hello standard alternativen för återförsök.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Tillagda hello stöd för flera regioner databasen konton.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Tillagda hello stöd för tid tooLive(TTL) funktionen för dokument.

### <a name="a-name161161"></a><a name="1.6.1"/>1.6.1
* Felkorrigeringar relaterade tooserver sida partitionering tooallow specialtecken i partitionkey sökväg.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md). 

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Lägg till hash- & intervall partition matchare tooassist med horisontell partitionering program över flera partitioner.

### <a name="a-name142142"></a><a name="1.4.2"/>1.4.2
* Implementera Upsert. Nya metoder för UpsertXXX lägga toosupport Upsert funktion.
* Implementera ID-baserat routning. Inga offentliga API-ändringar, alla ändringar som är interna.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Stöder geospatiala index.
* Verifierar id-egenskapen för alla resurser. ID för resurser kan inte innehålla?, /, #, \, tecken eller sluta med ett blanksteg.
* Lägger till nya rubriken ”index omvandling pågår” tooResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Implementerar V2 indexprincip.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Stöd för proxyanslutning.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Versionen & pensionering datum
Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.

Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som sådan rekommenderas det att du alltid uppgradera toohello senaste SDK version så snart som möjligt. 

Alla begäran tooCosmos databasen med en pensionerad SDK avvisas av hello-tjänsten.

> [!WARNING]
> Alla versioner av hello Azure DocumentDB SDK för Python tidigare tooversion **1.0.0** ska tas bort på **29 februari 2016**. 
> 
> 

<br/>

| Version | Utgivningsdatum | Datumet för tillbakadragandet |
| --- | --- | --- |
| [2.2.0](#2.2.0) |10 maj 2017 |--- |
| [2.1.0](#2.1.0) |01 kan 2017 |--- |
| [2.0.1](#2.0.1) |30 oktober 2016 |--- |
| [2.0.0](#2.0.0) |Den 29 september 2016 |--- |
| [1.9.0](#1.9.0) |07 juli 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.0](#1.7.0) |26 april 2016 |--- |
| [1.6.1](#1.6.1) |08 april 2016 |--- |
| [1.6.0](#1.6.0) |Den 29 mars 2016 |--- |
| [1.5.0](#1.5.0) |03 januari 2016 |--- |
| [1.4.2](#1.4.2) |06 oktober 2015 |--- |
| [1.4.1](#1.4.1) |06 oktober 2015 |--- |
| [1.2.0](#1.2.0) |06 augusti 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.1](#1.0.1) |25 maj 2015 |--- |
| [1.0.0](#1.0.0) |07 april 2015 |--- |
| 0.9.4-prelease |14 januari 2015 |Den 29 februari 2016 |
| 0.9.3-prelease |09 december 2014 |Den 29 februari 2016 |
| 0.9.2-prelease |25 november 2014 |Den 29 februari 2016 |
| 0.9.1-prelease |23 september 2014 |Den 29 februari 2016 |
| 0.9.0-prelease |21 augusti 2014 |Den 29 februari 2016 |

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Se även
toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida. 

