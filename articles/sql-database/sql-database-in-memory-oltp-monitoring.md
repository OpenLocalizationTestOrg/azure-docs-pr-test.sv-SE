---
title: aaaMonitor XTP InMemory-lagringen | Microsoft Docs
description: "Uppskattning och övervaka XTP InMemory-lagringen använder kapacitet. Åtgärda felet kapacitet 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Övervaka minnesintern OLTP-lagring
När du använder [Minnesintern OLTP](sql-database-in-memory.md), minnesoptimerade tabeller och tabellvariabler finns i InMemory-OLTP-lagring. Varje premiumnivån har en maximal Minnesintern OLTP lagringsstorlek, som dokumenteras i hello [SQL Database servicenivåer artikel](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). När den här gränsen överskrids, infoga och uppdatera operations startar (med fel 41823). Då kommer du behöver tooeither ta bort data tooreclaim minne eller uppgradera hello prestandanivån för din databas.

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a>Avgöra om data får plats inom hello InMemory-lagringen linjeslut
Fastställa hello lagring cap: Kontakta hello [SQL Database servicenivåer artikel](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) för hello lagring caps av hello olika Premium tjänstnivåer.

Uppskatta minne krav för en minnesoptimerad tabell fungerar hello samma har sätt för SQL Server som den i Azure SQL Database. Ta ett par minuter tooreview avsnittet på [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Observera att hello tabell och variabeln tabellraderna samt index, räknas in Hej max användaren datastorlek. ALTER TABLE måste dessutom tillräckligt med utrymme toocreate en ny version av hello hela tabellen och dess index.

## <a name="monitoring-and-alerting"></a>Övervakning och avisering
Du kan övervaka InMemory-lagringsanvändning som en procentandel av hello [lagring fjärrskrivbordsanslutning för din prestandanivån](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) i hello Azure [portal](https://portal.azure.com/): 

* Leta upp hello resurs användning rutan på hello databasbladet och klicka på Redigera.
* Välj hello mått `In-Memory OLTP Storage percentage`.
* tooadd en avisering klickar du på hello resursutnyttjande rutan tooopen hello mått bladet och sedan klicka på Lägg till avisering.

Eller Använd hello följande fråga tooshow hello InMemory-lagringsanvändningen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Korrigera minnet är slut situationer - fel 41823
Kör minnet är slut resultat i INSERT-, UPDATE- och skapa åtgärder misslyckas med felmeddelandet 41823.

Felmeddelandet 41823 innebär att hello minnesoptimerade tabeller och tabellvariabler har överskridit hello maxstorleken.

tooresolve felet, antingen:

* Ta bort data från hello minnesoptimerade tabeller, potentiellt avlasta hello data tootraditional, diskbaserade tabeller. eller,
* Uppgradera hello service tier tooone med tillräckligt med InMemory-lagringen för hello data behöver du tookeep i minnesoptimerade tabeller.

## <a name="next-steps"></a>Nästa steg
För att övervaka information, se [övervakning Azure SQL Database med dynamiska hanteringsvyer](sql-database-monitoring-with-dmvs.md).
