---
title: "Gränserna för Azure SQL Database | Microsoft Docs"
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
ms.openlocfilehash: e75458db79e6c15f8d5155b71f04a20fa21818ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-sql-database-resource-limits"></a>Gränserna för Azure SQL-databas
## <a name="overview"></a>Översikt
Azure SQL Database hanterar resurser som är tillgängliga för en databas på två olika sätt: **resurser styrning** och **tvingande gränser**. Det här avsnittet beskriver dessa två huvudsakliga områden av resurshantering.

## <a name="resource-governance"></a>Resurs-styrning
En av designmålen för tjänstnivåerna Basic, Standard, Premium och Premium-RS är för Azure SQL Database fungerar som om databasen körs på sin egen dator isoleras från andra databaser. Resurs-styrning emulerar problemet. Om aggregerade resursanvändningen når maximal tillgänglig CPU, minne, logg-i/o och Data i/o-resurser tilldelas databasen resource styrning köer frågor körning och tilldela resurser till köade frågor som de Frigör.

Som på en särskild dator som använder alla tillgängliga resurser resultat i en längre körning av frågor som körs som kan leda till kommando-timeout på klienten. Program med aggressiv logik och program som kör frågor mot databasen med en hög frekvens kan uppstå felmeddelanden när du försöker köra nya frågor när antalet samtidiga begäranden har uppnåtts.

### <a name="recommendations"></a>Rekommendationer:
Övervaka resursutnyttjandet och genomsnittliga svarstider för frågor när närmar sig maximal användning av en databas. När den påträffar ökad latens i fråga har vanligtvis tre alternativ:

1. Minska antalet inkommande begäranden till databasen för att förhindra timeout och stapel upp med begäranden.
2. Tilldela en högre prestandanivå för databasen.
3. Optimera frågor för att minska resursanvändningen för varje fråga. Mer information finns i avsnittet frågan finjustera/Hinting i Azure SQL Database-Prestandaråd artikeln.

## <a name="enforcement-of-limits"></a>Tillämpning av gränser
Resurser än CPU, minne, logg-i/o och Data i/o tillämpas genom att neka nya begäranden när gränsen nås. När en databas når den konfigurerade maximala storleksgränsen, misslyckas infogningar och uppdateringar som ökar datastorleken när du väljer och borttagningar fortsätter att fungera. Klienter tar emot en [felmeddelande](sql-database-develop-error-messages.md) beroende på gränsen har nåtts.

Till exempel är antalet anslutningar till en SQL-databas och antalet samtidiga begäranden som kan bearbetas begränsade. SQL-databas kan antalet anslutningar till databasen måste vara större än antalet samtidiga begäranden till stöd för anslutningspooler. Antalet anslutningar som är tillgängliga kan enkelt kontrolleras av programmet, är antalet parallella begäranden ofta svårare att beräkna och för att styra gånger. Särskilt under belastning belastning när programmet antingen skickar för många begäranden eller databasen når sin resurs begränsar och startar samlas arbetstrådar på grund av längre frågor som körs, kan fel uppstå.

## <a name="service-tiers-and-performance-levels"></a>Tjänste- och prestandanivåer
Det finns servicenivåer och prestandanivåer för både enskilda databaser och elastiska pooler.

### <a name="single-databases"></a>Enkla databaser
För en enskild databas definieras gränserna för en databas av service tjänstnivå och prestandanivå databasnivå. I följande tabell beskrivs egenskaperna för Basic, Standard, Premium och RS för Premium-databaser på olika prestandanivåer.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> Kunder som använder P11 och P15 prestandanivåer kan använda upp till 4 TB med lagring utan extra kostnad. Det här alternativet om 4 TB är tillgängliga i följande områden: oss East2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, söder Östasien, östra, Östra Australien, Kanada Central och Kanada Öst.
>

### <a name="elastic-pools"></a>Elastiska pooler
[Elastiska pooler](sql-database-elastic-pool.md) dela resurser över databaser i poolen. I följande tabell beskrivs egenskaperna för Basic, Standard, Premium och Premium-RS elastiska pooler.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

En expanderad definition av varje resurs som visas i föregående tabeller finns beskrivningar i [funktioner och begränsningar](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). En översikt över tjänstnivåer, se [Azure SQL Database servicenivåer och prestandanivåer](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Andra begränsningar för SQL-databas
| Område | Gräns | Beskrivning |
| --- | --- | --- |
| Databaser med hjälp av automatisk export per prenumeration |10 |Automatiserad export kan du skapa ett anpassat schema för att säkerhetskopiera SQL-databaser. Förhandsversionen för den här funktionen kommer att avslutas på 1 mars 2017.  |
| Databaser per server |Upp till 5 000 |Upp till 5000 databaser tillåts per server. |
| Dtu: er per server |45000 |45000 dtu: er tillåts per server för att etablera fristående databaser och elastiska pooler. Det totala antalet fristående databaser och pooler tillåts per server begränsas bara av antalet server dtu: er.  

> [!IMPORTANT]
> Azure SQL Database automatiserad Export är nu i förhandsversionen och ska tas bort på 1 mars 2017. Starta 1 December 2016 kommer du inte längre att kunna konfigurera automatisk export på en SQL-databas. Alla dina befintliga automatiserad export jobben fortsätter att fungera förrän den 1 mars 2017. Du kan använda efter den 1 December 2016 [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md) eller [Azure Automation](../automation/automation-intro.md) att arkivera SQL-databaserna regelbundet med hjälp av PowerShell regelbundet enligt ett schema som du väljer. För ett exempelskript som du kan hämta den [exempel på skript från GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Resurser
[Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md)

[Azure SQL Database servicenivåer och prestandanivåer](sql-database-service-tiers.md)

[Felmeddelanden för SQL Database-klientprogram](sql-database-develop-error-messages.md)
