---
title: "aaaAzure gränserna för SQL-databasen | Microsoft Docs"
description: "Den här sidan beskrivs några vanliga gränserna för Azure SQL Database."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2017
ms.author: carlrab
ms.openlocfilehash: 3e7be4fdc74e802d37274690531caaf138bcedb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-resource-limits"></a>Gränserna för Azure SQL-databas
## <a name="overview"></a>Översikt
Azure SQL Database hanterar hello resurser tillgängliga tooa databasen på två olika sätt: **resurser styrning** och **tvingande gränser**. Det här avsnittet beskriver dessa två huvudsakliga områden av resurshantering.

## <a name="resource-governance"></a>Resurs-styrning
En av hello designmålen för hello Basic, Standard, Premium och Premium-RS tjänstnivåer är Azure SQL Database toobehave som om hello databas körs på sin egen dator isoleras från andra databaser. Resurs-styrning emulerar problemet. Om hello samman resursutnyttjande når hello högsta tillgängliga CPU, minne, logg-i/o och Data i/o resurser tilldelade toohello databasen resource styrning köer frågor i körningen och tilldela toohello i kö frågor som de frigöra resurser.

Som på en särskild dator som använder alla tillgängliga resurser resultat i en längre körning av frågor som körs som kan leda till kommando-timeout på hello-klienten. Program med aggressiv logik och program som kör frågor mot hello-databasen med en hög frekvens kan uppstå felmeddelanden vid tooexecute nya frågor när hello samtidiga begäranden har nåtts.

### <a name="recommendations"></a>Rekommendationer:
Övervaka hello resursutnyttjande och hello genomsnittlig svarstid för frågor när snart hello maximal användning av en databas. När den påträffar ökad latens i fråga har vanligtvis tre alternativ:

1. Minska hello antalet inkommande begäranden toohello databasen tooprevent timeout och hello stapel upp med begäranden.
2. Tilldela en högre prestanda nivå toohello databas.
3. Optimera frågor tooreduce hello resursanvändningen för varje fråga. Mer information finns i hello frågan finjustera/Hinting avsnitt i hello Azure SQL Database-Prestandaråd artikeln.

## <a name="enforcement-of-limits"></a>Tillämpning av gränser
Resurser än CPU, minne, logg-i/o och Data i/o tillämpas genom att neka nya begäranden när gränsen nås. När en databas når hello konfigurerade maximala storleken som tillåts, infogningar och uppdateringar som ökar datastorleken misslyckas medan väljer och borttagningar fortsätter toowork. Klienter tar emot en [felmeddelande](sql-database-develop-error-messages.md) beroende på hello gräns som har uppnåtts.

Till exempel begränsas hello antalet anslutningar tooa SQL-databas och hello antalet samtidiga begäranden som kan bearbetas. SQL-databas kan hello antalet anslutningar toohello databasen toobe större än hello antalet samtidiga begäranden toosupport anslutningspooler. Hello antalet anslutningar som är tillgängliga kan enkelt kontrolleras av programmet hello, är hello antalet parallella begäranden ofta gånger svårare tooestimate och toocontrol. Särskilt under belastning belastning när programmet hello skickar antingen för många begäranden eller hello databasen når gränsen resurs och startar samlas arbetstrådar på grund av toolonger frågor som körs, fel kan uppstå.

## <a name="service-tiers-and-performance-levels"></a>Tjänste- och prestandanivåer
Det finns servicenivåer och prestandanivåer för både enskilda databaser och elastiska pooler.

### <a name="single-databases"></a>Enkla databaser
För en enskild databas definieras hello gränserna för en databas av hello databasen tjänstnivå och prestandanivå servicenivå. hello i den följande tabellen beskrivs hello egenskaper för Basic, Standard, Premium och RS för Premium-databaser på olika prestandanivåer.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Kunder som använder P11 och P15 prestandanivåer kan använda upp too4 TB ingår lagringsutrymme utan extra kostnad. Det här alternativet för 4 TB är tillgängliga i hello följande områden: oss East2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, söder Östasien, östra, Östra Australien, Kanada Central och Kanada Öst.
>

### <a name="elastic-pools"></a>Elastiska pooler
[Elastiska pooler](sql-database-elastic-pool.md) dela resurser över databaser i hello pool. hello i den följande tabellen beskrivs hello egenskaper för Basic, Standard, Premium och Premium-RS elastiska pooler.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

En expanderad definition av varje resurs som visas i föregående tabeller i hello finns hello beskrivningar i [funktioner och begränsningar](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). En översikt över tjänstnivåer, se [Azure SQL Database servicenivåer och prestandanivåer](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Andra begränsningar för SQL-databas
| Område | Gräns | Beskrivning |
| --- | --- | --- |
| Databaser med hjälp av automatisk export per prenumeration |10 |Automatiserad export kan du toocreate ett anpassat schema för att säkerhetskopiera SQL-databaser. hello förhandsversionen av den här funktionen kommer att avslutas på 1 mars 2017.  |
| Databaser per server |Konfigurera too5000 |Konfigurera too5000 tillåts databaser per server. |
| Dtu: er per server |45000 |45000 dtu: er tillåts per server för att etablera fristående databaser och elastiska pooler. hello totala antalet fristående databaser och pooler tillåts per server begränsas bara av hello antalet server dtu: er.  

> [!IMPORTANT]
> Azure SQL Database automatiserad Export är nu i förhandsversionen och ska tas bort på 1 mars 2017. Startar 1 December 2016 kommer du inte längre att kunna tooconfigure automatiserad export på en SQL-databas. Toowork fortsätter alla dina befintliga automatiserad export-jobb förrän den 1 mars 2017. Du kan använda efter den 1 December 2016 [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md) eller [Azure Automation](../automation/automation-intro.md) tooarchive SQL-databaserna regelbundet med PowerShell med jämna mellanrum enligt tooa schema du väljer. Du kan hämta hello för ett exempelskript [exempel på skript från GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Resurser
[Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md)

[Azure SQL Database servicenivåer och prestandanivåer](sql-database-service-tiers.md)

[Felmeddelanden för SQL Database-klientprogram](sql-database-develop-error-messages.md)
