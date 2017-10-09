---
title: "SQL Database: Vad är en DTU? | Microsoft Docs"
description: "Förstå vad en Azure SQL Database-transaktionsenhet är."
keywords: databasalternativ, databasprestanda
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: 89e3e9ce-2eeb-4949-b40f-6fc3bf520538
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 04/13/2017
ms.author: carlrab
ms.openlocfilehash: ba1fe580b32826259edaf6c8dc124faaf5b3d234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Förklarar vad databastransaktionsenheter (DTU, Database Transaction Unit) och elastiska databastransaktionsenheter (eDTU) är.
Den här artikeln förklarar Database Transaction Units (Dtu) och elastiska Datatransaktionsenheter (edtu: er) och vad som händer när du klickar på hello högsta dtu: er eller edtu: er.  

## <a name="what-are-database-transaction-units-dtus"></a>Vad är databastransaktionsenheter (DTU:er)?
För en enda Azure SQL-databas på specifika prestandanivåer inom en [tjänstnivån](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels), Microsoft garanterar en viss nivå av resurser för den här databasen (oberoende av andra databas hello Azure-molnet), och tillhandahålla en förutsägbar nivå av prestanda. Den här mängden resurser beräknas som ett antal Database Transaction Units eller dtu: er och är en blandning av processor, minne, i/o (data och transaktionen loggning i/o). hello förhållandet bland resurserna har ursprungligen bestäms av ett [OLTP-arbetsbelastning benchmark](sql-database-benchmark-overview.md) utformats toobe typiska för verkliga OLTP-arbetsbelastningar. När din arbetsbelastning överskrider hello mängden någon av dessa resurser, är din dataflödet begränsad – vilket resulterar i långsammare prestanda och timeout. hello-resurser som används av din arbetsbelastning påverkar inte hello resurser tillgängliga tooother SQL-databaser i hello Azure-molnet och hello-resurs som används av andra arbetsbelastningar påverkar inte hello resurser tillgängliga tooyour SQL-databas.

![avgränsningsram](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu: er är mest användbara för att förstå hello relativa mängd resurser mellan Azure SQL-databaser på olika prestandanivåer och servicenivåer. Till exempel dubblerade hello dtu: er genom att öka hello prestandanivå för en databas är lika toodoubling hello uppsättning tillgängliga toothat resursdatabasen. En premium P11-databas med 1 750 DTU:er erbjuder exempelvis 350 gånger mer DTU-beräkningskraft än en Basic-databas med 5 DTU:er.  

toogain djupare inblick i hello-resursförbrukning (DTU) för din arbetsbelastning, Använd [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) till:

- Identifiera hello de vanligaste frågorna med CPU/varaktighet/körningar som potentiellt kan anpassas för bättre prestanda. Till exempel en i/o-intensiva fråga kan dra nytta av hello användning av [i minnet optimeringstekniker](sql-database-in-memory.md) toomake bättre användning av hello tillgängligt minne på en viss tjänst tjänstnivå och prestandanivå nivå.
- Öka detaljnivån hello information om en fråga kan visa text och historik över resursutnyttjandet.
- Rekommendationer som visar åtgärder som utförs av för åtkomst av prestandajustering [SQL Database Advisor](sql-database-advisor.md).

Du kan [ändra tjänstnivå](sql-database-service-tiers.md) när som helst med minimal avbrottstid tooyour program (vanligtvis medelvärdet under fyra sekunder). För många företag och appar som kan toocreate databaser och reglera prestandan uppåt eller nedåt på begäran är tillräckligt, särskilt om användningsmönster är relativt förutsägbara. Men om du har oförutsägbara användningsmönster, kan det vara svårt toomanage kostnader och din affärsmodell. I det här scenariot använder du en elastisk pool med ett visst antal edtu: er som ska delas mellan flera databaser i poolen hello.

![Introduktion tooSQL databasen: enkel databas dtu: er efter nivå](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Vad är elastiska databastransaktionsenheter (eDTU:er)
Snarare än innehåller en särskild uppsättning resurser (Dtu) tooa SQL-databas som alltid är tillgänglig oavsett om de inte behövs kan du placera databaser i en [elastisk pool](sql-database-elastic-pool.md) på en SQL Database-server som delar en pool av resurser bland dessa databas. hello delade resurser i en elastisk pool som mäts med elastiska Datatransaktionsenheter eller edtu: er. Elastiska pooler ger en enkel och kostnadseffektiv lösning toomanage hello prestandamål för flera databaser som har vitt spridda och oförutsägbara användningsmönster. I en elastisk pool kan du garantera att ingen en databas använder alla hello resurser i hello poolen och även som en minimal mängd resurser är alltid tillgänglig tooa databas i en elastisk pool. Se [elastiska pooler](sql-database-elastic-pool.md) för mer information.

![Introduktion tooSQL databasen: edtu: er efter nivå](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

En pool tilldelas ett bestämt antal eDTU:er till ett fast pris. Inom hello elastisk pool ges enskilda databaser hello flexibilitet tooauto skala inom hello konfigurerats gränser. Hårt belastad, kan en databas använda flera edtu: er toomeet begäran när databaser under lätta belastningar förbrukar mindre in toohello plats att använda databaser under ingen inga edtu: er. Som etablerar resurser för hello hela poolen i stället per databas, administrativa uppgifter är förenklade och du har en förutsägbar budget för hello poolen.

Ytterligare edtu: er kan läggas tooan befintlig pool utan avbrott i databasen och utan att påverka på hello-databaser i hello pool. Om eDTU:er sedan inte längre behövs, kan de tas bort från en befintlig pool när som helst. Du kan lägga till eller ta bort databaser toohello pool eller begränsa hello mängden edtu: er som en databas kan använda under hög belastning tooreserve edtu: er för andra databaser. Om en databas förutsägbart under-använder resurser, som du kan flytta från hello poolen och konfigurera den som en enskild databas med förutsägbar mängden resurser som krävs.

## <a name="how-can-i-determine-hello-number-of-dtus-needed-by-my-workload"></a>Hur vet hello antalet dtu: er som krävs av min arbetsbelastning?
Om du söker toomigrate en befintlig lokal eller SQL Server virtuella arbetsbelastning tooAzure SQL-databas, kan du använda hello [DTU Calculator](http://dtucalculator.azurewebsites.net/) tooapproximate hello antalet dtu: er som behövs. Du kan använda för en befintlig Azure SQL Database-arbetsbelastning [SQL Database Query Performance Insight](sql-database-query-performance.md) toounderstand din databas resurs förbrukning (Dtu) tooget djupare insyn i hur toooptimize din arbetsbelastning. Du kan också använda hello [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV tooget hello förbrukning resursinformation för hello senast en timme. Du kan också hello katalogvyn [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) kan också vara efterfrågade tooget hello samma data för hello senaste 14 dagarna, men med en lägre återgivningen av fem minuter långa medelvärden.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hur vet jag om jag kan dra nytta av en elastisk pool med resurser?
Pooler lämpar sig för ett stort antal databaser med specifika användningsmönster. För en viss databas kännetecknas det här mönstret av låg genomsnittlig användning med relativt ovanliga användningstoppar. SQL-databasen automatiskt utvärderar hello historiska resursanvändningen för databaser i en befintlig SQL-databasserver och rekommenderar hello poolen konfigurationen i hello Azure-portalen. Mer information finns i [När ska jag använda en elastisk pool?](sql-database-elastic-pool.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Vad händer när jag når maxgränsen för antalet DTU:er?
Prestanda är kalibrera och styrda tooprovide hello behövs resurser toorun din databas arbetsbelastning in toohello max gränser som är tillåten för din valda nivån/prestandanivå. Om din arbetsbelastning är träffa hello gränser i någon av CPU/Data IO/logg-i/o-gränser, kan du fortsätta tooreceive hello resurser på hello högsta tillåtna nivå, men du är sannolikt toosee ökat svarstiderna för dina frågor. Dessa begränsningar resulterar inte i fel, men i stället en nedgång i hello arbetsbelastning, om inte hello långsammare blir så allvarliga att frågor startar tidsinställning. Om du når gränsen för det högsta antalet tillåtna samtidiga användarsessioner/begäranden (arbetstrådar) returneras explicita fel. Information om gränserna för andra resurser än processor, minne, data-I/O och transaktionsloggs-I/O finns i [Azure SQL Database-resursgränser](sql-database-resource-limits.md).

## <a name="next-steps"></a>Nästa steg
* Se [tjänstnivån](sql-database-service-tiers.md) information om hello dtu: er och edtu: er som är tillgängliga för enskilda databaser och elastiska pooler.
* Information om gränserna för andra resurser än processor, minne, data-I/O och transaktionsloggs-I/O finns i [Azure SQL Database-resursgränser](sql-database-resource-limits.md).
* Se [SQL Database Query Performance Insight](sql-database-query-performance.md) toounderstand användning (Dtu).
* Se [SQL-databas benchmark-översikt](sql-database-benchmark-overview.md) toounderstand hello metoder bakom benchmark hello OLTP-arbetsbelastning används toodetermine hello DTU blandas.
