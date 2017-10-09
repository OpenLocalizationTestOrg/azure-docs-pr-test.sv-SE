---
title: "aaaSync data (förhandsgranskning) | Microsoft Docs"
description: "Den här översikten introducerar Azure SQL Data Sync (förhandsversion)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.openlocfilehash: d5b2bbd6a502ba94dba7fb309a6583d2d95cc1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Synkronisera data över flera molntjänster och lokala databaser med SQL datasynkronisering

SQL-datasynkronisering är en tjänst som bygger på Azure SQL Database som gör att du kan synkronisera hello data som du väljer i båda riktningarna över flera SQL-databaser och SQL Server-instanser.

Datasynkronisering baseras runt hello konceptet för en grupp för synkronisering. En synkronisering grupp är en grupp databaser som du vill toosynchronize.

En grupp för synkronisering har hello följande egenskaper:

-   Hej **synkroniseringsschema** beskriver vilka data synkroniseras.

-   Hej **Sync riktning** kan vara dubbelriktad eller kan flöda bara i en riktning. Det vill säga hello Sync-riktningen vara *hubb tooMember* eller *medlem tooHub*, eller båda.

-   Hej **synkroniseringsintervall** är hur ofta synkronisering sker.

-   Hej **konflikt upplösning princip** är en nivå Grupprincip, vilket kan vara *hubb wins* eller *medlem wins*.

Datasynkronisering använder ett nav och eker-topologi toosynchronize data. Du definierar i gruppen hello hello databaserna som hello NAV-databasen. hello resten av hello databaser är medlem databaser. Synkronisera sker bara mellan hello hubb och enskilda medlemmarna.
-   Hej **hubb databasen** måste vara en Azure SQL Database.
-   Hej **medlem databaser** kan vara antingen SQL-databaser, lokal SQL Server-databaser eller SQL Server-instanser på virtuella Azure-datorer.
-   Hej **Sync-databasen** innehåller hello metadata och loggfiler för Data Sync. hello Sync-databasen har toobe Azure SQL-databas finns i hello samma region som hello NAV-databasen. hello Sync-databasen är kunden har skapats och ägs av kunden.

> [!NOTE]
> Om du använder en på lokal databas finns också[konfigurera en lokal agent.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)

![Synkronisera data mellan databaser](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-toouse-data-sync"></a>När toouse Data synkroniseras

Datasynkronisering är användbart i fall där data måste toobe hålls in toodate över flera Azure SQL-databaser eller SQL Server-databaser. Här följer hello huvudsakliga användningsområden för datasynkronisering:

-   **Hybrid datasynkronisering:** med datasynkronisering, kan du synkronisera data mellan din lokala databaser och Azure SQL-databaser tooenable hybridprogram. Den här funktionen kan överklaga toocustomers som överväger glidande toohello molnet och vill tooput några av sina program i Azure.

-   **Distribuerade program:** i många fall är det bra tooseparate olika arbetsbelastningar över olika databaser. Till exempel om du har en stor produktionsdatabas, men du måste också toorun en reporting eller analytics arbetsbelastningen på dessa data, är det bra toohave en andra databas för den här ytterligare arbetsbelastning. Detta minimerar hello prestandapåverkan på din arbetsbelastning i produktion. Du kan använda datasynkronisering tookeep dessa två databaserna synkroniserade.

-   **Globalt distribuerade program:** många företag sträcker sig över flera regioner och även flera länder. toominimize Nätverksfördröjningen, är det bästa toohave dina data i en region Stäng tooyou. Du kan enkelt behålla databaser i regioner världen hello synkroniseras med synkronisering av Data.

Vi rekommenderar inte datasynkronisering för hello följande scenarier:

-   Katastrofåterställning

-   Läs skala

-   ETL (OLTP tooOLAP)

-   Migrering från lokala SQL Server-tooAzure SQL-databas

## <a name="how-does-data-sync-work"></a>Hur fungerar datasynkronisering? 

-   **Spåra dataändringar:** datasynkronisering spårar ändringar genom att använda Infoga, uppdatera och ta bort utlösare. hello ändringar registreras i en tabell på serversidan i hello användardatabas.

-   **Synkronisera data:** datasynkronisering har utformats i en nav och eker-modell. hello synkroniserar hubb med varje medlem individuellt. Ändringar från hello navet är hämtade toohello medlem och sedan ändringar från hello medlemmen är överförda toohello hubb.

-   **Lösa konflikter:** datasynkronisering innehåller två alternativ för konfliktlösning, *hubb wins* eller *medlem wins*.
    -   Om du väljer *hubb wins*, hello ändringar i hello hubb alltid över ändringar i hello medlem.
    -   Om du väljer *medlem wins*, hello ändringar i hello medlem Skriv över ändringar i hello hubb. Om det finns mer än en medlem, beror hello slutliga värdet på vilken medlem synkroniseras först.

## <a name="limitations-and-considerations"></a>Begränsningar och överväganden

### <a name="performance-impact"></a>Inverkan på prestanda
Data Sync använder Infoga, uppdatera och ta bort utlösare tootrack ändringar. Den skapar tabeller sida i hello användardatabas för ändringsspårning. Dessa aktiviteter för spårning av ändring kan påverka din arbetsbelastning i databasen. Utvärdera din tjänstnivå och uppgradera om det behövs.

### <a name="eventual-consistency"></a>Slutliga konsekvensen
Eftersom datasynkronisering utlösaren-baserade, garanteras inte transaktionskonsekvens. Microsoft garanterar att alla ändringar som görs till slut och att datasynkronisering inte leder till dataförlust.

### <a name="unsupported-data-types"></a>Datatyper

-   FileStream

-   SQL/CLR-UDT

-   XMLSchemaCollection (XML stöds)

-   Pekaren och tidsstämpel Hierarchyid

### <a name="requirements"></a>Krav

-   Varje tabell måste ha en primärnyckel.

-   En tabell kan inte ha en identitetskolumn som inte är hello primärnyckel.

-   hello namnen på objekten (databaser, tabeller och kolumner) kan inte innehålla hello utskrivbara tecken punkt (.), vänster hakparentes ([]), eller fyrkantiga höger hakparentes (]).

### <a name="limitations-on-service-and-database-dimensions"></a>Begränsningar för tjänsten och databasen dimensioner

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| **Dimensioner**                                                      | **Gränsen**              | **Lösning**              |
| Maximalt antal synkroniseringsgrupper alla databaser kan tillhöra.       | 5                      |                             |
| Maximalt antal slutpunkter i en enda sync-grupp              | 30                     | Skapa flera synkroniseringsgrupper |
| Maximalt antal lokala slutpunkter i en enda sync-grupp. | 5                      | Skapa flera synkroniseringsgrupper |
| Namn på databasen, tabell, schemat och kolumn                       | 50 tecken per namn |                             |
| Tabeller i en grupp för synkronisering                                          | 500                    | Skapa flera synkroniseringsgrupper |
| Kolumner i en tabell i en grupp för synkronisering                              | 1000                   |                             |
| Datastorlek för rad i en tabell                                        | 24 mb                  |                             |
| Minsta synkroniseringsintervall                                           | 5 minuter              |                             |

## <a name="common-questions"></a>Vanliga frågor

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Hur ofta kan datasynkronisering synkroniserar Mina data? 
hello minsta frekvens är var femte minut.

### <a name="can-i-use-data-sync-toosync-between-sql-server-on-premises-databases-only"></a>Kan jag använda datasynkronisering toosync mellan SQL Server lokala databaser? 
Inte direkt. Du kan synkronisera mellan lokala SQL Server-databaser indirekt, men genom att skapa en Hub-databas i Azure och sedan lägga till hello lokala databaser toohello sync gruppen.
   
### <a name="can-i-use-data-sync-tooseed-data-from-my-production-database-tooan-empty-database-and-then-keep-them-synchronized"></a>Kan jag använda datasynkronisering tooseed data från min produktion tooan tom databasen och hålla dem synkroniserade? 
Ja. Skapa hello schemat manuellt i hello ny databas med hjälp av skript från hello ursprungliga. När du har skapat hello schemat Lägg till hello tabeller tooa synkronisera grupp toocopy hello data och hålla den synkroniserad.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Varför ser tabeller som jag inte kan skapa?  
Datasynkronisering skapar tabeller sida i databasen för ändringsspårning. Ta bort inte eller datasynkronisering slutar fungera.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-hello-value-null-into-hello-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-hello-error"></a>Jag har fått ett felmeddelande som säger ”det går inte att infoga hello värdet NULL i hello kolumnen \<kolumnen\>. Kolumnen tillåter inte null-värden ”. Vad innebär det och hur kan jag åtgärda hello fel? 
Det här felmeddelandet innebär en hello två följande problem:
1.  Det kan finnas en tabell utan en primärnyckel. toofix problemet kan lägga till en primär nyckel tooall hello tabeller du synkronisera.
2.  Det kan finnas en WHERE-sats i CREATE INDEX-instruktionen. Synkronisera hanterar inte det här villkoret. toofix problemet, ta bort hello WHERE-satsen eller manuellt ändrar hello tooall databaser. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-hello-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Hur hanterar datasynkronisering cirkelreferenser? Det vill säga när hello samma data synkroniseras i flera synkroniseringsgrupper och ändras som ett resultat?
Datasynkronisering kan inte hantera cirkelreferenser. Vara säker på att tooavoid dem. 

## <a name="next-steps"></a>Nästa steg

För mer information om SQL-datasynkronisering, se:

-   [Komma igång med datasynkronisering SQL](sql-database-get-started-sql-data-sync.md)

-   Slutföra PowerShell-exempel som visar hur tooconfigure SQL-Data synkroniseras:
    -   [Använd PowerShell toosync mellan flera Azure SQL-databaser](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Använd PowerShell toosync mellan en Azure SQL Database och en lokal SQL Server-databas](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Hämta hello Fullständig datasynkronisering SQL teknisk dokumentation](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [Hämta hello SQL Data Sync REST API-dokumentation](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

För mer information om SQL-databasen, se:

-   [Översikt över SQL-databas](sql-database-technical-overview.md)

-   [Livscykelhantering för databasen](https://msdn.microsoft.com/library/jj907294.aspx)
