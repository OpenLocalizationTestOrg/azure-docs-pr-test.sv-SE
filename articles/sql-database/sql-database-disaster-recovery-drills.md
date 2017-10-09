---
title: aaaSQL databasen Haveriberedskap | Microsoft Docs
description: "Lär dig vägledning och bästa praxis för att använda Azure SQL Database tooperform disaster recovery flyttar toohelp behålla din uppdrag affärskritiska program flexibel toofailures och avbrott."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: bf17857a19fdebddf0d4f55e4db3a1b33efb4e8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performing-disaster-recovery-drill"></a>Utför Återställningsgranskning för katastrofåterställning
Du rekommenderas att verifieringen av programmet beredskap för återställningsarbetsflöde utförs med jämna mellanrum. Verifierar hello programmets beteende och följderna av att data går förlorade och/eller hello avbrott innebär att redundansväxlingen är bra engineering. Det är också ett krav som de flesta branschstandarder som en del av business continuity certifikatutfärdare.

Utför en katastrofåterställning återställningsgranskning består av:

* Simulering data nivå avbrott
* Återställa
* Validera programåterställning integritet post

Beroende på hur du [utformats för ditt program för affärskontinuitet](sql-database-business-continuity.md), hello arbetsflöde tooexecute hello ökad kan variera. Nedan beskrivs hello metodtips utför en katastrofåterställning återställningsgranskning hello gäller Azure SQL Database.

## <a name="geo-restore"></a>Geo-återställning
tooprevent hello potentiell dataförlust när du utför en katastrofåterställning återställningsgranskning, rekommenderar vi hello ökad använder en testmiljö genom att skapa en kopia av hello produktionsmiljön och använder den tooverify hello programmet redundans arbetsflöde.

#### <a name="outage-simulation"></a>Avbrott simulering
toosimulate hello avbrott, kan du ta bort eller byta namn på hello källdatabasen. Detta medför anslutningsfel till programmet.

#### <a name="recovery"></a>Återställning
* Utföra hello geo-återställning av hello-databas till en annan server enligt beskrivningen [här](sql-database-disaster-recovery.md).
* Ändra hello programmet configuration tooconnect toohello återställda databasen och följ hello [konfigurera en databas efter återställningen](sql-database-disaster-recovery.md) guide toocomplete hello återställning.

#### <a name="validation"></a>Validering
* Fullständig hello detaljnivån genom att verifiera hello integritet efter programåterställning (inklusive anslutningssträngar, inloggningar, grundläggande funktioner testning eller andra verifieringar som en del av standardprogrammet signeringar procedurer).

## <a name="geo-replication"></a>Geo-replikering
För en databas som skyddas med geo-replikering hello ökad omfattar Övning planerad redundans toohello sekundär databas. hello säkerställer planerad redundans att hello primära och sekundära databaser i hello är synkroniserade när hello roller växlas. Till skillnad från Hej oplanerad växling vid fel, åtgärden inte leder till dataförlust, så hello ökad kan utföras i hello-produktionsmiljö.

#### <a name="outage-simulation"></a>Avbrott simulering
toosimulate hello avbrott, kan du inaktivera hello webbprogram eller virtuella datorn ansluten toohello databas. Detta resulterar i anslutningsfel till hello för hello webbklienter.

#### <a name="recovery"></a>Återställning
* Gör att hello programkonfigurationen i hello DR region punkter toohello f.d. sekundära som blir hello fullt tillgängliga nya primära.
* Utföra [planerad redundans](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello sekundära databasen en ny primär
* Följ hello [konfigurera en databas efter återställningen](sql-database-disaster-recovery.md) guide toocomplete hello återställning.

#### <a name="validation"></a>Validering
* Fullständig hello detaljnivån genom att verifiera hello integritet efter programåterställning (inklusive anslutningssträngar, inloggningar, grundläggande funktioner testning eller andra verifieringar som en del av standardprogrammet signeringar procedurer).

## <a name="next-steps"></a>Nästa steg
* toolearn om business continuity scenarier finns [kontinuitet scenarier](sql-database-business-continuity.md)
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md)
* toolearn om hur du använder automatisk säkerhetskopiering för återställning finns [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md)
* toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)  
