---
title: 'Azure Cosmos DB: DocumentDB Java API, SDK & resurser | Microsoft Docs'
description: "Läs mer om hello Java API och SDK inklusive frisläppningsdatum, tillbakadragning datum och ändringar mellan varje version av hello Azure Cosmos DB DocumentDB Java SDK."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a>Azure Cosmos DB: DocumentDB Java SDK viktig information och resurser
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

<tr><td>**SDK-hämtningen**</td><td>[Maven 3.](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**API-dokumentationen**</td><td>[Java API-referensdokumentation](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td>**Bidra tooSDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Kom igång**</td><td>[Kom igång med hello Java SDK](documentdb-java-get-started.md)</td></tr>

<tr><td>**Självstudier för Web app**</td><td>[Utveckling av webbappar med Azure Cosmos DB](documentdb-java-application.md)</td></tr>

<tr><td>**Aktuella stöds runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Viktig information

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Kritiska felkorrigeringar toorequest bearbetning under partition delar.
* Ett problem har åtgärdats med hello starka och BoundedStaleness konsekvensnivåer.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Tillagt stöd för en ny konsekvensnivå kallas ConsistentPrefix.
* Fast ett fel vid läsning av samlingen i sessionsläge.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Aktiverar stöd för partitionerade samling med som låga som 2 500 RU/s och skala i steg om 100 RU/s.
* Fast ett programfel i hello interna sammansättning som kan orsaka NullRef undantag i några frågor.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Fast ett programfel i hello frågan konfigurationen för motorns som kan orsaka undantag för frågor i Gateway-läge.
* Korrigerat några fel i hello session behållaren som kan orsaka ett ”ägare resursen hittades inte” undantag för begäranden omedelbart efter att samlingen har skapats.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Stöd för aggregering frågor (COUNT, MIN, MAX, SUM och AVG) har lagts till. Se [aggregering support](documentdb-sql-query.md#Aggregates).
* Tillagt stöd för ändring feed.
* Stöd för samlingen-kvotinformation via RequestOptions.setPopulateQuotaInfo har lagts till.
* Stöd för lagrad procedur skript loggning med RequestOptions.setScriptLoggingEnabled har lagts till.
* Fast en bugg där frågan i DirectHttps läge låser sig när den påträffar fel begränsning.
* Fast ett programfel i sessionsläge för konsekvenskontroll.
* Fast ett programfel som kan orsaka NullReferenceException i HttpContext när frågehastigheten är för hög.
* Förbättrad prestanda för DirectHttps läge.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Tillagda enkel instans-baserade proxyservrar klientstöd med ConnectionPolicy.setProxy() API.
* Tillagda DocumentClient.close() API tooproperly avstängning DocumentClient-instans.
* Förbättrad prestanda i direktanslutning läge av härledda hello frågeplan från hello interna sammansättningen i stället för hello Gateway.
* Ange FAIL_ON_UNKNOWN_PROPERTIES = false så att användarna inte behöver toodefine JsonIgnoreProperties i sina POJO.
* Omstrukturerade loggning toouse SLF4J.
* Korrigerat några andra fel i konsekvenskontroll läsaren.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Fast ett programfel i hello anslutning management tooprevent anslutning minnesläckor i direktanslutning läge.
* Fast ett programfel i hello ÖVERSTA fråga där den kan utlösa NullReferenece undantag.
* Förbättrad prestanda genom att minska hello antalet nätverk för hello internt.
* Tillagda statuskoden, ActivityID och Begärd URI i DocumentClientException för bättre felsökning.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Ett problem har åtgärdats i hello anslutningshanteringen för stabilitet.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* Stöd för BoundedStaleness konsekvensnivå har lagts till.
* Stöd för direkt anslutning för CRUD-åtgärder för partitionerade samlingar har lagts till.
* Fast ett programfel i frågar en SQL-databas.
* Fast ett programfel i hello sessionscachen där sessionstoken kan ställas in felaktigt.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Tillagt stöd för mellan partition parallella frågor.
* Stöd för upp/ORDER BY-frågor för partitionerade samlingar har lagts till.
* Tillagt stöd för stark konsekvens.
* Stöd för namn baserat på begäranden när du använder direkt anslutning har lagts till.
* Fast toomake ActivityId förblir konsekvent över alla anropsförsök.
* Fast ett programfel relaterade toohello sessionscachen när återskapa en samling med hello samma namn.
* Tillagda Polygon och LineString DataTypes när du anger samling indexering princip för geografiska avgränsningar spatial frågor.
* Fast problem med Java-dokument för Java 1.8.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Fast ett programfel i PartitionKeyDefinitionMap hämta toocache enskilda partitionssamlingar och inte göra extra partitionen viktiga förfrågningar.
* Fast bugg toonot försök igen när en felaktig partitionsnyckelvärde har angetts.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Tillagda hello stöd för flera regioner databasen konton.
* Tillagt stöd för automatiska försök igen på begränsad begäranden med alternativ toocustomize Hej max antal försök och försök max väntetid.  Se RetryOptions och ConnectionPolicy.getRetryOptions().
* Föråldrad IPartitionResolver baserad Anpassad partitionering kod. Använd partitionerade samlingar för högre lagring och genomflöde.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Tillagda försök princip stöd för begränsning.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Extra tid toolive (TTL) stöd för dokument.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Implementerad [partitionerade samlingar](partition-data.md) och [användardefinierade prestandanivåer](performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Fast ett programfel i HashPartitionResolver toogenerate hash-värden i little endian toobe konsekvent med andra SDK: er.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Lägg till hash- & intervall partition matchare tooassist med horisontell partitionering program över flera partitioner.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Implementera Upsert. Nya metoder för upsertXXX lägga toosupport Upsert funktion.
* Implementera ID-baserat routning. Inga offentliga API-ändringar, alla ändringar som är interna.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Versionen hoppas över toobring versionsnumret i enlighet med andra SDK:

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Stöder geospatiala Index
* Verifierar id-egenskapen för alla resurser. ID för resurser kan inte innehålla?, /, #, \, tecken eller sluta med ett blanksteg.
* Lägger till nya rubriken ”index omvandling pågår” tooResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Implementerar V2 indexprincip

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK

## <a name="release--retirement-dates"></a>Versionen & pensionering datum
Microsoft meddelar notification minst **12 månader** innan du tar bort en SDK i ordning toosmooth hello övergången tooa nyare/stöds version.

Nya funktioner och funktionalitet och optimeringar läggs endast toohello aktuella SDK, som sådan rekommenderas det att du alltid uppgradera toohello senaste SDK version så snart som möjligt.

Alla begäran tooCosmos databasen med en pensionerad SDK avvisas av hello-tjänsten.

> [!WARNING]
> Alla versioner av hello DocumentDB SDK för Java tidigare tooversion **1.0.0** ska tas bort på **29 februari 2016**.
> 
> 

<br/>

| Version | Utgivningsdatum | Datumet för tillbakadragandet |
| --- | --- | --- |
| [1.12.0](#1.12.0) |11 juli 2017 |--- |
| [1.11.0](#1.11.0) |10 maj 2017 |--- |
| [1.10.0](#1.10.0) |11 mars 2017 |--- |
| [1.9.6](#1.9.6) |Den 21 februari 2017 |--- |
| [1.9.5](#1.9.5) |31 januari 2017 |--- |
| [1.9.4](#1.9.4) |24 november 2016 |--- |
| [1.9.3](#1.9.3) |30 oktober 2016 |--- |
| [1.9.2](#1.9.2) |28 oktober 2016 |--- |
| [1.9.1](#1.9.1) |26 oktober 2016 |--- |
| [1.9.0](#1.9.0) |03 oktober 2016 |--- |
| [1.8.1](#1.8.1) |30 juni 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.1](#1.7.1) |30 april 2016 |--- |
| [1.7.0](#1.7.0) |27 april 2016 |--- |
| [1.6.0](#1.6.0) |Den 29 mars 2016 |--- |
| [1.5.1](#1.5.1) |Den 31 december 2015 |--- |
| [1.5.0](#1.5.0) |04 december 2015 |--- |
| [1.4.0](#1.4.0) |05 oktober 2015 |--- |
| [1.3.0](#1.3.0) |05 oktober 2015 |--- |
| [1.2.0](#1.2.0) |05 augusti 2015 |--- |
| [1.1.0](#1.1.0) |09 juli 2015 |--- |
| [1.0.1](#1.0.1) |12 maj 2015 |--- |
| [1.0.0](#1.0.0) |07 april 2015 |--- |
| 0.9.5-prelease |09 mar 2015 |Den 29 februari 2016 |
| 0.9.4-prelease |17 februari 2015 |Den 29 februari 2016 |
| 0.9.3-prelease |Den 13 januari 2015 |Den 29 februari 2016 |
| 0.9.2-prelease |19 december 2014 |Den 29 februari 2016 |
| 0.9.1-prelease |19 december 2014 |Den 29 februari 2016 |
| 0.9.0-prelease |10 december 2014 |Den 29 februari 2016 |

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Se även
toolearn mer om Cosmos DB finns [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) sida.

