---
title: "aaaSQL (PaaS) Database kontra SQL Server i hello molnet på virtuella datorer (IaaS) | Microsoft Docs"
description: "Lär dig vilka molnet SQLServer-alternativ som passar ditt program: Azure SQL (PaaS) Database eller SQL Server i hello molnet på Azure Virtual Machines."
services: sql-database, virtual-machines
keywords: SQL Server-moln, SQL Server i hello moln, PaaS databasen molnet SQL Server, DBaaS
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cjgronlund
ms.assetid: 7467f422-b77d-4b60-9cb5-0f1ec17ec565
ms.service: sql-database
ms.custom: DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: carlrab
ms.openlocfilehash: 1b462a9a822d04dc5deb8422ed505a5d09279253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Välj ett molnbaserat SQL Server-alternativ: Azure SQL (PaaS) Database eller SQL Server på Azure Virtual Machines (IaaS)
Azure har två alternativ för att hantera SQL Server-arbetsbelastningar i Microsoft Azure:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): en SQL-databas inbyggd toohello molnet, kallas även för en plattform som en tjänst (PaaS)-databas eller en databas som en tjänst (DBaaS) som är optimerad för apputveckling för programvara som en tjänst (SaaS). Den är kompatibel med de flesta funktioner i SQL Server. Mer information om PaaS finns i [Vad är PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
* [SQL Server på Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server är installerad och ligger i hello molnet på virtuella Windows Server-datorer (VM) som körs på Azure, även kallat en infrastruktur som en tjänst (IaaS).
  SQL Server på virtuella datorer i Azure är optimerat för migrering av befintliga SQL Server-program. Alla hello versioner och utgåvor av SQL Server är tillgängliga. Den erbjuder 100% kompatibilitet med SQL Server, vilket gör att du toohost så många databaser som behövs och köra flera databaser transaktioner. Du har fullständig kontroll i SQL Server och Windows.

Lär dig hur varje alternativ passar in i hello Microsofts dataplattform och få hjälp med matchande hello rätt alternativ tooyour affärsbehov. Om du prioriterar kostnadsbesparingar eller minimering av administration över allt annat kan i den här artikeln hjälpa dig att avgöra vilket arbetssätt som levererar mot hello affärskrav som du är mest intresserad.

## <a name="microsofts-data-platform"></a>Microsofts dataplattform
En av hello första saker toounderstand när det gäller Azure kontra lokala SQL Server-databaser är att du kan använda allt. Microsofts dataplattform utnyttjar SQL Server-teknologi och gör den tillgänglig på fysiska, lokala datorer, i privata molnmiljöer, tredjeparts privata molnmiljöer och det offentliga molnet. SQL Server på virtuella Azure-datorer kan du toomeet unika och skilda verksamhetsbehov genom en kombination av lokala och molnbelägna distributioner medan hello med samma uppsättning serverprodukter, utvecklingsverktyg och expertis i dessa miljöer.

   ![Molnet alternativ för SQL Server: SQLServer på IaaS, eller SaaS-SQL-databas i hello molnet.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Som visas i diagram hello kan särskiljs kännetecknas genom hello andelen administration som du har över hello infrastrukturen (på hello X-axeln) och hello graden av kostnadseffektivitet uppnås av konsolidering på databasnivå och automatisering (på hello Y-axeln).

När du skapar ett program, är fyra grundläggande alternativ tillgängliga för värd hello SQL serverdelen av programmet hello:

* SQL Server på icke-virtualiserade fysiska datorer
* SQL Server på lokala virtualiserade datorer (privat moln)
* SQL Server på virtuella Azure-datorer (offentligt Microsoft-moln)
* Azure SQL Database (offentligt Microsoft-moln)

I följande avsnitt hello, du lär dig mer om SQL Server i hello Microsoft offentliga molnet: Azure SQL Database och SQL Server på virtuella Azure-datorer. Dessutom går vi igenom vanliga verksamhetsmotiv för att fastställa vilket alternativ som är bäst för ditt fall.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>En närmare titt på Azure SQL Database och SQL Server på virtuella Azure-datorer
**Azure SQL Database** är en relationell databas som en-tjänst (DBaaS) finns i hello Azure-molnet hamnar i hello branschen kategorier av *programvara som en-tjänst (SaaS)* och *plattform som en tjänst (PaaS)* . [SQL Database](sql-database-technical-overview.md) bygger på standardiserad maskinvara och programvara som ägs, finns hos och hanteras av Microsoft. Med SQL-databas kan du utveckla direkt på hello-tjänsten med hjälp av inbyggda funktioner. När du använder SQL Database, betalar med alternativ tooscale upp eller ut för mer kraft utan avbrott du.

**SQL Server på Azure Virtual Machines (virtuella datorer)** hamnar i hello branschkategori *Infrastructure-as-a-Service (IaaS)* och låter dig toorun SQL Server på en virtuell dator i hello molnet. Liknande tooSQL databasen är den byggd på standardiserad maskinvara som ägs, finns, och hanteras av Microsoft. När du använder SQL Server på en virtuell dator kan du antingen betala per användning för en SQL Server-licens som redan finns i en SQL Server-avbildning eller enkelt använda en befintlig licens. Du kan också enkelt skala upp/ned och pausa/hello VM efter behov.

Generellt sett är de här två SQL-alternativen optimerade för olika syften:

* **Azure SQL Database** är optimerad tooreduce övergripande kostnader toohello minsta för etablering och hantering av många databaser. Det minskar löpande administrationskostnader eftersom du inte har toomanage alla virtuella datorer, operativsystem eller databasprogram. Du har inte toomanage uppgraderingar, hög tillgänglighet, eller [säkerhetskopieringar](sql-database-automated-backups.md). I allmänhet Azure SQL Database kraftigt öka hello antalet databaser som hanteras av en enskild IT eller utvecklingsresurs.
* **SQL Server som körs på Azure Virtual Machines** är optimerad för att migrera befintliga program tooAzure eller utöka befintliga lokala program toohello moln i hybriddistribution. Du kan dessutom använda SQL Server i en virtuell dator toodevelop och testa traditionella SQL Server-program. Med SQL Server på Azure Virtual Machines har du hello fullständig administrativ behörighet via en dedikerad SQL Server-instans och en molnbaserad VM. Det är det perfekta valet när en organisation redan har IT-resurser tillgängliga toomaintain hello virtuella datorer. Dessa funktioner kan du toobuild ett höganpassat system tooaddress ditt programs specifika krav på prestanda och tillgänglighet.

hello följande tabell sammanfattas hello huvudegenskaper SQL Database och SQL Server på virtuella Azure-datorer:

| **Bäst för:** | **Azure SQL Database** | **SQL Server på en virtuell Azure-dator** |
| --- | --- | --- |
|  |Nya molnbaserade program med tidsbegränsningar på utveckling och marknadsföring. |Befintliga program som kräver snabb migrering toohello molnet med minimala ändringar. Snabb utveckling och testning scenarier när du inte vill toobuy lokal icke-produktion SQL Server-maskinvara. |
|  | Team som behöver inbyggd hög tillgänglighet och katastrofåterställning uppgradering för hello-databasen. |Team som kan konfigurera och hantera hög tillgänglighet, haveriberedskap och korrigeringar för SQL Server. Vissa automatiserade funktioner kan avsevärt förenkla detta arbete. | |
|  | Team som inte vill toomanage hello underliggande operativsystemen och konfigurationsinställningarna. |Du behöver en anpassad miljö med fullständiga administrativa rättigheter. | |
|  | Databaser för in too4 TB eller större databaser som kan vara [vågrätt eller lodrätt partitionerad](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) ett skalbara mönster. |SQL Server-instanser med upp too64 TB lagringsutrymme. hello-instans kan stödja valfritt antal databaser efter behov. | |
|  | [Skapa programvara som en tjänst (SaaS)-program](sql-database-design-patterns-multi-tenancy-saas-applications.md). |Migrera och utveckla företags- och hybridprogram. | |
|  | | |
| **Resurser:** |Du inte vill tooemploy IT-resurser för konfiguration och hantering av hello underliggande infrastruktur, men vill toofocus på programnivå hello. |Du har några IT-resurser som ansvarar för konfiguration och hantering. Vissa automatiserade funktioner kan avsevärt förenkla detta arbete. |
| **Totalkostnad för ägarskap:** |Eliminerar kostnaderna för maskinvara och reducerar de administrativa kostnaderna. |Eliminerar maskinvarukostnaderna. |
| **Verksamhetskontinuitet:** |I tillägg toobuilt i infrastrukturen feltolerans, Azure SQL Database ger funktioner, t.ex [automatisk säkerhetskopiering](sql-database-automated-backups.md), [Point-In-Time-återställning](sql-database-recovery-using-backups.md#point-in-time-restore), [geo-återställning](sql-database-recovery-using-backups.md#geo-restore), och [aktiv geo-replikering](sql-database-geo-replication-overview.md) tooincrease kontinuitet för företag. Mer information finns i [översikt över verksamhetskontinuitet i SQL Database](sql-database-business-continuity.md). |Med SQL Server på Azure Virtual Machines kan du konfigurera en lösning med hög tillgänglighet och haveriberedskap för din databas specifika behov. Du kan därmed få ett system som är höggradigt optimerat för just ditt program. Du kan själv testa och köra felväxling vid behov. Mer information finns i [Hög tillgänglighet och haveriberedskap för SQL Server på Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybridmoln:** |Ditt lokala program har åtkomst till data i Azure SQL Database. |Med SQL Server på Azure Virtual Machines, du kan ha program som kör delvis i hello molnet och delvis lokalt. Du kan till exempel utöka ditt lokala nätverk och Active Directory-domän toohello molnet via [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Dessutom kan du lagra lokala datafiler i Azure Storage med [SQL Server-datafiler i Azure](http://msdn.microsoft.com/library/dn385720.aspx). Mer information finns i [introduktion tooSQL Server 2014 Hybridmoln](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Stöder [SQL Server-Transaktionsreplikering](https://msdn.microsoft.com/library/mt589530.aspx) som en prenumerant tooreplicate data. |Fullständigt stöd för [SQL Server-Transaktionsreplikering](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn Availability Groups](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services och Loggöverföring tooreplicate data. Dessutom finns fullständigt stöd för traditionella SQL Server-säkerhetskopieringar | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Verksamhetsmotivering för att välja Azure SQL Database eller SQL Server på virtuella Azure-datorer
### <a name="cost"></a>Kostnad
Om du är en startup med dålig för kontanter eller ett team i ett etablerat företag som agerar under begränsad budget begränsningar, begränsad budget är ofta hello primära drivrutinen när du bestämmer hur toohost dina databaser. I det här avsnittet får du lära dig om hello fakturering och licensiering i Azure med gäller toothese två relationella databasalternativen: SQL Database och SQL Server på virtuella Azure-datorer. Här beskrivs också hur beräkna hello totala kostnaden.

#### <a name="billing-and-licensing-basics"></a>Debitering och licensiering
**SQL-databas** säljs toocustomers som en tjänst, inte med en licens.  [SQL Server på Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) säljs med en medföljande licens som du betalar per minut. Om du har en befintlig licens kan du också använda den.  

För närvarande **SQL-databas** är tillgänglig i flera servicenivåer, där alla debiteras timvis till en fast kostnad som baseras på hello tjänstnivå och prestandanivå servicenivå du väljer. Dessutom debiteras du för utgående Internettrafik till normal [dataöverföringskostnad](https://azure.microsoft.com/pricing/details/data-transfers/). Hej Basic, Standard, Premium och Premium-RS nivåer har utformats för toodeliver förutsägbar prestanda med flera prestanda nivåer toomatch ditt programs högsta krav. Du kan ändra mellan tjänstnivåer och prestanda nivåer toomatch programmets olika genomströmning måste. Om databasen har hög transaktionella volym och behov toosupport många samtidiga användare, rekommenderar vi premiumnivån hello. Hello senaste informationen om servicenivåer för hello aktuella stöds, se [Azure SQL Database servicenivåer](sql-database-service-tiers.md). Du kan också skapa [elastiska pooler](sql-database-elastic-pool.md) tooshare prestanda resurser mellan databasinstanser.

Med **SQL-databas**, hello-databas som automatiskt konfigureras, korrigeras och uppgraderas av Microsoft, vilket minskar kostnaderna för administration. Dessutom gör dess [inbyggda säkerhetskopierings](sql-database-automated-backups.md)-funktioner att du kan uppnå markanta kostnadsbesparingar, speciellt om du har ett stort antal databaser.

Med **SQL Server på Azure Virtual Machines**, du kan använda någon av hello plattformen erbjuder SQL Server-avbildningar (som inkluderar en licens) eller använda din SQL Server-licens. Alla hello SQL Server-versioner som stöds (2008R2, 2012, 2014, 2016) och utgåvor (utvecklare, snabb, webb, Standard, Enterprise) är tillgängliga. Dessutom finns versioner Bring-Your-äger-licens (BYOL) av hello avbildningar. När du använder hello Azure tillhandahålls bilder, beror driftskostnaderna för hello på hello VM-storlek och hello utgåva av SQL Server som du väljer. Oavsett VM-storlek eller SQL Server-utgåva betalar du per minut licensiering kostnaden för SQL Server och Windows Server, tillsammans med hello Azure Storage kostnaden för hello Virtuella diskar. hello per minut betalningsalternativ kan toouse SQL Server så länge du behöver utan att köpa ytterligare SQL Server-licenser. Om du använder en egen SQL Server-licens tooAzure debiteras du för Windows Server och lagringskostnader endast. Mer information om att använda sin egen licensiering finns i [Licensera Mobility via Software Assurance på Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-hello-total-application-cost"></a>Beräkning av hello totala programkostnaden
När du börjar använda en molnplattform, inkluderar hello kostnaden för att köra programmet hello utveckling och administrationskostnader samt hello offentligt moln plattform kostnad.

Här är hello detaljerad kostnadskalkyl för ditt program som körs i SQL Database och SQL Server på virtuella Azure-datorer:

**Med Azure SQL Database:**

*Totalkostnad för programmet = kraftigt minimerade administrationskostnader + kostnader för programvaruutveckling + SQL Database-tjänstekostnader*

**När du använder SQL Server på virtuella Azure-datorer:**

*Totalkostnaden för programmet = mycket mindre programvaruutvecklingskostnad + administrationskostnader + licensieringskostnader för SQL Server och Windows Server + kostnader för Azure Storage*

Mer information om priser finns i hello följande resurser:

* [Priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
* [Priser för virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/) för [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) och för [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Priskalkylator för Azure](https://azure.microsoft.com/pricing/calculator/)

> [!NOTE]
> Det finns vissa funktioner på SQL Server som inte är tillämpliga eller tillgängliga med SQL Databaser. Mer information finns i [SQL Database Features](sql-database-features.md) (SQL Database-funktioner) och [SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Information om SQL Database Transact-SQL). Om du flyttar en befintlig SQL Server-lösning toohello molnet, se [migrera en SQL-databas med SQL Server-databasen tooAzure](sql-database-cloud-migrate.md). När du migrerar en befintlig lokal SQL Server-program tooSQL databasen bör du även uppdatera hello programmet tootake nytta av hello-funktionerna som molntjänster erbjuder. Exempelvis kan du använda [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) eller [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) toohost ditt program layer tooincrease kostnad fördelar.
> 
> 

### <a name="administration"></a>Administration
För många företag är hello beslut tootransition tooa molntjänst som är mycket om att minska administrativ komplexitet som kostnad. Med **SQL-databas**, Microsoft administrerar hello underliggande maskinvara. Microsoft automatiskt replikerar alla data tooprovide hög tillgänglighet, konfigurerar och uppgraderar hello databasprogram, hanterar belastningsutjämning och genomför transparent växling om det finns ett serverfel. Du kan fortsätta tooadminister databasen, men du behöver inte längre toomanage hello databasmotorn, server-operativsystem eller maskinvara.  Exempel på saker som du kan fortsätta tooadminister inkluderar databaser och inloggningar, index och frågejusteringar och granskning och säkerhetsinställningar.

Med **SQL Server på Azure Virtual Machines**, har du fullständig kontroll över hello operativsystemet och konfiguration av SQL Server-instansen. Med en VM, är det upp tooyou toodecide när tooupdate eller uppgradering hello fungerar system- och programvara och när tooinstall ytterligare programvara, till exempel antivirusprogram. Vissa funktioner för automatisk tillhandahålls toodramatically förenkla uppdatering, säkerhetskopiering och hög tillgänglighet. Du kan dessutom styra hello storleken på hello VM, hello antalet diskar och deras lagringskonfigurationer. Azure kan du toochange hello storleken på en virtuell dator efter behov. Mer information finns i [Storlekar för virtuella Azure-datorer och Azure-molntjänster](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Serviceavtal (SLA)
För många IT-avdelningar är det av högsta vikt att uppfylla skyldigheterna på upptid enligt serviceavtalet (SLA). I det här avsnittet ska vi titta på SLA gäller tooeach databasalternativ.

För **SQL Database**-servicenivåerna Basic, Standard, Premium och Premium RS erbjuder Microsoft en SLA med 99,99 % tillgänglighet. Hello senaste information finns i [servicenivåavtalet](https://azure.microsoft.com/support/legal/sla/sql-database/). Hello senaste informationen om servicenivåer för SQL-databas och planer för verksamhetskontinuitet hello stöds, se [tjänstnivåer](sql-database-service-tiers.md).

För **SQL Server som körs på Azure Virtual Machines**, erbjuder Microsoft en tillgänglighets-SLA på 99,95% att omfattar bara hello virtuell dator. Detta serviceavtal omfattar inte hello processer (till exempel SQL Server) som körs på hello VM och kräver att du vara värd för minst två VM-instanser i en tillgänglighetsuppsättning. Hello senaste informationen finns hello [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). För databasen med hög tillgänglighet (HA) i virtuella datorer bör du konfigurera en av alternativ för hög tillgänglighet för hello stöds i SQL Server, som [AlwaysOn Availability Groups](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Med alternativet för hög tillgänglighet som stöds inte ger ett ytterligare SLA, men låter dig tooachieve > 99,99% tillgänglighet för databasen.

### <a name="market"></a>Tid toomarket
**SQL-databas** är hello rätta lösningen för molnutformade program när utvecklarproduktivitet och snabb tid till marknad är avgörande. Det är perfekt för molnarkitekter och utveckare med programmässiga DBA-lika funktionalitet, eftersom det sänker hello behovet av att hantera hello underliggande operativsystemet och databasen. Du kan till exempel använda hello [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) och [PowerShell-Cmdlets](http://msdn.microsoft.com/library/mt740629.aspx) tooautomate och hantera administrativa åtgärder för tusentals databaser. Funktioner som [elastiska pooler](sql-database-elastic-pool.md) Tillåt toofocus på programnivå hello och leverera din lösning toohello marknaden snabbare.

**SQL Server som körs på Azure Virtual Machines** är perfekt om dina befintliga eller nya program kräver stora databaser, relaterade databaser eller komma åt tooall funktioner i SQL Server eller Windows Server. Det är också passa om du vill toomigrate befintliga lokala program och databaser tooAzure som-är. Eftersom du inte behöver toochange hello presentationen, programmet och datalager, spara tid och budget på att omstrukturera din befintliga lösning. I stället kan du fokusera på att migrera alla lösningar tooAzure och genomföra prestandaoptimeringar som kan krävas för hello Azure-plattformen. Mer information finns i [Bästa praxis för prestanda i SQL Server på Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Sammanfattning
Den här artikeln har gått igenom SQL Database och SQL Server på virtuella Azure-datorer (VM:ar) och pratat om några vanliga verksamhetsmotivationer som kan påverka ditt beslut. hello nedan följer en sammanfattning av förslag som du tooconsider:

Välj **Azure SQL Database** om:

* Du kan skapa nya molnbaserade program tootake nytta av hello kostnaden besparingar och prestandaoptimering som molntjänster tillhandahåller. Den här metoden ger hello fördelarna med en helt hanterad molntjänst, hjälper lägre inledande tid till marknaden och kan ge långsiktiga kostnadsoptimering.
* Du vill toohave Microsoft hantera vanliga hanteringsåtgärder på databaserna och kräver bättre tillgänglighets-SLA för databaser.

Välj **SQL Server på virtuella Azure-datorer** om:

* Du har befintliga lokala program som du vill toomigrate eller utöka toohello moln, eller om du vill att toobuild företagsprogram är större än 4 TB. Den här metoden erbjuder hello fördelen med 100% SQL-kompatibilitet stora databaskapacitet fullständig kontroll över SQL Server- och Windows- och säkra tunnlar tooon lokala. Lösningen minimerar kostnaderna för utveckling och ändringar av befintliga program.
* Du har befintliga IT-resurser och kan själv hantera korrigeringar, säkerhetskopieringar och hög databastillgänglighet. Observera att vissa automatiska funktioner avsevärt förenklar dessa åtgärder. 

## <a name="next-steps"></a>Nästa steg
* Se [ditt första Azure SQL Database](sql-database-get-started-portal.md) tooget igång med SQL-databas.
* Se [Priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Se [etablera en virtuell dator med SQL Server i Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) tooget igång med SQL Server på virtuella Azure-datorer.

