---
title: aaaAzure SQL-databasen och svar | Microsoft Docs
description: "Svaren toocommon frågor kunder fråga om molnet databaser och Azure SQL Database, Microsofts relationella databashanteringssystem (RDBMS) och databasen som en tjänst i molnet hello."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 02/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 04c02f96e6e91cf314221134ee0ef6d24217ef45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-faq"></a>Vanliga frågor om SQL Database

## <a name="what-is-hello-current-version-of-sql-database"></a>Vad är hello nuvarande version av SQL-databasen?
hello aktuella versionen av SQL-databas är V12. Version V11 har tagits bort.

## <a name="what-is-hello-sla-for-sql-database"></a>Vad är hello SLA för SQL-databas?
Vi garanterar minst 99,99% hello tid kunder ska ha anslutning mellan sina enskild eller elastisk Basic, Standard, eller Premium Microsoft Azure SQL Database och våra Internet-gateway. Mer information finns i [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-hello-password-for-hello-server-admin"></a>Hur jag för att återställa hello lösenord för serveradministratören hello?
I hello [Azure-portalen](https://portal.azure.com) klickar du på **SQL Servers**markerar hello server hello listan och klicka sedan på **Återställ lösenord**.

## <a name="how-do-i-manage-databases-and-logins"></a>Hur hanterar jag databaser och inloggningar?
Se [hantera databaser och inloggningar](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-tooaccess-a-server"></a>Hur kan jag vara säker på att endast auktoriserade IP-adresser tillåts tooaccess en server?
Se [så här: konfigurera brandväggsinställningar på SQL-databas](sql-database-configure-firewall-settings.md).

## <a name="how-does-hello-usage-of-sql-database-show-up-on-my-bill"></a>Hur ska hello användning av SQL-databas på min faktura?
Växlar SQL-databas på en förutsägbar timvis avgift baserat på både hello tjänstnivån + prestanda för enskilda databaser eller per elastisk pool-edtu: er. Faktiska användningen beräknas och proportionerligt varje timma, så fakturan kan visa delar av en timme. Om det finns en databas för 12 timmar i månaden, visar fakturan användning av 0,5 dagar. Servicenivåer + prestandanivå och per pool-edtu: er är att gissa ut i hello faktura toomake den enklare toosee hello antalet databasdagar som du använde för varje i en månad.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Vad händer om en enskild databas är aktiv för mindre än en timme eller använder en högre tjänstnivå för mindre än en timme?
Du debiteras för varje timme finns en databas med hjälp av hello högsta tjänstnivån + prestanda nivå som tillämpas under den timmen, oavsett användning eller om hello databasen var aktiv för mindre än en timme. Om du skapar en databas och ta bort den fem minuter senare visar fakturan en avgift för en databas timme. 

Exempel

* Om du skapar en grundläggande databas och sedan omedelbart uppgradera den tooStandard S1 debiteras du med hello Standard S1 hastighet för hello timma.
* Om du uppgraderar en databas från grundläggande tooPremium vid 10:00 och att uppgraderingen har slutförts klockan 1:35 hello efter dag, som du debiteras med hello Premium hastighet som börjar vid 1:00 a.m. 
* Om du nedgradera en databas från Premium tooBasic klockan 11:00 och den är klar kl. 2:15 sedan hello-databas debiteras med hello betalsamtal fram till 15:00. Därefter debiteras den med grundläggande hello-priser.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hur elastisk pool användning visas på min faktura och vad som händer när jag ändrar edtu: er per pool?
Elastisk pool debiterar visa upp på fakturan som elastiska dtu: er (edtu: er) i hello steg som visas under edtu: er per pool på [hello sida med priser](https://azure.microsoft.com/pricing/details/sql-database/). Det kostar ingenting per databas för elastiska pooler. Du debiteras för varje timme poolen finns på hello högsta eDTU, oavsett användning eller om hello poolen var aktiv för mindre än en timme. 

Exempel

* Om du skapar en Standard elastisk pool med 200 edtu: er på 11:18:00, debiteras lägga till fem databaser toohello poolen, du för 200 edtu: er för hello hela timme, början på 11: 00 via hello resten av hello dag.
* På dag 2 i 05:05:00, databasen 1 börjar förbruka 50 edtu: er och innehåller konstant via hello dag. Databaser 2-5 variera mellan 0 och 80 edtu: er. Lägg till fem andra databaser som använder olika edtu: er i hela hello dag under hello dag. Dag 2 är en hel dag faktureras enligt 200 eDTU. 
* På dag 3, 5 klockan du lägger till en annan 15 databaser. Databasanvändningen ökar i hela hello dag toohello plats väljer du tooincrease edtu: er för hello adresspoolen från 200 too400 kl. 8:05 Avgifter på hello 200 eDTU nivå gällde till 20: 00 och ökar too400 edtu: er för hello återstående fyra timmar. 

## <a name="elastic-pool-billing-and-pricing-information"></a>Elastisk pool fakturerings- och information om priser
Elastiska pooler debiteras per hello följande egenskaper:

* En elastisk pool debiteras när skapades, även om det finns inga databaser i hello pool.
* En elastisk pool faktureras timvis. Detta är hello samma avläsning frekvens som prestandanivåer för enskilda databaser.
* Om en elastisk pool är storleksändrade tooa nya faktureras antalet edtu: er, sedan hello poolen inte enligt toohello nya mängden edtu: er tills hello storleksändring åtgärden har slutförts. Det följer samma mönster som att ändra hello prestandanivå för enskilda databaser hello.
* hello priset på en elastisk pool baseras på hello antalet edtu: er hello. hello priset på en elastisk pool är oberoende av hello tal och användning av hello elastiska databaser i den.
* Priset beräknas som (antal pool-edtu: er) x (pris per eDTU).

Hej eDTU enhetspriset för en elastisk pool är högre än hello DTU enhetspriset för en enskild databas i hello samma tjänstnivån. Mer information finns i [Priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 

toounderstand hello-edtu: er och tjänsten nivåer, se [SQL Database-alternativ och prestanda](sql-database-service-tiers.md).

## <a name="how-does-hello-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Hur använder hello av aktiv geo-replikering i en elastisk pool visas på min faktura?
Till skillnad från enskilda databaser, med hjälp av [aktiv geo-replikering](sql-database-geo-replication-overview.md) med elastiska databaser inte har en direkt inverkan för fakturering.  Endast debiteras du för hello edtu: er etableras för varje hello pooler (primär och sekundär pool)

## <a name="how-does-hello-use-of-hello-auditing-feature-impact-my-bill"></a>Hur hello används av hello granskning funktionen påverkan min faktura?
Granskning är inbyggd i hello SQL Database-tjänsten utan extra kostnad och är tillgängliga tooBasic, Standard, Premium och Premium-RS databaser. Dock granskningsloggar toostore hello, hello granskning funktionen använder ett Azure Storage-konto och priser för tabeller och köer i Azure Storage tillämpas baserat på hello storleken på din granskningslogg.

## <a name="how-do-i-find-hello-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Hur hittar jag hello rätt tjänstnivå och prestandanivå servicenivå för enskilda databaser och elastiska pooler
Det finns några verktyg tillgängliga tooyou. 

* Använd hello för lokala databaser [DTU storlek advisor](http://dtucalculator.azurewebsites.net/) toorecommend hello databaser och dtu: er som krävs och utvärdera flera databaser för elastiska pooler.
* Om en enskild databas lönar sig att den finns i en pool, rekommenderar Azures intelligent motorn en elastisk pool om den ser ett mönster för användningshistorik som på grund av den. Se [övervaka och hantera en elastisk pool med hello Azure-portalen](sql-database-elastic-pool-manage-portal.md). Mer information om hur toodo hello matematiska själv finns [pris- och prestandaöverväganden för en elastisk pool](sql-database-elastic-pool.md)
* toosee om du behöver toodial en enskild databas uppåt eller nedåt i [prestandaråd för enskilda databaser](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-hello-service-tier-or-performance-level-of-a-single-database"></a>Hur ofta kan jag ändra hello nivå eller prestanda tjänstenivå för en enskild databas?
Du kan ändra hello tjänstnivån (mellan Basic, Standard, Premium och Premium-RS) eller hello prestandanivån i en tjänstnivå (till exempel S1 tooS2) så ofta du vill. Du kan ändra totalt fyra gånger under en 24-timmarsperiod hello nivå eller prestanda servicenivå för den tidigare versionen databaser.

## <a name="how-often-can-i-adjust-hello-edtus-per-pool"></a>Hur ofta kan jag justera hello edtu: er per pool?
Så ofta du vill.

## <a name="how-long-does-it-take-toochange-hello-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Hur länge det ta toochange hello nivå eller prestanda tjänstenivå för en enskild databas eller flytta en databas till och från en elastisk pool?
Ändra hello tjänstnivån för en databas och flytta in och ut av en pool kräver hello databasen toobe kopieras på hello plattform som en bakgrundsåtgärd. Ändra hello tjänstnivån kan ta några minuter tooseveral timmar beroende på hello storlek hello databaser. I båda fallen vara hello databaser online och tillgänglig under hello move. Mer information om hur du ändrar enskilda databaser finns [ändra hello tjänstnivån för en databas](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>När bör jag använda en enskild databas jämfört med elastiska databaser?
I allmänhet elastiska pooler är utformade för att en typisk [programmönster för programvara som en tjänst (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md), där det finns en databas per kund eller klient. Köp enskilda databaser och överetablering toomeet hello variabeln och belastning begära för varje databas kostar ofta inte effektivt. Du hanterar hello samlade prestandan för hello-pool med pooler, och hello databaser skala upp eller ned automatiskt. Azures intelligent motorn rekommenderar en pool för databaser när en användningsmönstret på grund av den. Mer information finns i [elastisk pool vägledning](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-toohave-up-too200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Vad innebär det toohave too200% av lagringsenheterna maximala etablerade databas för lagring av säkerhetskopior?
Lagring för säkerhetskopiering är hello lagring som är associerade med dina automatiserade databassäkerhetskopieringar som används för [punkt-i-tid-återställning](sql-database-recovery-using-backups.md#point-in-time-restore) och [geo-återställning](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Database ger dig too200% av dina högsta etablerade databaslagring för säkerhetskopieringslagring utan extra kostnad. Till exempel om du har en Standard DB-instans med en etablerade DB storlek på 250 GB finns med 500 GB lagring för säkerhetskopiering utan extra kostnad. Om din databas överskrider hello tillhandahålls lagring för säkerhetskopiering, kan du välja tooreduce hello kvarhållningsperiod genom att kontakta Azure-supporten eller betala för hello extra lagring av säkerhetskopior faktureras enligt standardkostnaden geografiskt Redundant lagring med läsbehörighet (RA-GRS). Mer information om fakturering för RA-GRS finns prisinformation för lagring.

## <a name="im-moving-from-webbusiness-toohello-new-service-tiers-what-do-i-need-tooknow"></a>Jag flyttas från Web/företag toohello nya servicenivåer, vad gör jag behöver tooknow?
Azure SQL-webb- och affärsdatabaser är nu inaktiverad. hello Basic, Standard, Premium, Premium RS och elastiska nivåer Ersätt hello tas ur bruk webb- och affärsdatabaser. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-hello-same-azure-geography"></a>Vad är en förväntad replikeringsfördröjning när geo-replikering av en databas mellan två regioner inom hello samma Azure geografisk plats?
Vi stöder för närvarande ett Återställningpunktsmål fem sekunder och hello replikeringsfördröjning har mindre än den när hello geo sekundära finns i hello Azure rekommenderas parad region och på hello tjänsten samma nivå.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-hello-same-region-as-hello-primary-database"></a>Vad är en förväntad replikeringsfördröjning när geo sekundär skapas i hello samma region som hello primära databasen?
Baserat på uteslutande data, finns det inte för mycket skillnaden mellan intra-region och mellan region replikeringsfördröjning när hello Azure rekommenderar region parad används. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-hello-retry-logic-work-when-geo-replication-is-set-up"></a>Om det finns ett fel i nätverket mellan två regioner, hur hello logik fungerar när geo-replikering konfigureras?
Om det finns ett kopplas från, vi igen var 10 sekunder toore-upprätta anslutningar.

## <a name="what-can-i-do-tooguarantee-that-a-critical-change-on-hello-primary-database-is-replicated"></a>Vad kan jag göra tooguarantee att viktiga förändringar i hello primära databasen replikeras?
hello geo sekundära är en asynkron replikering och vi inte försöka tookeep i fullständig synkronisering med hello primära. Men vi ger en metod tooforce tooensure hello synkronisering av viktiga ändringar (till exempel lösenordsuppdateringar). Framtvingad synkronisering påverkar prestanda eftersom hello anropande tråden blockeras tills alla genomförda transaktioner replikeras. Mer information finns i [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-toomonitor-hello-replication-lag-between-hello-primary-database-and-geo-secondary"></a>Vilka verktyg är tillgängliga toomonitor hello replikeringsfördröjning mellan hello primära databasen och geo sekundära?
Vi exponera hello realtid replikeringsfördröjning mellan hello primära databasen och geo sekundära via en DMV. Mer information finns i [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="toomove-a-database-tooa-different-server-in-hello-same-subscription"></a>toomove en tooa olika databasserver i hello samma prenumeration
* I hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-databaser**, Välj en databas hello listan och klicka sedan på **kopiera**. Se [kopiera en Azure SQL database](sql-database-copy.md) för mer information.

## <a name="toomove-a-database-between-subscriptions"></a>toomove en databas mellan prenumerationer
* I hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-servrar** och välj sedan hello-server som är värd för databasen hello-listan. Klicka på **flytta**, och välj sedan hello resurser toomove och hello prenumeration toomove till.
