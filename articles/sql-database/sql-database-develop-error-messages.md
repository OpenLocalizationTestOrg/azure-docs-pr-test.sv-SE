---
title: "felkoder för aaaSQL - Databasanslutningsfel | Microsoft Docs"
description: "Mer information om SQL-felkoder för SQL-databas klientprogram, till exempel vanliga anslutningsfel i databasen och kopiera databasproblem Allmänt fel. "
keywords: "SQL-felkod, åtkomst till sql, Databasanslutningsfel, sql-felkoder"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2a23e4ca-ea93-4990-855a-1f9f05548202
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 683a7d72c935742f3f9f9a0c683e5d8161167d6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-felkoder för SQL Database-klientprogram: anslutningsfel och andra problem
<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Den här artikeln innehåller SQL-felkoder för SQL Database-klientprogram, inklusive anslutningsfel i databasen, tillfälligt fel (kallas även tillfälliga problem), resurs styrning fel, kopiera databasproblem, elastisk pool och andra fel. De flesta kategorierna viss tooAzure SQL-databas, och gäller inte tooMicrosoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Fel vid anslutning till databasen, tillfälligt fel och andra tillfälliga fel
hello beskrivs följande tabell hello SQL-felkoder för anslutningsfel går förlorade och andra tillfälliga fel som kan uppstå när du försöker tooaccess SQL-databas för dina program. För att komma igång Självstudier om hur tooconnect tooAzure SQL-databas finns [ansluter tooAzure SQL-databas](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>De vanligaste databasen anslutningsfel och tillfälliga fel fel
hello Azure-infrastrukturen har hello möjlighet toodynamically konfigurera underordnade när tunga arbetsbelastningar uppstå hello SQL Database-tjänsten.  Detta dynamiska kan medföra klienten programmet toolose dess anslutning tooSQL databas. Den här typen av fel kallas en *tillfälligt fel*.

Du rekommenderas att klientprogrammet har logik så att den kan återupprätta en anslutning med hello tillfälligt fel tid toocorrect sig själv.  Vi rekommenderar att du fördröjning för 5 sekunder innan din första försök. Försöker igen efter en kortare än 5 sekunder fördröjning risker överväldigande hello-Molntjänsten. För varje efterföljande försök ska hello fördröjning växa exponentiellt, in tooa högst 60 sekunder.

Tillfälligt fel fel visas vanligtvis som en av följande felmeddelanden från ditt klientprogram hello:

* Databasen &lt;%{db_name/&gt; på servern &lt;Azure_instance&gt; är inte tillgänglig. Försök igen senare hello-anslutning. Om hello problemet kvarstår kontaktar du kundsupport och uppger hello sessionsspårnings-ID: &lt;session_id&gt;
* Databasen &lt;%{db_name/&gt; på servern &lt;Azure_instance&gt; är inte tillgänglig. Försök igen senare hello-anslutning. Om hello problemet kvarstår kontaktar du kundsupport och uppger hello sessionsspårnings-ID: &lt;session_id&gt;. (Microsoft SQL Server, fel: 40613)
* En befintlig anslutning tvingades att stänga av hello fjärrvärden.
* System.Data.Entity.Core.EntityCommandExecutionException: Ett fel uppstod när hello kommandodefinition. Se hello ursprungsundantag för detaljer. ---> System.Data.SqlClient.SqlException: ett fel uppstod när resultat skulle tas emot från servern hello. (providern: Session-providern, fel: 19 - fysiska anslutningen kan inte användas)
* Ett försök tooa sekundär databas misslyckades eftersom hello databasen är i hello processen för reconfguration och den är upptagen med att tillämpa nya sidor vid hello mitten av en aktiv transation på hello primära databasen. 

Kodexempel för logik finns:

* [Anslutningsbibliotek för SQL Database och SQLServer](sql-database-libraries.md) 
* [Åtgärder toofix anslutningsfel och tillfälliga fel i SQL-databas](sql-database-connectivity-issues.md)

En beskrivning av hello *blockerande period* för klienter som använder ADO.NET finns i [SQL Server anslutning poolning (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Felkoder för tillfälligt fel
hello följande fel är tillfälliga och bör göras i programlogiken 

| Felkod | Allvarsgrad | Beskrivning |
| ---:| ---:|:--- |
| 4060 |16 |Det går inte att öppna databasen ”%. & #x2a; ls” begärdes vid inloggningen för hello. hello inloggningen misslyckades. |
| 40197 |17 |hello-tjänsten har påträffat ett fel när din begäran bearbetades. Försök igen. Felkod: %d.<br/><br/>Du får det här felet när hello-tjänsten har stoppats på grund av toosoftware eller uppgraderingar av maskinvara, maskinvarufel eller andra failover-problem. hello felkoden (%d) som är inbäddad i hello-meddelande med fel 40197 tillhandahåller ytterligare information om hello typ av fel och växling vid fel som inträffat. Några exempel på hello fel koder är inbäddade i fel 40197 hello-meddelande är 40020, 40143, 40166 och 40540.<br/><br/>Återanslutning tooyour SQL Database-server ansluter automatiskt du tooa felfri kopia av databasen. Ditt program måste fånga 40197, log hello inbäddade fel felkoden (%d) inom hello-meddelande för felsökning och försök ansluta tooSQL databas tills hello resurser är tillgängliga, och anslutningen har upprättats igen. |
| 40501 |20 |hello-tjänsten är upptagen. Försök igen med begäran hello efter 10 sekunder. Incident-ID: %ls. Felkod: %d.<br/><br/>*Obs:* mer information finns:<br/>• [Gränserna för azure SQL Database](sql-database-resource-limits.md). |
| 40613 |17 |Databasen '%. & #x2a; ls' på servern '%. & #x2a; ls' är inte tillgänglig. Försök igen senare hello-anslutning. Om hello problemet kvarstår kontaktar du kundsupport och uppger hello session spårnings-ID (%. & #x2a; ls). |
| 49918 |16 |Det går inte att bearbeta begäran. Det finns inte tillräckligt med resurser tooprocess begäran.<br/><br/>hello-tjänsten är upptagen. Försök hello begäran senare. |
| 49919 |16 |Bearbeta det går inte att skapa eller uppdatera begäran. För många skapande- eller uppdateringsåtgärder pågår för prenumerationen ”% ld”.<br/><br/>hello-tjänsten är upptagen bearbeta flera skapa eller uppdatera begäranden för din prenumeration eller server. Begäranden blockeras för resursoptimering. Frågan [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) för väntande åtgärder. Vänta tills väntande skapa eller uppdatera begäranden har slutförts eller ta bort en av dina väntande begäranden och försök igen med din begäran senare. |
| 49920 |16 |Det går inte att bearbeta begäran. För många åtgärder pågår för prenumerationen ”% ld”.<br/><br/>hello-tjänsten är upptagen med flera förfrågningar för den här prenumerationen. Begäranden blockeras för resursoptimering. Frågan [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) för Åtgärdsstatus. Vänta tills väntande begäranden har slutförts eller ta bort en av dina väntande begäranden och försök igen med din begäran senare. |
| 4221 |16 |Inloggningen tooread sekundära misslyckades på grund av toolong vänta på 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'. hello repliken är inte tillgänglig för inloggning eftersom raden versioner saknas för transaktioner som var relä när hello replik återvanns. hello problemet kan lösas genom att återställa eller acceptera hello aktiva transaktioner på hello primära repliken. Förekomster av det här tillståndet kan minimeras genom att man undviker långa skrivtransaktioner på hello primära. |

## <a name="database-copy-errors"></a>Kopiera databasfel
hello följande fel inträffade kan kopiera en databas i Azure SQL Database. Mer information finns i [Kopiera en Azure SQL Database](sql-database-copy.md).

| Felkod | Allvarsgrad | Beskrivning |
| ---:| ---:|:--- |
| 40635 |16 |Klienten med IP-adressen (%. & #x2a; ls) är tillfälligt inaktiverad. |
| 40637 |16 |Skapa databaskopian är för närvarande inaktiverad. |
| 40561 |16 |Databaskopieringen misslyckades. Antingen hello käll- eller databasen finns inte. |
| 40562 |16 |Databaskopieringen misslyckades. hello källdatabasen har släppts. |
| 40563 |16 |Databaskopieringen misslyckades. hello måldatabasen har släppts. |
| 40564 |16 |Databaskopieringen misslyckades på grund av tooan internt fel. Släpp måldatabasen och försök igen. |
| 40565 |16 |Databaskopieringen misslyckades. Samtidiga databaskopian från samma källa tillåts hello mer än 1. Släpp måldatabasen och försök igen senare. |
| 40566 |16 |Databaskopieringen misslyckades på grund av tooan internt fel. Släpp måldatabasen och försök igen. |
| 40567 |16 |Databaskopieringen misslyckades på grund av tooan internt fel. Släpp måldatabasen och försök igen. |
| 40568 |16 |Databaskopieringen misslyckades. Källdatabasen är inte tillgänglig. Släpp måldatabasen och försök igen. |
| 40569 |16 |Databaskopieringen misslyckades. Måldatabasen är inte tillgänglig. Släpp måldatabasen och försök igen. |
| 40570 |16 |Databaskopieringen misslyckades på grund av tooan internt fel. Släpp måldatabasen och försök igen senare. |
| 40571 |16 |Databaskopieringen misslyckades på grund av tooan internt fel. Släpp måldatabasen och försök igen senare. |

## <a name="resource-governance-errors"></a>Resursen styrning fel
hello följande fel orsakas av överdriven användning av resurser när du arbetar med Azure SQL Database. Exempel:

* En transaktion har varit öppen för länge.
* En transaktion innehåller för många Lås.
* Ett program förbrukar för mycket minne.
* Ett program förbrukar för mycket `TempDb` utrymme.

Närliggande ämnen:

* Mer information finns här: [gränserna för Azure SQL Database](sql-database-resource-limits.md)

| Felkod | Allvarsgrad | Beskrivning |
| ---:| ---:|:--- |
| 10928 |20 |Resurs-ID: %d. hello %s gränsen för hello-databas är %d och har nåtts. Mer information finns i [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>hello resurs-ID anger hello-resurs som har nått gränsen för hello. Trådar, hello i resurs-ID = 1. Sessioner, hello i resurs-ID = 2.<br/><br/>*Obs:* för mer information om felet och hur tooresolve, se:<br/>• [Gränserna för azure SQL Database](sql-database-resource-limits.md). |
| 10929 |20 |Resurs-ID: %d. hello %s minsta garantin är %d, högsta gränsen är %d och hello aktuell användning för hello databasen är %d. Dock hello servern är upptagen toosupport begäranden som är större än %d för den här databasen. Mer information finns i [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Annars, försök igen senare.<br/><br/>hello resurs-ID anger hello-resurs som har nått gränsen för hello. Trådar, hello i resurs-ID = 1. Sessioner, hello i resurs-ID = 2.<br/><br/>*Obs:* för mer information om felet och hur tooresolve, se:<br/>• [Gränserna för azure SQL Database](sql-database-resource-limits.md). |
| 40544 |20 |hello-databas har uppnått sin kvot storlek. Partitionera eller ta bort data, släpp index eller Läs om möjliga lösningar hello-dokumentationen. |
| 40549 |16 |Sessionen avslutas eftersom du har en tidskrävande transaktion. Försök att göra transaktionen kortare. |
| 40550 |16 |hello-sessionen har avslutats eftersom den genererade för många Lås. Försök att läsa eller ändra färre rader i en enda transaktion. |
| 40551 |16 |hello-sessionen har avslutats på grund av hög `TEMPDB` användning. Försök att ändra din fråga tooreduce hello temporärt tabellutrymme.<br/><br/>*Tips:* om du använder temporära objekt spara utrymme i hello `TEMPDB` databasen genom att släppa temporära objekt när de inte längre behövs av hello-sessionen. |
| 40552 |16 |hello-sessionen har avslutats på grund av mycket transaktionsloggutrymme användning av diskutrymme för loggen. Försök att ändra färre rader i en enda transaktion.<br/><br/>*Tips:* om du utför bulkinfogningar med hello `bcp.exe` verktyg eller hello `System.Data.SqlClient.SqlBulkCopy` klassen genom att använda hello `-b batchsize` eller `BatchSize` alternativ toolimit hello antalet rader kopierade toohello server i varje transaktion. När du återskapar ett index med hello `ALTER INDEX` instruktionen, försök att använda hello `REBUILD WITH ONLINE = ON` alternativet. |
| 40553 |16 |hello-sessionen har avslutats på grund av omfattande minnesanvändning. Försök att ändra din fråga tooprocess färre rader.<br/><br/>*Tips:* minska hello antalet `ORDER BY` och `GROUP BY` åtgärder i Transact-SQL-kod minskar hello minneskrav av din fråga. |

## <a name="elastic-pool-errors"></a>Elastisk pool-fel
hello följande fel är relaterade toocreating och använda Elastics pooler.

| ErrorNumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX_RESOURCE |hello elastiska poolen har nått sin lagringsgräns. hello lagringskvoten för hello elastiska poolen får inte överskrida (%d) MB. |Elastisk pool gräns för diskutrymme i MB. |Försöker toowrite data tooa databas när hello lagringsgränsen för hello elastisk pool har uppnåtts. |Överväg att öka hello Dtu för hello elastisk pool om möjligt i ordning tooincrease sin lagringsgräns, minska hello lagringsutrymme som används av enskilda databaser inom hello elastisk pool eller ta bort databaser från hello elastiska poolen. |
| 10929 |EX_USER |hello %s minsta garantin är %d, högsta gränsen är %d och hello aktuell användning för hello databasen är %d. Dock hello servern är upptagen toosupport begäranden som är större än %d för den här databasen. Se [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) om du behöver hjälp. Annars, försök igen senare. |Minsta DTU per databas. Max DTU per databas |Hej totala antalet samtidiga arbetare (antal begäranden) över alla databaser i hello elastiska poolen försök tooexceed hello poolen gränsen. |Överväg att öka hello dtu: er för hello elastisk pool om möjligt i ordning tooincrease worker gränsen, eller ta bort databaser från hello elastiska poolen. |
| 40844 |EX_USER |Databasen %ls på %ls-servern är %ls edition-databas i en elastisk pool och kan inte ha en kontinuerlig kopiering relation. |databasens namn, databasversionen, servernamn |En StartDatabaseCopy-kommandot har utfärdats för en icke-premium db i en elastisk pool. |Kommer snart |
| 40857 |EX_USER |Elastisk pool saknas för servern: %ls, namn på elastisk pool: %ls. |namnet på servern. namn på elastisk pool |Angivna elastisk pool finns inte i angivna hello-servern. |Ange ett giltigt elastisk pool-namn. |
| 40858 |EX_USER |Elastisk pool %ls finns redan på servern: %ls |namn på elastisk pool, servernamn |Angivna elastisk pool finns redan i hello angivna logisk server. |Ange namn på ny elastisk pool. |
| 40859 |EX_USER |Elastiska poolen stöder inte tjänstnivån %ls. |elastiska pooltjänstnivå |Angivna tjänstnivån stöds inte för etablering av elastisk pool. |Ange hello rätt version eller lämna nivån tomt toouse hello standard service tjänstnivån. |
| 40860 |EX_USER |Kombinationen av elastiska poolen %ls och tjänsten målet %ls är ogiltig. |namn på elastisk pool; namn på servicenivåmål |Elastisk pool och tjänsten målet kan anges samtidigt endast om tjänstmålet anges som 'ElasticPool'. |Ange rätt kombination av elastiska poolen och tjänstmålet. |
| 40861 |EX_USER |hello databasversionen ' %. *ls' kan inte vara ett annat än hello elastiska pooltjänstnivå som är ' %.* ls'. |Databasversionen elastiska pooltjänstnivå |hello databasversionen skiljer sig hello elastiska pooltjänstnivå. |Ange inte en databasversionen som skiljer sig från hello elastiska pooltjänstnivå.  Observera att hello databasversionen inte behöver toobe som angetts. |
| 40862 |EX_USER |Namn på elastisk pool måste vara anges om hello elastisk pool tjänstmålet har angetts. |Ingen |Elastisk pool tjänstmålet identifiera inte unikt en elastisk pool. |Ange namn på elastisk pool hello om använder tjänstmålet för hello elastisk pool. |
| 40864 |EX_USER |Hej dtu: er för hello elastiska poolen måste vara minst (%d) dtu: er för tjänstnivån ' %. * ls'. |Dtu: er för elastiska poolen; elastiska pooltjänstnivå. |Försöker tooset hello dtu: er för hello elastiska poolen nedan hello lägsta tillåtna värdet. |Försök att ange hello dtu: erna för hello elastisk pool tooat minst hello lägsta tillåtna värdet. |
| 40865 |EX_USER |Hej dtu: er för hello elastiska poolen får inte överskrida (%d) dtu: er för tjänstnivån ' %. * ls'. |Dtu: er för elastiska poolen; elastiska pooltjänstnivå. |Försöker tooset hello dtu: er för hello elastiska poolen ovan hello maxgränsen. |Försök igen om hello dtu: erna för hello elastisk pool toono större än hello maxgränsen. |
| 40867 |EX_USER |Hej max DTU per databas måste vara minst (%d) för tjänstnivån ' %. * ls'. |Max DTU per databas. elastiska pooltjänstnivå |Försök tooset Hej max DTU per databas under hello begränsning som stöds. |Överväg att använda hello elastiska pooltjänstnivå som stöder hello önskad inställningen. |
| 40868 |EX_USER |Hej max DTU per databas får inte överskrida (%d) för tjänstnivån ' %. * ls'. |Max DTU per databas. elastiska pooltjänstnivå. |Försök tooset Hej max DTU per databas utöver hello begränsning som stöds. |Överväg att använda hello elastiska pooltjänstnivå som stöder hello önskad inställningen. |
| 40870 |EX_USER |minsta för hello DTU per databas får inte överskrida (%d) för tjänstnivån ' %. * ls'. |Minsta DTU per databas. elastiska pooltjänstnivå. |Försök minsta tooset hello DTU per databas utöver hello begränsning som stöds. |Överväg att använda hello elastiska pooltjänstnivå som stöder hello önskad inställningen. |
| 40873 |EX_USER |hello antalet databaser (%d) och minsta DTU per databas (%d) får inte överstiga hello dtu: er för hello elastisk pool (%d). |Antalet databaser i elastiska poolen; Minsta DTU per databas. Dtu: er för elastisk pool. |Försöker toospecify DTU min för databaser i hello elastisk pool som överskrider hello dtu: er för hello elastisk pool. |Överväg att öka hello dtu: er för hello elastisk pool, eller minska minsta för hello DTU per databas, eller minska hello antal databaser i hello elastisk pool. |
| 40877 |EX_USER |En elastisk pool kan inte tas bort om den inte innehåller några databaser. |Ingen |hello elastiska poolen innehåller en eller flera databaser och därför kan inte tas bort. |Ta databaser bort från hello elastisk pool i ordning toodelete den. |
| 40881 |EX_USER |Hej elastisk pool ' %. * ls' har nått sin gräns för antalet av databasen.  hello databasen gränsvärde för hello elastiska poolen får inte överskrida (%d) för en elastisk pool med (%d) dtu: er. |Namn på elastisk pool; Antal begränsning på elastisk pool; e dtu: er för resurspoolen. |Försöker toocreate eller Lägg till tooelastic databaspool när hello databasen antal för hello elastisk pool har nåtts. |Överväg att öka hello dtu: er för hello elastisk pool om möjligt i ordning tooincrease sin gräns för databasen, eller ta bort databaser från hello elastiska poolen. |
| 40889 |EX_USER |Hej dtu: erna eller lagringsgränsen för hello elastisk pool ' %. * ls' kan inte minskas eftersom som inte skulle ger tillräckligt med lagringsutrymme för dess databaser. |Namn på elastisk pool. |Försöker toodecrease hello lagringsgränsen för hello elastisk pool under dess lagringskvoten. |Överväg att minska hello lagringskvoten för enskilda databaser i hello elastisk pool eller ta bort databaser från hello-pool i ordning tooreduce dess dtu: er eller lagring gränsen. |
| 40891 |EX_USER |minsta för hello DTU per databas (%d) får inte överskrida max för hello DTU per databas (%d). |Minsta DTU per databas. Max DTU per databas. |Försöker minsta tooset hello DTU per databas som är högre än Hej max DTU per databas. |Kontrollera att hello DTU min. per databaser inte överstiger Hej max DTU per databas. |
| TBD |EX_USER |hello lagringsstorleken för en individuell databas i en elastisk pool får inte överskrida hello maxstorlek som tillåts av ' %. * ls' service tier elastisk pool. |elastiska pooltjänstnivå |Hej max storlek för hello-databasen överskrider hello maxstorlek som tillåts av hello elastiska pooltjänstnivå. |Ange hello maxstorleken på hello databas inom hello maxstorlek som tillåts av hello elastiska pooltjänstnivå hello gränser. |

Närliggande ämnen:

* [Skapa en elastisk pool (C#)](sql-database-elastic-pool-manage-csharp.md) 
* [Hantera en elastisk pool (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Skapa en elastisk pool (PowerShell)](sql-database-elastic-pool-manage-powershell.md) 
* [Övervaka och hantera en elastisk pool (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Allmänt fel
Följande fel hello omfattas inte i alla tidigare kategorier.

| Felkod | Allvarsgrad | Beskrivning |
| ---:| ---:|:--- |
| 15006 |16 |<AdministratorLogin>är inte ett giltigt namn eftersom det innehåller ogiltiga tecken. |
| 18452 |14 |Inloggningen misslyckades. hello inloggningen kommer från en icke betrodd domän och kan inte användas med Windows authentication.%. & #x2a; ls (Windows-inloggningar inte stöds i den här versionen av SQL Server.) |
| 18456 |14 |Inloggningen misslyckades för användaren ' %. #x2a;ls'.%. & #x2a % ls. & #x2a; ls (hello inloggningen misslyckades för användaren ”%. & #x2a; ls”. Det gick inte att ändra lösenordet för hello. Lösenordsändring under inloggning stöds inte i den här versionen av SQL Server.) |
| 18470 |14 |Inloggningen misslyckades för användaren (%. & #x2a; ls). Orsak: hello-kontot är disabled.%. & #x2a; ls |
| 40014 |16 |Flera databaser kan inte användas i hello samma transaktion. |
| 40054 |16 |Tabeller utan grupperat index stöds inte i den här versionen av SQL Server. Skapa ett grupperat index och försök igen. |
| 40133 |15 |Den här åtgärden stöds inte i den här versionen av SQL Server. |
| 40506 |16 |Angiven SID är ogiltigt för den här versionen av SQL Server. |
| 40507 |16 |' %. & #x2a; ls' kan inte anropas med parametrar i den här versionen av SQL Server. |
| 40508 |16 |USE-instruktionen är inte stöds tooswitch mellan databaser. Använd en ny anslutning tooconnect tooa annan databas. |
| 40510 |16 |Instruktionen '%. & #x2a; ls' stöds inte i den här versionen av SQL Server |
| 40511 |16 |Den inbyggda funktionen '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40512 |16 |Föråldrad funktion %ls stöds inte i den här versionen av SQL Server. |
| 40513 |16 |Servern variabeln '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40514 |16 |%ls stöds inte i den här versionen av SQL Server. |
| 40515 |16 |Toodatabase och/eller server referensnamn i '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40516 |16 |Globala temporära objekt stöds inte i den här versionen av SQL Server. |
| 40517 |16 |Nyckelordet eller instruktionsalternativet '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40518 |16 |DBCC-kommandot '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40520 |16 |Den skyddbara klassen '% S_MSG' stöds inte i den här versionen av SQL Server. |
| 40521 |16 |Den skyddbara klassen '% S_MSG' stöds inte i hello serverdefinitionsområdet i den här versionen av SQL Server. |
| 40522 |16 |Databastyp UPN '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40523 |16 |Skapa en implicit användarroll '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. Skapa hello användaren explicit innan du använder den. |
| 40524 |16 |Datatypen '%. & #x2a; ls' stöds inte i den här versionen av SQL Server. |
| 40525 |16 |MED '%.ls' stöds inte i den här versionen av SQL Server. |
| 40526 |16 |' %. & #x2a; ls' raduppsättningsprovidern stöds inte i den här versionen av SQL Server. |
| 40527 |16 |Länkade servrar stöds inte i den här versionen av SQL Server. |
| 40528 |16 |Användare får inte vara mappade toocertificates, asymmetriska nycklar eller Windows-inloggningsnamn i den här versionen av SQL Server. |
| 40529 |16 |Den inbyggda funktionen '%. & #x2a; ls' personifiering kontexten inte stöds i den här versionen av SQL Server. |
| 40532 |11 |Det går inte att öppna servern ”%. & #x2a; ls” begärdes vid inloggningen för hello. hello inloggningen misslyckades. |
| 40553 |16 |hello-sessionen har avslutats på grund av omfattande minnesanvändning. Försök att ändra din fråga tooprocess färre rader.<br/><br/>*Obs:* minska hello antalet `ORDER BY` och `GROUP BY` åtgärder i Transact-SQL-kod bidrar till att minska hello minneskrav av din fråga. |
| 40604 |16 |Det gick inte CREATE/ALTER DATABASE eftersom den skulle överskrida hello kvoten hello-Server. |
| 40606 |16 |Koppla databaser stöds inte i den här versionen av SQL Server. |
| 40607 |16 |Windows-inloggningar stöds inte i den här versionen av SQL Server. |
| 40611 |16 |Servrar kan ha högst 128 brandväggsregler definierats. |
| 40614 |16 |Första IP-adress för brandväggsregeln får inte överstiga sista IP-adress. |
| 40615 |16 |Det går inte att öppna servern '{0}' som begärdes vid inloggningen för hello. Klienten med IP-adressen är '{1}' inte tillåten tooaccess hello server.  tooenable åtkomst, Använd hello SQL Database-portalen eller kör sp_set_firewall_rule på hello huvuddatabasen toocreate en brandväggsregel för IP-adressen eller adressintervallet.  Det kan ta upp toofive minuter för den här ändringen tootake effekt. |
| 40617 |16 |hello brandväggsregeln som inleds med <rule name> är för långt. Maxlängden är 128. |
| 40618 |16 |hello brandväggsregeln får inte vara tomt. |
| 40620 |16 |hello inloggningen misslyckades för användaren ”%. & #x2a; ls”. Det gick inte att ändra lösenordet för hello. Lösenordsändring under inloggning stöds inte i den här versionen av SQL Server. |
| 40627 |20 |Åtgärden på servern och databasen: {0} '{1}' pågår. Vänta några minuter innan du försöker igen. |
| 40630 |16 |Lösenordsverifieringen misslyckades. hello lösenordet uppfyller inte principkraven eftersom det är för kort. |
| 40631 |16 |hello-lösenord som du angav är för långt. hello lösenord ska ha mer än 128 tecken. |
| 40632 |16 |Lösenordsverifieringen misslyckades. hello lösenordet uppfyller inte principkraven eftersom det inte är tillräckligt komplicerat. |
| 40636 |16 |Det går inte att använda ett reserverat namn (%. & #x2a; ls) i den här åtgärden. |
| 40638 |16 |Ogiltigt prenumerations-id < prenumerations-id >. Prenumerationen finns inte. |
| 40639 |16 |Begäran stämmer inte överens tooschema: <schema error>. |
| 40640 |20 |hello-servern påträffade ett oväntat undantag. |
| 40641 |16 |hello angivna platsen är ogiltig. |
| 40642 |17 |hello-servern är upptagen. Försök igen senare. |
| 40643 |16 |hello angetts x-ms-versionsvärdhuvud är ogiltigt. |
| 40644 |14 |Misslyckade tooauthorize åtkomst toohello angivna prenumerationen. |
| 40645 |16 |ServerName <servername> får inte vara tomt eller null. Det kan endast bestå av gemener ”a”-”z” hello siffrorna 0-9 och bindestreck hello. hello bindestreck kan inte leda eller slutet i hello namn. |
| 40646 |16 |Prenumerations-ID kan inte vara tomt. |
| 40647 |16 |Prenumeration < prenumerations-id har inte servern servername. |
| 40648 |17 |För många begäranden har utförts. Försök igen senare. |
| 40649 |16 |Ogiltig innehållstyp anges. Endast application/xml stöds. |
| 40650 |16 |Prenumerationen < prenumerations-id > finns inte eller är inte klar för hello igen. |
| 40651 |16 |Det gick inte toocreate server eftersom hello prenumeration < prenumerations-id > är inaktiverad. |
| 40652 |16 |Det går inte att flytta eller skapa server. Prenumeration < prenumerations-id > kommer att överskrida serverkvoten. |
| 40671 |17 |Kommunikationsfel mellan hello gateway och hello management-tjänsten. Försök igen senare. |
| 40852 |16 |Det går inte att öppna databasen ' %. *ls' på servern ' %.* ls' begärdes vid inloggningen för hello. Access-databas som toohello tillåts endast med hjälp av en säkerhetsaktiverad anslutningssträng. tooaccess den här databasen, ändra din anslutning strängar toocontain säker i hello serverns FQDN - 'servernamn'.database.windows .net ska vara ändrade too'server namn '.database. `secure`. windows.net. |
| 45168 |16 |hello SQL Azure-systemet är under belastning och placerar en övre gräns för samtidiga DB CRUD-åtgärder för en enskild server (t.ex. Skapa databas). hello-server som anges i felmeddelandet hello överskred hello maximalt antal samtidiga anslutningar. Försök igen senare. |
| 45169 |16 |hello SQL azure-systemet är under belastning och placerar en övre gräns för hello antalet samtidiga server CRUD-åtgärder för en enda prenumeration (t.ex. Skapa server). hello prenumerationen som anges i hello fel meddelandet överskred hello maximalt antal samtidiga anslutningar och hello begäran nekades. Försök igen senare. |

## <a name="related-links"></a>Relaterade länkar
* [Azure SQL Database-funktioner](sql-database-features.md)
* [Gränserna för Azure SQL-databas](sql-database-resource-limits.md)

