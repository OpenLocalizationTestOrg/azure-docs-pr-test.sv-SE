---
title: aaaIntroduction tooAzure Cosmos DBS tabell API | Microsoft Docs
description: "Lär dig hur du kan använda Azure Cosmos DB toostore och fråga massiva mängder nyckel / värde-data med låg latens med hello populära OSS MongoDB APIs."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: arramac
ms.openlocfilehash: 4c5678898a772808f4bcd1465a23d436b0f8fc0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-table-api"></a>Introduktion tooAzure Cosmos DB: tabell-API

[Azure Cosmos DB](introduction.md) är Microsofts globalt distribuerade databastjänst för flera datamodeller för verksamhetskritiska program. Azure Cosmos-DB tillhandahåller [nyckelfärdig global distributionsplatsen](distribute-data-globally.md), [elastisk skalbarhet av dataflöden och lagringsutrymmen](partition-data.md) över hela världen, en siffra millisekunders latens vid hello 99th percentilen, [fem väldefinierade konsekvensnivåer](consistency-levels.md), och garanteras hög tillgänglighet, alla säkerhetskopieras av [branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos-DB [indexerar automatiskt data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) utan att du toodeal med hantering av schemat och index. Det stöder flera modeller och dokument, nyckelvärde graf och kolumndatamodeller. 

![API för Azure Table Storage och Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos-DB tillhandahåller hello tabell-API (förhandsversion) för program som behöver en nyckelvärdeslagring med flexibelt schema, förutsägbar prestanda, global distributionsplatsen och hög genomströmning. hello tabell API ger hello samma funktioner som Azure Table storage, men utnyttjar hello fördelarna med hello Azure Cosmos-databasmotor. 

Du kan fortsätta toouse Azure Table storage för tabeller med hög lagring och lägre genomströmning krav. Azure Cosmos-DB vi lägger till stöd för storage optimerat tabeller i en kommande uppdatering och befintliga och nya Azure Table storage-konton kommer att uppgraderas tooAzure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Tabell-API:er för premium och standard
Om du använder Azure Table storage, får du följande fördelar genom att flytta tooAzure Cosmos DB ”premium table” preview hello:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (förhandsversion) |
| --- | --- | --- |
| Svarstid | Snabb, men inga övre gränser för svarstid | En siffra millisekunds fördröjning för läsningar och skrivningar, säkerhetskopieras med < 10 ms latens läser och < 15 ms latens skriver vid hello 99th percentilen skaländras, var som helst i hello world |
| Dataflöde | Skalbar, men inte dedikerad dataflödesmodell. Tabeller har en gräns för skalbarhet på 20 000 åtgärder/s | Mycket skalbara med [dedikerat reserverat dataflöde per tabell](request-units.md), som understöds av serviceavtal. Konton har ingen maxgräns för dataflöde och kan hantera >10 miljoner åtgärder/s per tabell |
| Global distribution | En enda region med en valfri läsbar sekundär läsregion för HA. Du kan inte initiera växling vid fel | [Nyckelfärdig global distributionsplatsen](distribute-data-globally.md) från en too30 + regioner, stöd för [automatisk och manuell redundans](regional-failover.md) när som helst var som helst i hello world |
| Indexering | Ett primärt index för PartitionKey och RowKey. Inga sekundära index | Automatisk och fullständig indexering för alla egenskaper utan indexhantering |
| Fråga | Frågekörningen använder index för primär nyckel och genomsöker annars. | Frågor kan dra nytta av automatisk indexering av egenskaper för snabba frågetider. Databasmotorn i Azure Cosmos DB har stöd för mängdfunktioner samt geospatial sökning och sortering. |
| Konsekvens | Stark inom primär region, eventuell med sekundär region | [Fem väldefinierade konsekvensnivåer](consistency-levels.md) tootrade av tillgänglighet, svarstid, genomflöde och konsekvenskontroll utifrån behoven för ditt program |
| Prissättning | Optimerad för lagring  | Optimerad för dataflöde |
| Serviceavtal | 99,9% tillgänglighet | 99,99% tillgänglighet inom en enskild region och möjlighet tooadd flera regioner för högre tillgänglighet. [Branschledande omfattande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/) med allmän tillgänglighet |

## <a name="how-tooget-started"></a>Hur tooget igång

Skapa ett Azure DB som Cosmos-konto i hello [Azure-portalen](https://portal.azure.com), och komma igång med våra [Snabbstart för tabell-API med hjälp av .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Nästa steg

Här följer några tips tooget du startade:
* [Skapa ett .NET-program med hjälp av hello tabell-API](create-table-dotnet.md)
* [Utveckla med hello tabell API i .NET](tutorial-develop-table-dotnet.md)
* [Fråga tabelldata med hjälp av hello tabell-API](tutorial-query-table.md)
* [Hur toosetup Azure Cosmos DB global distributionsplatsen med hello tabell-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB-tabell-API SDK för .NET](table-sdk-dotnet.md)
