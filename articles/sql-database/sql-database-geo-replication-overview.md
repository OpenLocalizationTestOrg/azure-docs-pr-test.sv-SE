---
title: aaaFailover grupper och aktiv geo-replikering - Azure SQL Database | Microsoft Docs
description: "Automatisk redundans grupper med aktiv geo-replikering kan du toosetup 4 repliker av din databas i någon av hello Azure-datacenter och autoomatically redundans i ett avbrott hello-händelse."
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 07/10/2017
ms.author: sashan
ms.openlocfilehash: 7a39af8505624c0f19c5abccff051af836b1f9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Översikt: Redundans grupper och aktiv geo-replikering
Aktiv geo-replikering kan du tooconfigure in toofour läsbara sekundära databaser i hello samma eller olika Datacenter platser (regioner). Sekundära databaser är tillgängliga för frågor och för redundans hello gäller en data center nätverksavbrott eller hello oförmåga tooconnect toohello primära databasen. hello växling vid fel måste initieras manuellt av hello tillämpningen av hello användare. Efter växling vid fel har hello nya primära en annan anslutning slutpunkt. 

> [!NOTE]
> Aktiv geo-replikering är tillgänglig för alla databaser i alla servicenivåer i alla regioner.
>  

Azure SQL Database automatisk redundans grupper (i förhandsversion) är en SQL-databas funktionen utformats tooautomatically hanterar geo-replikering relation-, anslutnings- och växling vid fel i större skala. Återskapa hello kunder vinst hello möjlighet tooautomatically flera relaterade databaser i hello sekundär region efter oåterkalleligt regionala fel eller andra oplanerade händelser som ger fullständig eller partiell förlust av hello SQL Database-tjänsten tillgänglighet i hello primär region. Dessutom kan de använda hello läsbara sekundära databaser toooffload skrivskyddade arbetsbelastningar.  Eftersom automatisk redundans grupper inkluderar flera databaser måste de konfigureras på hello primära servern. Både primära och sekundära servrar måste finnas i hello samma prenumeration. Automatisk redundans grupper stöd för replikering av alla databaser i hello grupp tooonly en sekundär server i en annan region. Aktiv geo-replikering, utan automatisk redundans grupper kan in too4 sekundärservrar i en region.

Om du använder aktiv geo-replikering och för den primära databasen misslyckas orsak eller helt enkelt behov toobe tas offline, kan du initiera redundans tooany sekundära databaserna. När redundansväxlingen är aktiverad tooone hello sekundära databaser kan alla sekundära är automatiskt länkade toohello nya primära. Om du använder automatisk redundans grupper (i förhandsversion) toomanage databasåterställning och eventuella avbrott som påverkar en eller flera av hello databaser i hello gruppen ger automatisk redundans. Du kan konfigurera hello automatisk redundans principen som bäst uppfyller behoven för ditt program eller du kan välja och använda manuell aktivering. Dessutom automatisk redundans grupper (i förhandsversion) ger Läs-och skrivbehörighet och skrivskyddad lyssnare slutpunkter som förblir oförändrade under redundans. Om du använder manuell eller automatisk redundans aktivering växlar redundans alla sekundära databaser i hello grupp tooprimary. När hello databasfel har slutförts är hello DNS-posten uppdateras automatiskt tooredirect hello slutpunkter toohello nya region.

Du kan hantera replikering och redundans för en individuell databas eller en uppsättning databaser på en server eller i en elastisk pool använder aktiv geo-replikering. Du kan göra det med hello [Azure-portalen](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md), eller hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). Kontrollera hello autentiseringskraven för servern och databasen har konfigurerats på hello nya primära efter redundans. Mer information finns i [SQL Database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md). Detta gäller både tooactive geo-replikering och automatisk redundans grupper (i förhandsversion).

Aktiv geo-replikering använder hello [alltid på](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) teknik för SQL Server tooasynchronously replikerar genomförda transaktioner på hello primära tooa sekundära databasen med ögonblicksbild allokerad för läsning isolering (RCSI). Automatisk redundans grupper ger hello grupp semantik ovanpå aktiv geo-replikering men hello samma asynkron replikering används. När vid en viss, hello sekundära databasen kan vara något bakom hello primära databasen, hello sekundära data garanteras toonever har delvis transaktioner. Cross-region redundans gör det möjligt för program tooquickly Återställ en förlust av ett helt datacenter eller delar av ett datacenter som orsakats av naturkatastrof, oåterkalleligt mänskliga fel eller skadliga åtgärder. hello specifika Återställningspunktmål data finns på [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).

hello visar följande bild ett exempel på en aktiv geo-replikering konfigureras med en primär region för hello norra centrala USA och sekundära i hello södra centrala USA region.

![GEO-replikering relation](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

De kan vara används toooffload skrivskyddade arbetsbelastningar som rapportering jobb eftersom hello sekundära databaser är läsbar. Om du använder aktiv geo-replikering, det är möjligt toocreate hello sekundär databas i hello samma region med hello primära, men inte öka hello programmet återhämtning toocatastrophic fel. Om du använder automatisk redundans grupper (i förhandsversion) skapas alltid din sekundära databas i en annan region.

Dessutom kan toodisaster recovery aktiv geo-replikering användas i hello följande scenarier:

* **Databasen migrering**: du kan använda aktiv geo-replikering toomigrate en databas från en server tooanother online med minimal avbrottstid.
* **Programuppgraderingar**: du kan skapa en extra sekundär som en bakre kopia misslyckas under programuppgraderingar.

tooachieve verkliga affärskontinuitet, lägger till databasredundans mellan Datacenter är bara en del av hello lösning. Återställa ett program (tjänsten) slutpunkt till slutpunkt efter återställning av alla komponenter som utgör hello-tjänsten och alla beroende tjänster kräver att ett oåterkalleligt fel. Exempel på dessa komponenter är hello-klientprogrammet (till exempel en webbläsare med en anpassad JavaScript), frontwebbservrar, lagring och DNS. Det är viktigt att alla komponenter är flexibel toohello samma fel och blir tillgängliga inom hello recovery tid mål för Återställningstid för programmet. Därför behöver tooidentify alla beroende tjänster och förstå hello garantier och funktioner som de tillhandahåller. Sedan måste du vidta lämpliga åtgärder tooensure som din servicefunktioner under hello redundans hello-tjänster som den beroende. Mer information om hur man designar lösningar för katastrofåterställning finns [designa Molnlösningar för Disaster Recovery använder aktiv geo-replikering](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Funktioner för aktiv geo-replikering
hello aktiv geo-replikering funktionen ger hello följande grundläggande funktioner:
* **Automatisk asynkron replikering**: du kan bara skapa en sekundär databas genom att lägga till tooan befintlig databas. hello sekundära kan skapas i en Azure SQL Database-server. När du skapat fylls hello sekundär databas med hello data som kopieras från hello primära databasen. Den här processen kallas seeding. När sekundära databas har skapats och angetts, replikeras uppdateringar toohello primära databasen är asynkront toohello sekundära databasen automatiskt. Asynkron replikering innebär att transaktionerna sparas på hello primära databasen innan de replikerade toohello sekundär databas. 
* **Läsbara sekundära databaser**: ett program kan komma åt en sekundär databas för skrivskyddade åtgärder med hjälp av hello samma eller olika säkerhetsobjekt som används för att komma åt hello primära databasen. hello sekundära databaser fungerar i ögonblicksbildisolering läge tooensure replikering av hello uppdateringar av hello primära (loggen replay) inte skjutas upp med frågor som körs på hello sekundär.

   > [!NOTE]
   > hello loggen replay är försenad på hello sekundära databasen om det finns schemat uppdateras den tar emot från hello primära som kräver ett schemalås på hello sekundär databas. 
   > 

* **Flera läsbara sekundärservrar**: två eller flera sekundära databaser öka redundans och nivå av skydd för hello primära databasen och program. Om det finns flera sekundära databaser, förblir hello program skyddat även om en av hello sekundära databaser misslyckas. Om det finns endast en sekundär databas och misslyckas, hello programmet exponeras toohigher risk förrän en ny sekundär databas har skapats.

   > [!NOTE]
   > Om du använder aktiv geo-replikering toobuild ett globalt distribuerat program och behöver tooprovide skrivskyddad åtkomst till toodata i mer än 4 regioner, kan du skapa sekundär för en sekundär (en process som kallas länkning). Det här sättet kan du uppnå praktiskt taget obegränsade skalan för databasreplikering. Dessutom minskar länkning hello arbetet med att replikeringen från hello primära databasen. hello kompromiss är hello ökad replikeringsfördröjning på hello löv mest sekundära databaser. . 
   >

* **Stöd för elastiska poolen databaser**: aktiv geo-replikering kan konfigureras för alla databaser i en elastisk pool. hello sekundära databasen kan vara i en annan elastisk pool. För vanliga databaser kan hello sekundära en elastisk pool och vice versa så länge hello tjänstnivåer är hello samma. 
* **Konfigurerbara prestandanivå hello sekundära databasen**: en sekundär databas kan skapas med lägre prestandanivå än hello primär. Både primära och sekundära databaser krävs toohave hello samma tjänstnivån. Det här alternativet rekommenderas inte för program med hög Databasaktivitet för skrivning eftersom hello ökad replikeringsfördröjning ökar hello risk för betydande dataförlust efter en växling vid fel. Dessutom när redundans hello programmets prestanda påverkas förrän hello nya primära är uppgraderade tooa högre prestanda nivå. hello logg-i/o procentandel diagram på Azure-portalen innehåller ett bra sätt tooestimate hello minimal prestandanivå hello sekundära som är nödvändiga toosustain hello replikering belastningen. Till exempel om den primära databasen är P6 (1000 DTU) och dess loggen IO-procent är 50% hello sekundära behöver toobe minst P4 (500 DTU). Du kan också hämta hello loggen IO data med hjälp av [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) eller [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) databasen vyer.  Mer information om hello prestandanivåer för SQL-databas finns [SQL Database-alternativ och prestanda](sql-database-service-tiers.md). 
* **Användaren styrs redundans och återställning efter fel**: en sekundär databas explicit kan vara avstängda toohello primära rollen när som helst av programmet hello eller hello användare. Under en verklig avbrott hello ska ”oplanerad” alternativet användas som främjar omedelbart en sekundär toobe hello primär. När hello misslyckade primära återställer och är tillgänglig igen, hello system märken hello återställas primära som en sekundär och automatiskt ta den uppdaterade med hello nya primära. På grund av toohello asynkron uppbyggnad replikering, kan en liten mängd data gå förlorade under oplanerad redundans om en primär misslyckas innan de replikeras hello senaste ändringar toohello sekundär. När en primärnyckel med flera sekundärservrar växlar hello system omkonfigureras automatiskt hello replikeringens relationer och länkar hello återstående sekundärservrar toohello nyligen upphöja primära utan användarens ingripande. Det kan vara önskvärt tooreturn hello programmet toohello primär region när hello-avbrott som har orsakat hello redundans minskas. toodo att hello redundanskommandot ska anropas med hello planerad ”” alternativet. 
* **Synkronisera autentiseringsuppgifter och brandväggsregler**: Vi rekommenderar att du använder [databasen brandväggsregler](sql-database-firewall-configure.md) för geo-replikerade databaser så att de här reglerna kan replikeras med hello databasen tooensure har alla sekundära databaser Hej samma brandväggsregler som hello primära. Den här metoden gör att kunder toomanually konfigurerar och underhåller brandväggsregler på servrar som är värd för både hello primära och sekundära databaser hello behöver. På samma sätt med hjälp av [innehöll databasanvändare](sql-database-manage-logins.md) för data access ser du till både primära och sekundära databaser har alltid hello samma autentiseringsuppgifter så att det finns inga avbrott under växling vid fel, på grund av toomismatches med inloggningar och lösenord. Med hello [Azure Active Directory](../active-directory/active-directory-whatis.md)kunder kan hantera åtkomst tooboth primära och sekundära databaserna och eliminera hello behöver för att hantera autentiseringsuppgifter i databaser helt och hållet.

## <a name="auto-failover-group-capabilities"></a>Automatisk redundans grupp funktioner

Automatisk redundans grupper funktion tillhandahåller en kraftfull abstraktion av aktiv geo-replikering genom att stödja grupp nivån replikering och automatisk redundans. Dessutom tas bort hello nödvändigt toochange hello SQL-anslutningssträng efter växling vid fel genom att tillhandahålla hello ytterligare lyssnare slutpunkter. 

* **Redundansväxlingsgrupp**: en eller flera grupper för växling vid fel kan skapas mellan två servrar i olika regioner (primära och sekundära servrar). Varje grupp kan innehålla en eller flera databaser som återställs som en enhet om alla eller vissa av de databaser som primär inaktiveras på grund av tooan avbrott i hello primära region.  
* **Primär server**: en server som är värd för hello primära databaser i gruppen för hello växling vid fel.
* **Sekundär server**: en server som är värd för hello sekundära databaser i gruppen för hello växling vid fel. hello sekundär server kan inte vara i hello samma region som hello primära servern.
* **Att lägga till databaser toofailover grupp**: du kan placera flera databaser i en server eller i en elastisk pool i hello samma grupp för växling vid fel. Om du lägger till en fristående databas toohello grupp skapas automatiskt en sekundär databas med hjälp av hello samma version och prestanda nivå. Om hello primära databasen är i en elastisk pool, hello sekundära skapas automatiskt i hello elastisk pool med hello samma namn. Om du lägger till en databas som redan har en sekundär databas i hello sekundär server ärvs som geo-replikering av hello grupp.

   > [!NOTE]
   > När du lägger till en databas som redan har en sekundär databas på en server som inte är en del av hello redundansväxlingsgrupp skapas en ny sekundär i hello sekundär server. 
   >

* **Redundans skrivskyddad lyssnare**: skapas från en DNS CNAME-post  **&lt;redundans gruppnamn&gt;. database.windows.net** som pekar toohello aktuella primära server-URL. Tillåter hello skrivskyddad SQL program tootransparently återansluta toohello primära databasen när hello primära ändras efter växling vid fel. 
* **Redundans skrivskyddad lyssnare**: skapas från en DNS CNAME-post  **&lt;redundans gruppnamn&gt;. secondary.database.windows.net** som pekar toohello sekundär server URL: en. Tillåter hello skrivskyddad SQL program tootransparently ansluta toohello sekundär databas med hjälp av hello angivna regler för belastningsutjämning. Alternativt kan du ange om du vill hello skrivskyddad trafik toobe automatiskt omdirigeras toohello primära servern när hello sekundär server inte är tillgänglig.
* **Princip för automatisk redundans**: som standard hello redundansväxlingsgrupp har konfigurerats med en princip för automatisk redundans. hello systemet utlöser redundans så snart hello fel identifieras. Om du vill toocontrol hello redundans arbetsflödet från hello program kan inaktivera du automatisk redundans. 
* **Manuell växling**: du kan initiera redundans manuellt när som helst oavsett hello automatisk redundans-konfigurationen. Om automatisk redundansprincip inte är konfigurerade manuell växling är obligatoriska toorecover databaser i hello redundansväxlingsgrupp. Du kan initiera framtvingad eller eget redundansväxling (med Fullständig datasynkronisering). hello senare kan använda toorelocate hello active server toohello primär region. När redundansväxlingen är klar är hello DNS-poster uppdateras automatiskt tooensure anslutning toohello rätt server.
* **Grace-period med dataförlust**: eftersom hello primära och sekundära databaser är synkroniserad med hjälp av asynkron replikering hello redundans kan resultera i förlust av data. Du kan anpassa hello automatisk redundans princip tooreflect programmets tolerans toodata går förlorade. Genom att konfigurera **GracePeriodWithDataLossHours**, du kan styra hur länge hello systemet ska vänta innan du påbörjar hello växling vid fel som är sannolikt tooresult data går förlorade. 

   > [!NOTE]
   > När systemet identifierar att hello i hello gruppen är online (exempelvis hello avbrott bara påverkas hello service control plan), aktiveras omedelbart hello redundans med Fullständig datasynkronisering (eget failover) oavsett hello värde Ange **GracePeriodWithDataLossHours**. Det här beteendet garanteras att det finns inga data går förlorade under hello återställningen. hello respitperioden gäller endast när ett eget failover inte är möjligt. Om hello avbrott minskas innan hello respitperioden upphör att gälla, aktiveras inte hello växling vid fel.
   >

* **Flera grupper för växling vid fel**: du kan konfigurera flera grupper för växling vid fel för hello samma par av servrar toocontrol hello skalan för växling vid fel. Varje grupp flyttas över oberoende av varandra. Om ditt program med flera innehavare använder elastiska pooler, kan du använda den här funktionen toomix primära och sekundära databaser i varje pool. Det här sättet kan du minska hello effekten av ett avbrott tooonly hälften av hello innehavare.

## <a name="best-practices-of-building-highly-available-service"></a>Metodtips för att skapa tjänst med hög tillgänglighet

toobuild en tjänst med hög tillgänglighet som använder Azure SQL database hello kunder bör följa dessa riktlinjer:
- **Använd failover grupp**: en eller flera grupper för växling vid fel kan skapas mellan två servrar i olika regioner (primära och sekundära servrar). Varje grupp kan innehålla en eller flera databaser som återställs som en enhet om alla eller vissa av de databaser som primär inaktiveras på grund av tooan avbrott i hello primära region. hello redundansväxlingsgrupp skapar geo sekundär databas med hello samma tjänst mål som hello primära. Om du lägger till en befintlig geo-replikering relationen toohello redundans grupp Se till att hello geo sekundära har konfigurerats med hello samma tjänst nivån mål som hello primära.
- **Använd läsa / skriva-lyssnare för OLTP-arbetsbelastning**: när du utför OLTP operations Använd  **&lt;redundans gruppnamn&gt;. database.windows.net** som hello-Serveradress och hello anslutningar kommer att automatiskt dirigerad toohello primära. URL: en ändras inte efter hello redundans.  
Obs hello redundans innebär att uppdatera hello DNS-posten så att hello klientanslutningar omdirigerade toohello nya primära endast efter hello klientens DNS-cachen har uppdaterats.
- **Använd endast Läs-lyssnaren för skrivskyddad arbetsbelastning**: Om du har en logiskt isolerade skrivskyddad arbetsbelastning som är feltolerant toocertain föråldrad av data, du kan använda hello sekundär databas i hello program. För att använda skrivskyddad sessioner  **&lt;redundans gruppnamn&gt;. secondary.database.windows.net** som hello-Serveradress och hello anslutningen kommer att automatiskt dirigerad toohello sekundär. Vi rekommenderar också att du anger i anslutningssträngen läsa avsikten med hjälp av **ApplicationIntent ReadOnly =**. 
- **Förberedas för prestanda försämras**: SQL redundans beslut är oberoende av hello resten av programmet hello eller andra tjänster som används. programmet hello får ”blandas” med vissa komponenter i en region och en del i en annan. tooavoid hello försämring Kontrollera hello redundant programdistribution i hello DR region och följ hello riktlinjerna i den här artikeln.  
Obs hello programmet hello DR region har inte toouse en annan anslutningssträng.  
- **Förbereda för förlust av data**: om ett avbrott har upptäckts SQL automatiskt utlöser skrivskyddad växling vid fel om det finns inga data går förlorade toohello av vår knowledge bästa. I annat fall det väntar på hello period som du angett av **GracePeriodWithDataLossHours**. Om du har angett **GracePeriodWithDataLossHours**, att förbereda för förlust av data. I allmänhet prioriterar Azure under avbrott, tillgänglighet. Om du inte har råd dataförlust, se till att tooset **GracePeriodWithDataLossHours** tooa tillräckligt många, till exempel 24 timmar. 


## <a name="upgrading-or-downgrading-a-primary-database"></a>Uppgradera eller nedgradera en primär databas
Du kan uppgradera eller nedgradera primära databasen tooa olika prestandanivå (inom hello samma servicenivå) utan att koppla från alla sekundära databaser. När du uppgraderar, rekommenderar vi att du först uppgraderar hello sekundära databasen och sedan uppgradera hello primär. När nedgradera, omvänd hello ordning: först nedgradera hello primära och nedgradera hello sekundär. När du uppgraderar eller nedgradera hello databasnivå tooa olika tjänsten tillämpas denna rekommendation. 

> [!NOTE]
> Om du har skapat sekundära databasen som en del av hello redundanskonfiguration grupp är den inte rekommenderade toodowngrade hello sekundär databas. Detta är tooensure data-tier har tillräcklig kapacitet tooprocess din vanliga arbetsbelastning efter växling vid fel är aktiverat. 
>

## <a name="preventing-hello-loss-of-critical-data"></a>Hindrar hello förlust av viktiga data
På grund av toohello hög fördröjning av WAN-nätverk använder kontinuerlig kopiering en asynkron replikering. Asynkron replikering kan data gå förlorade oundvikligt om ett fel inträffar. Vissa program kan dock kräva att inga data går förlorade. tooprotect viktiga uppdateringar, programutvecklare kan anropa hello [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) system proceduren när du genomför hello transaktion. Anropar **sp_wait_for_database_copy_sync** block hello anropar tråd förrän hello senaste allokerad transaktion har överförts toohello sekundär databas. Men väntar den inte hello överförs transaktioner toobe spelas och verkställs på hello sekundär. **sp_wait_for_database_copy_sync** är begränsade tooa specifika kontinuerlig kopiering länk. Alla användare med hello anslutning rättigheter toohello primära databasen kan anropa den här proceduren.

> [!NOTE]
> **sp_wait_for_database_copy_sync** kommer att förhindra förlust av data efter växling vid fel, men det inte garanterar fullständig synkronisering för läsåtkomst. Hej försening som orsakas av en **sp_wait_for_database_copy_sync** proceduranropet kan vara betydande och beror på hello storleken på transaktionsloggen hello när hello hello-anrop. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Programmässigt hantera redundans grupper och aktiv geo-replikering
Som beskrivits tidigare, automatisk redundans grupper (i förhandsversion) och aktiv geo-replikering kan också hanteras via programmering med hjälp av Azure PowerShell och hello REST API. hello följande tabeller beskrivs hello uppsättning kommandon som är tillgängliga.

**Azure Resource Manager API och rollbaserad säkerhet**: aktiv geo-replikering innehåller en uppsättning API: er för Azure Resource Manager för hantering, inklusive hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) och [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Dessa API: er kräver hello användning av resursgrupper och stöder rollbaserad säkerhet (RBAC). Mer information om hur tooimplement åt roller finns [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Flera nya funktioner för aktiv geo-replikering stöds bara via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) baserat [Azure SQL REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) och [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx). Hej [(klassiskt) REST API](https://msdn.microsoft.com/library/azure/dn505719.aspx) och [Azure SQL Database (klassisk) cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) stöds för bakåtkompatibilitet det med hjälp av hello Azure Resource Manager-baserade API: er rekommenderas. 
> 

## <a name="manage-sql-database-failover-using-using-transact-sql"></a>Hantera SQL database växling vid fel med hjälp av Transact-SQL

| Kommando | Beskrivning |
| --- | --- |
| [ALTER DATABASE (Azure SQL-databas)](https://msdn.microsoft.com/library/mt574871.aspx) |Använd Lägg till ON SEKUNDÄRSERVER argumentet toocreate en sekundär databas för en befintlig databas och startar datareplikering |
| [ALTER DATABASE (Azure SQL-databas)](https://msdn.microsoft.com/library/mt574871.aspx) |Använd växling vid fel eller FORCE_FAILOVER_ALLOW_DATA_LOSS tooswitch en sekundär databas toobe primära tooinitiate redundans |
| [ALTER DATABASE (Azure SQL-databas)](https://msdn.microsoft.com/library/mt574871.aspx) |Använd ta bort sekundära på servern tooterminate en datareplikering mellan en SQL-databas och hello angivna sekundära databasen. |
| [sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx) |Returnerar information om alla befintliga replikeringslänkar för varje databas på logisk hello Azure SQL Database-server. |
| [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx) |Hämtar hello senast replikering, senaste replikeringsfördröjning och annan information om hello replikeringslänken för en angiven SQL-databas. |
| [sys.dm_operation_status (Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx) |Visar hello statusen för alla databasåtgärder inklusive hello länkstatus hello replikering. |
| [sp_wait_for_database_copy_sync (Azure SQL Database)](https://msdn.microsoft.com/library/dn467644.aspx) |Gör hello programmet toowait tills alla genomförda transaktioner replikeras och bekräftas av hello aktiva sekundära databasen. |
|  | |

## <a name="manage-sql-database-failover-using-using-powershell"></a>Hantera SQL database växling vid fel med hjälp av PowerShell

| Cmdlet | Beskrivning |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Hämtar en eller flera databaser. |
| [Ny AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Skapar en sekundär databas för en befintlig databas och startar datareplikeringen. |
| [Ange AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Växlar en sekundär databas toobe primära tooinitiate redundans. |
| [Ta bort AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Avbryter datareplikering mellan en SQL-databas och hello angivna sekundära databasen. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Hämtar hello geo-replikeringslänkar mellan en Azure SQL Database och en resursgrupp eller SQL Server. |
| [Ny AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Det här kommandot skapar en redundansväxlingsgrupp och registrerar den på både primära och sekundära servrar|
| [Ta bort AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Tar bort hello redundansväxlingsgrupp från hello-servern och tar bort alla sekundära databaser ingår hello-grupp |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Hämtar hello redundans-konfiguration |
| [Ange AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Ändrar hello konfigurationen av hello redundansväxlingsgrupp |
| [Växeln-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Utlösare redundans för hello redundans grupp toohello sekundär server |
|  | |

> [!IMPORTANT]
> Exempel på skript, se [konfigurera och redundans en enskild databas som använder aktiv geo-replikering](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [konfigurera och redundans en delad databas som använder aktiv geo-replikering](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), och [konfigurera och redundans en redundansväxlingsgrupp för en enskild databas (förhandsgranskning)] (skript/sql-database-setup-geodr-failover-database-failover-group-powershell.md.
>

## <a name="manage-sql-database-failover-using-hello-rest-api"></a>Hantera SQL-databas redundans med hello REST API
| API | Beskrivning |
| --- | --- |
| [Skapa eller uppdatera databasen (createMode = återställer)](/rest/api/sql/Databases/CreateOrUpdate) |Skapar, uppdaterar eller återställer en primär eller sekundär databas. |
| [Hämta skapa eller uppdatera databasens Status](/rest/api/sql/Databases/CreateOrUpdate) |Returnerar hello status under en Skapa-åtgärd. |
| [Ange Sekundär databasen som primär (planerad redundans)](https://docs.microsoft.com/rest/api/sql/replicationlinkss#ReplicationLinks_Failover) |Anger vilka replikdatabasen är primär av misslyckande från hello aktuella primära replikdatabasen. |
| [Ange Sekundär databasen som primär (oplanerad växling vid fel)](https://docs.microsoft.com/rest/api/sql/replicationlinks#ReplicationLinks_FailoverAllowDataLoss) |Anger vilka replikdatabasen är primär av misslyckande från hello aktuella primära replikdatabasen. Den här åtgärden kan resultera i förlust av data. |
| [Hämta replikeringslänk](/rest/api/sql/replicationlinks/get) |Hämtar en specifik replikeringslänk för en angiven SQL-databas i ett partnerskap geo-replikering. Den hämtar hello information visas i vyn för hello sys.geo_replication_links katalog. |
| [Replikeringslänkar - lista av databasen](/rest/api/sql/replicationlinks/listbydatabase) | Hämtar alla länkar för databasreplikering för en angiven SQL-databas i ett partnerskap geo-replikering. Den hämtar hello information visas i vyn för hello sys.geo_replication_links katalog. |
| [Ta bort länk för Platsdatareplikering](/rest/api/sql/databases/delete) | Tar bort en databasreplikeringslänk. Kan inte utföras under växling vid fel. |
| [Skapa eller uppdatera redundans grupp] / rest/api/sql/failovergroups/createorupdate) | Skapar eller uppdaterar en redundansväxlingsgrupp |
| [Ta bort redundans grupp](/rest/api/sql/failovergroups/delete) | Tar bort hello redundansväxlingsgrupp från hello-server |
| [Växling vid fel (planerad)](/rest/api/sql/failovergroups/failover) | Flyttas över från hello aktuella primära toothis server. |
| [Påtvingad redundans tillåta förlust av Data](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails över från hello aktuella primära toothis server. Den här åtgärden kan resultera i förlust av data. |
| [Get redundans grupp](/rest/api/sql/failovergroups/get) | Hämtar en redundansväxlingsgrupp. |
| [Lista redundans grupper av servern](/rest/api/sql/failovergroups/listbyserver) | Visar hello redundans grupper i en server. |
| [Uppdatera grupp om växling vid fel](/rest/api/sql/failovergroups/update) | Uppdaterar en redundansväxlingsgrupp. |
|  | |

## <a name="next-steps"></a>Nästa steg
* För exempel på skript, se:
   - [Konfigurera och redundans en enskild databas som använder aktiv geo-replikering](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Konfigurera och redundans en delad databas som använder aktiv geo-replikering](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Konfigurera och redundans en växling vid fel grupp för en enskild databas (förhandsgranskning)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md).
* toolearn om hur du använder automatisk säkerhetskopiering för återställning, se [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md).
* toolearn om autentiseringskraven för nya primära servern och databasen finns [SQL Database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).

