---
title: aaaHow distributed data fungerar i Azure SQL Data Warehouse | Microsoft Docs
description: "Lär dig hur data distribueras för massivt parallell bearbetning (MPP) och hello alternativ för distribution av tabellerna i Azure SQL Data Warehouse och Parallel Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 9a712d8d5251e4391ede245105918283aaa4b193
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Distribuerade data och distribuerade tabeller för massivt parallell bearbetning (MPP)
Lär dig hur användardata distribueras i Azure SQL Data Warehouse och Parallel Data Warehouse är Microsofts massivt parallell bearbetning (MPP) system. Designa din data warehouse toouse kan distribuerade data effektivt du tooachieve hello frågebearbetning fördelarna med hello MPP-arkitektur. Några databasen designalternativen kan ha en betydande inverkan på förbättra frågeprestanda.  

> [!NOTE]
> Azure SQL Data Warehouse och Parallel Data Warehouse Använd hello samma massivt parallell bearbetning (MPP) design, men de har några skillnader på grund av hello underliggande plattformen. SQL Data Warehouse är en plattform som en tjänst (PaaS) som körs på Azure. Parallel Data Warehouse körs på Analytics Platform System (AP: er) som är ett lokalt program som körs på Windows Server.
> 
> 

## <a name="what-is-distributed-data"></a>Vad är distribuerade data?
I SQL Data Warehouse och Parallel Data Warehouse refererar distribuerade data toouser data som lagras på flera platser över hello MPP-system. Var och en av dessa platser fungerar som en oberoende lagring och bearbetningsenhet som kör frågor på sin del av hello data. Distribuerade data är grundläggande toorunning frågor i parallella tooachieve hög frågeprestanda.

toodistribute data hello datalagret tilldelar varje rad i en tabell tooone distribuerade plats.  Du kan distribuera tabeller med hash-distributionsmetod eller resursallokering metod. hello distributionsmetod har angetts i hello CREATE TABLE-instruktion. 

## <a name="hash-distributed-tables"></a>Hash-distribuerade tabeller
hello följande diagram illustrerar hur en fullständig (icke-distribuerade tabell) hämtar lagras som en distribuerad hash-tabell. En deterministisk funktion tilldelar varje rad toobelong tooone distribution. I hello tabelldefinitionen är en av kolumnerna hello utses hello distribution kolumn. hello hash-funktionen använder hello värde i hello distribution kolumnen tooassign varje rad tooa distribution.

Det finns prestandaöverväganden för hello val av en kolumn, som särskiljbarhet, förskjutning av data och hello typer av frågor som körs på hello system och distribution.

![Distribuerade tabell](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "distribuerade tabell")  

* Varje rad tillhör tooone distribution.  
* En deterministisk hash-algoritm tilldelar varje rad tooone distribution.  
* hello antalet rader per distribution varierar som visas av hello tabeller med olika storlekar.

## <a name="round-robin-distributed-tables"></a>Resursallokering distribuerade tabeller
En distribuerad tabell resursallokering distribuerar hello rader genom att tilldela varje rad tooa distribution i ordning. Det är snabbt tooload data i en tabell med resursallokering men frågeprestanda kanske långsammare.  Ansluta till en tabell med resursallokering vanligtvis måste reshuffling hello rader tooenable hello frågan tooproduce en korrekt resultat som det tar tid.

## <a name="distributed-storage-locations-are-called-distributions"></a>Distribuerade lagringsplatser kallas distributioner
Varje distribuerad plats kallas för en distributionsplats. När en fråga körs parallellt, utför en SQL-fråga på del av hello data varje distribution. SQL Data Warehouse använder SQL toorun hello databasfråga. Parallel Data Warehouse använder SQL Server toorun hello-frågan. Den här delade ingenting arkitekturdesign är grundläggande tooachieving skalbar parallell datorbearbetning.

### <a name="can-i-view-hello-distributions"></a>Kan jag visa hello distributioner?
Varje distribution har ett distribution-ID och visas i systemvyer som gäller tooSQL Data Warehouse och Parallel Data Warehouse. Du kan använda hello distribution ID tootroubleshoot frågeprestanda och andra problem. En lista över hello systemvyer finns hello [MPP system Visa](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Skillnaden mellan en distribution och en beräkningsnod
En distributionsplats är hello grundläggande enheten för att lagra distribuerade data och bearbeta parallella frågor. Distributioner grupperas i Compute-noder. Varje Compute-nod spårar en eller flera distributioner.  

* Analytics Platform System använder Compute-noder som en central del av hello arkitektur och skalbar maskinvarukapacitet. Den använder alltid åtta distributioner per beräkningsnod, som visas i hello diagram för hash-distribuerade tabeller. Hej antalet Compute-noder, och därför hello antal distributioner, bestäms av hello antal Compute-noder som du köper för hello-enhet. Om du köper åtta datornoderna hämta till exempel 64-distributioner (8 noder x 8 distributioner/beräkningsnod). 
* SQL Data Warehouse har en fast antal 60 distributioner och ett flexibelt antal Compute-noder. hello datornoderna implementeras med Azure och -resurser. hello antalet Compute-noder kan ändra bl.a toohello backend service arbetsbelastning och hello beräkningskapacitet (dwu: er) du anger för hello-datalagret. När hello antalet datornoderna ändras, ändras även hello antal distributioner per Compute-nod. 

### <a name="can-i-view-hello-compute-nodes"></a>Kan jag visa hello datornoderna?
Varje Compute-nod har ett nod-ID och visas i systemvyer som gäller tooSQL Data Warehouse och Parallel Data Warehouse.  Du ser hello Compute-nod för hello node_id kolumn i systemvyer vars namn börjar med sys.pdw_nodes. En lista över hello systemvyer finns hello [MPP system Visa](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikerade tabeller
En tabell som har replikerats har ett fullständigt kopia av hello tabell lagras på varje beräkningsnod. Replikera en tabell tar bort hello måste tootransfer data mellan datornoderna innan ett join- eller aggregering. Replikerade tabeller är bara möjligt med små tabeller på grund av hello extra lagring krävs toostore hello fullständig tabell på varje beräkningsnod.  

hello visar följande diagram en replikerad tabell som lagras på varje Compute-nod. För SQL Data Warehouse är hello replikerad tabell fullständigt kopierade tooa distributionsdatabasen på varje beräkningsnod. Parallel Data Warehouse lagras hello replikerad tabell över alla diskar som tilldelats toohello Compute-nod.  Den här disken strategin implementeras med hjälp av SQL Server-filgrupper.  

![Replikerade tabellen](media/sql-data-warehouse-distributed-data/replicated-table.png "replikerade tabell") 

## <a name="next-steps"></a>Nästa steg
toouse distribuerade tabeller effektivt, se [distribuerar tabeller i SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  

