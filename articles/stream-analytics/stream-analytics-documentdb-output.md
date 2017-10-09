---
title: "aaaJSON utdata för Stream Analytics | Microsoft Docs"
description: "Lär dig hur Stream Analytics kan inrikta dig på Azure Cosmos DB för JSON-utdata för dataarkivering och låg latens frågor för Ostrukturerade JSON-data."
keywords: JSON-utdata
documentationcenter: 
services: stream-analytics,documentdb
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fa717818c839ecd7a60fcee33d22011990fd5878
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Mål Azure Cosmos DB för JSON-utdata från Stream Analytics
Stream Analytics kan rikta [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) att aktivera arkivering och låg latens datafrågor på Ostrukturerade JSON-data för JSON-utdata. Det här dokumentet beskrivs några metoder för att implementera den här konfigurationen.

För de som inte är bekant med Cosmos DB, ta en titt på [Azure Cosmos DB Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/documentdb/) tooget igång. 

Obs: Mongo DB API-baserad Cosmos DB samlingar stöds inte för närvarande. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Grunderna i Cosmos DB som ett mål för utdata
hello Azure Cosmos DB utdata i Stream Analytics kan skriva strömmen bearbetning resultatet som JSON-utdata till Cosmos-DB-samling(ar). Stream Analytics skapar inte samlingar i databasen, i stället att kräva att du toocreate dem gång. Detta är så att hello fakturering kostnaderna för Cosmos DB samlingar är transparent tooyou och så att du kan finjustera hello prestanda, konsekvens och kapacitet för samlingar med direkt hello [Cosmos DB-API: er](https://msdn.microsoft.com/library/azure/dn781481.aspx). Vi rekommenderar en Cosmos DB databas per streaming job toologically separata samlingar för ett direktuppspelningsjobb.

Vissa av hello Cosmos DB samling alternativ beskrivs nedan.

## <a name="tune-consistency-availability-and-latency"></a>Finjustera konsekvens, tillgänglighet och svarstid
toomatch din programkrav, Cosmos DB kan du toofine finjustera hello-databasen och samlingar och kontrollera avvägningarna mellan konsekvens, tillgänglighet och svarstid. Beroende på vilka nivåer av läskonsekvens scenariot behov mot läsa och skriva latens, du kan välja en konsekvensnivå på ditt konto. Som standard kan Cosmos DB också synkron indexering på varje CRUD åtgärden tooyour samling. Det här är en annan användbar alternativet toocontrol hello skrivning/läsning prestanda i Cosmos-databasen. Mer information om det här avsnittet granska hello [ändra din databas och fråga konsekvensnivåer](../documentdb/documentdb-consistency-levels.md) artikel.

## <a name="upserts-from-stream-analytics"></a>Upserts från Stream Analytics
Stream Analytics-integrering med Cosmos DB tillåter tooinsert eller uppdatera poster i din Cosmos-DB-samling baserat på en viss dokument-ID-kolumn. Det är också hänvisade tooas en *Upsert*.

Stream Analytics använder optimistisk Upsert lösning där uppdateringar görs endast när insert misslyckas på grund av tooa dokument-ID-konflikt. Den här uppdateringen utförs av Stream Analytics som en korrigering, så gör deluppdateringar toohello dokument, t.ex. lägga till nya egenskaper eller ersätta en befintlig egenskap utförs inkrementellt. Observera att ändringar i hello värdena i matrisen egenskaper i JSON-dokumentet medföra hello hela matrisen komma över, d.v.s. hello matrisen inte samman.

## <a name="data-partitioning-in-cosmos-db"></a>Datapartitionering i Cosmos-DB
Cosmos DB [partitionerade samlingar](../cosmos-db/partition-data.md) är hello rekommenderat tillvägagångssätt för partitionering av dina data. 

För enkel Cosmos DB samlingar kan Stream Analytics du fortfarande toopartition dina data baserat på både hello frågemönster och prestanda för programmet. Varje samling kan innehålla in too10GB av data (max) och för närvarande är det inget sätt tooscale (eller dataspill) en samling. Skala ut Stream Analytics kan du för toowrite toomultiple samlingar med ett givet prefix (se användningsinformation nedan). Stream Analytics använder hello konsekvent [hash-Partition matcharen](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategi baserat på hello användaren får PartitionKey kolumnen toopartition dess utdata-poster. hello antal samlingar med hello ges prefix hello strömning jobbets starttid används som hello utdata partitionsantal toowhich hello jobbet skriver tooin parallell (Cosmos DB samlingar = utdata partitioner). För en enda samling med lazy indexering detta endast infogar, om 0,4 som kan förväntas MB/s genomströmning för skrivning. Med hjälp av flera samlingar kan du tooachieve högre genomflöde och bättre kapacitet.

Om du planerar tooincrease hello partitionsantal i hello framtida, kanske du måste toostop jobbet partitionera om hello data från din befintliga samlingar till nya samlingar och sedan starta om hello Stream Analytics-jobbet. Mer information om att använda PartitionResolver och nytt partitionering tillsammans med exempelkod inkluderas i en e-post. hello artikel [partitionering och skalning i Cosmos DB](../documentdb/documentdb-partition-data.md) innehåller även information om detta.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB inställningar för JSON-utdata
Skapa Cosmos DB som utdata i Stream Analytics genererar en fråga om information som visas nedan. Det här avsnittet innehåller en förklaring av hello egenskaper definition.

Partitionerad samling | Flera ”enskild Partition” samlingar
---|---
![documentdb stream analytics utdata skärmen](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![documentdb stream analytics utdata skärmen](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> Hej **flera ”enkel” partitionssamlingar** scenariot kräver en partitionsnyckel och är en konfiguration som stöds. 

* **Utdata Alias** – ett alias toorefer detta utdata i frågan ASA  
* **Kontonamn** – hello namn eller endpoint-URI för hello Cosmos-DB-konto.  
* **Kontot nyckeln** – hello delade åtkomstnyckeln för hello Cosmos-DB-konto.  
* **Databasen** – hello Cosmos-DB-databasnamn.  
* **Samlingsnamnsmönstret** – hello samlingens namn eller deras mönster för hello samlingar toobe används. Hej samlingsnamnsformatet kan konstrueras med valfritt {partition}-token hello där partitionerna börjar från 0. Följande är exempel giltiga indata:  
  1\) MyCollection – en samling med namnet ”MyCollection” måste finnas.  
  2\) MyCollection {partition} – dessa samlingar måste finnas – ”MyCollection0”, ”MyCollection1”, ”MyCollection2” och så vidare.  
* **Partitionera nyckeln** – det är valfritt. Det här krävs bara om du använder en {partition}-token i din samlingsnamnsmönstret. hello namnet på hello fält i utdata händelser som används toospecify hello nyckel för att partionera utdata över samlingarna. Enda samling utdata för en godtycklig utdatakolumnen kan vara används t.ex. PartitionId.  
* **Dokumentera ID** – det är valfritt. hello namnet på hello-fältet i utdatahändelserna används toospecify hello primärnyckel operations baseras på vilka insert eller update.  
