---
title: aaaPerformance rekommendationer - Azure SQL Database | Microsoft Docs
description: "hello Azure SQL Database innehåller rekommendationer för SQL-databaser som kan förbättra aktuella frågeprestanda."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 77db338a0a395aec78c9e02849ae5ba4f2d01680
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-recommendations"></a>Rekommendationer

Azure SQL Database lär sig och anpassar sig med ditt program och innehåller anpassade rekommendationer som gör att du toomaximize hello prestanda för SQL-databaser. Prestandan utvärderas kontinuerligt genom att analysera din användningshistorik för SQL-databas. hello rekommendationerna baseras på ett unikt arbetsbelastning mönster för databasen och förbättra prestandan.

> [!NOTE]
> Rekommenderade sätt att använda rekommendationer är genom att aktivera 'Automatisk justering' på din databas. Mer information finns i [automatisk justering](sql-database-automatic-tuning.md).
>

## <a name="create-index-recommendations"></a>Skapa index-rekommendationer
Azure SQL Database övervakar hello frågor som körs kontinuerligt och identifierar hello index som kan förbättra hello prestanda. När tillräckligt med förtroende skapas som ett visst index saknas, en ny **skapa index** rekommendation kommer att skapas. Azure SQL Database versioner förtroende genom att uppskatta prestandaindex vinst hello skulle ta över tiden. Beroende på hello uppskattad prestandafördelar kategoriseras rekommendationer som hög, medel eller låg. 

Index som skapats med hjälp av rekommendationer flaggas alltid som auto_created index. Du kan se vilka index är auto_created genom att titta på sys.indexes vyn. Automatiskt skapade index blockera inte ALTER/Byt namn på kommandon. Om du försöker toodrop hello kolumn med ett automatiskt skapat index över den hello kommandot skickar och hello automatiskt skapat index släpps med hello-kommando. Vanliga index skulle blockera hello ALTER/Byt namn på kommandot för kolumner som indexeras.

När hello skapar indexrekommendationen tillämpas, Azure SQL-databasen kommer att jämföra prestanda för hello frågor med hello baslinje-prestanda. Om nytt index föras förbättringar i hello prestanda, rekommendation flaggas lyckas och rapporten blir tillgängliga. Hello-index inte sätta hello fördelar, om det kommer att återställas automatiskt. Det här sättet Azure SQL Database ser till att använda rekommendationer endast förbättras hello databasens prestanda.

Alla **skapa index** rekommendation har en tillbaka av princip som inte tillåter att tillämpa hello rekommendation om hello databas eller poolen DTU-användningen är över 80% i sista 20 minuter eller om hello lagring är över 90% av användning. I det här fallet ska hello rekommendation skjutas upp.

## <a name="drop-index-recommendations"></a>DROP index-rekommendationer
Dessutom analyserar toodetecting saknas index, Azure SQL Database kontinuerligt hello prestanda för katastrofåterställning. Om inte index används rekommenderar Azure SQL Database släpper den. Du bör släppa ett index i två fall:
* Index är en dubblett av ett annat index (samma indexerade och ingår kolumn, partition schemat och filter)
* Index används inte för långa perioder (93 dagar)

DROP index-rekommendationer kan också gå igenom hello verifiering efter implementering. Om bättre prestanda för hello blir hello rapporten tillgänglig. Om prestandaförsämring upptäcks kommer rekommendation att återställas.


## <a name="parameterize-queries-recommendations"></a>Parameterstyra frågor rekommendationer
**Parameterstyra frågor** rekommendationer visas när du har en eller flera frågor som är kompileras ständigt men slutar med hello samma plan för körning av frågan. Det här villkoret öppnas en möjlighet tooapply tvingas parameterisering, vilket innebär att frågeplaner toobe cachelagras och återanvändas i hello framtida förbättra prestandan och minska Resursanvändning. 

Varje fråga som ursprungligen utfärdade mot SQL Server måste kompileras toobe toogenerate en åtgärdsplan. Varje genererade plan läggs toohello plancache och efterföljande körningar av hello samma fråga kan återanvända den här planen från hello cache, eliminera hello behovet av ytterligare sammanställning. 

Program som skickar frågor, bland annat icke-parameteriserat värden, kan leda tooa prestanda försämras där för varje sådan fråga med olika parametervärden hello åtgärdsplan kompileras igen. I många fall hello samma frågor med olika värden generera hello samma körning planer, men dessa planer läggs fortfarande separat toohello plancache. Kompilera körning planer använder databasresurser kan öka hello varaktighet tid och spill hello frågeplan cache orsakar planer toobe avlägsnas från hello cache. Det här beteendet av SQL Server kan ändras genom att ange hello Tvingad parameterization alternativet på hello-databasen. 

toohelp som du beräkna hello effekten av den här rekommendationen du tillhandahålls med en jämförelse mellan hello faktiska CPU användnings- och hello planerat CPU-användning (som om hello rekommendation valdes). Dessutom tooCPU besparingar din fråga varaktighet minskas för hello tid i kompilering. Också är mycket mindre försämras på plancache, så att merparten av hello planer toostay i cachen och återanvändas. Du kan använda den här rekommendationen snabbt och enkelt genom att klicka på hello **tillämpa** kommando. 

När du använder den här rekommendationen aktiveras framtvingad parameterization inom minuter på din databas som startar hello övervakning bearbetar som varar cirka 24 timmar. När den här tiden kommer du att vara kan toosee hello verifieringsrapport som visar CPU-användningen för din databas 24 timmar innan och efter hello rekommendation har tillämpats. SQL Database Advisor har en säkerhetsmekanism som återställs automatiskt hello tillämpas rekommendation om en prestanda regression har upptäckts.

## <a name="fix-schema-issues-recommendations"></a>Åtgärda problem rekommendationer för schemat
**Åtgärda problem med schemat** rekommendationer visas när hello SQL Database-tjänsten meddelanden avvikelser i hello antal schema-relaterade SQL-fel som händer på din Azure SQL-databas. Denna rekommendation visas vanligen när databasen påträffar flera schema-relaterade fel (Ogiltigt kolumnnamn, Ogiltigt objektnamn osv.) inom en timme.

”Schema” problem är en typ av syntaxfel i SQL Server som inträffar när hello definitionen av hello SQL-fråga och hello definition av hello databasschemat inte är justerade. Till exempel en av hello kolumner som förväntades av hello frågan kanske saknas i måltabellen för hello, och vice versa. 

”Åtgärda problemet schema” rekommendation visas när Azure SQL Database-tjänsten meddelanden avvikelser i hello antal schema-relaterade SQL-fel som händer på din Azure SQL-databas. följande tabell visar hello fel som är relaterade tooschema problem hello:

| SQL-felkod | Meddelande |
| --- | --- |
| 201 |Proceduren eller funktionen '*'förväntar parametern'*', som inte har angetts. |
| 207 |Ogiltigt kolumnnamn ' *'. |
| 208 |Ogiltigt objektnamn ' *'. |
| 213 |Kolumnnamnet eller antalet angivna värden stämmer inte med tabelldefinitionen. |
| 2812 |Det gick inte att hitta lagrade proceduren ' *'. |
| 8144 |Proceduren eller funktionen * har för många argument. |

## <a name="next-steps"></a>Nästa steg
Övervaka dina rekommendationer och fortsätta tooapply dem toorefine prestanda. Databasarbetsbelastningar är dynamiska och ändra kontinuerligt. SQL Database advisor fortsätter toomonitor och ger rekommendationer som kan förbättra dina databasprestanda. 

* Se [rekommendationer i hello Azure-portalen](sql-database-advisor-portal.md) stegvisa instruktioner för hur toouse rekommendationer i hello Azure-portalen.
* Se [insikter i frågeprestanda](sql-database-query-performance.md) toolearn om och visa hello prestandapåverkan top-frågor.

## <a name="additional-resources"></a>Ytterligare resurser
* [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [SKAPA INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)

