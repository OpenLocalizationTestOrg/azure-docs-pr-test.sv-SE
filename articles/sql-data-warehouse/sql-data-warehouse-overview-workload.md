---
title: aaaLearn om Azure SQL Data Warehouse operations | Microsoft Docs
description: "Elasticiteten i SQL Data Warehouse låter dig öka, minska eller pausa beräkningskraft med hjälp av en glidande skala för informationslagerenheter (DWU:er). Den här artikeln förklarar måtten i informationslager hello och hur de hör ihop tooDWUs. "
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: cadffa9c-589d-4db7-888a-1f202a753bc5
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 8be5ff6b14ab907e2b0a7eb55e0e2f4139aca8b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-workload"></a>Arbetsbelastning i informationslager
En arbetsbelastning i informationslager refererar tooall hello-åtgärder som utförs mot ett data warehouse. hello arbetsbelastningen i informationslagret innefattar hello hela processen med att läsa in data i datalagret hello, utföra analyser och rapportering om hello datalagret, hantera data i datalagret hello och exportera data från hello-datalagret. hello är djupa och breda de här komponenterna ofta proportion till hello ålder för hello-datalagret.

## <a name="new-toodata-warehousing"></a>Nya toodata lagring?
Ett informationslager är en samling data som har lästs in från en eller flera data datakällor och används tooperform business intelligence-åtgärder som rapportering och dataanalys.

Informationslager kännetecknas av frågor som skanna stort antal rader, stora dataintervall och som kan returnera relativt stora resultat för hello analys och rapportering. Informationslager kännetecknas också av relativt stora databelastningar gentemot små infogningar/uppdateringar/borttagningar på transaktionsnivå.

* Ett informationslager fungerar bäst när hello data lagras på ett sätt som optimerar frågor som behöver tooscan stort antal rader eller stora dataintervall. Den här typen genomsökningar fungerar bäst när hello data lagras och genomsöks i kolumner i stället för i rader.

> [!NOTE]
> hello i minnet columnstore-indexet, som använder kolumnen lagring, ger too10x komprimering vinster och 100 x frågeprestanda över traditionell binära träd för rapportering och analys. Vi anser att kolumnlager-index som hello standard för lagring och genomsökning av stora mängder data i ett data warehouse.
> 
> 

* Ett informationslager har andra krav på sig än ett system som optimerats för online transaktionsbearbetning (OLTP). hello OLTP-system har många infoga, uppdatera och ta bort. De här åtgärderna söker toospecific rader i hello tabellen. Tabellsökningar gör bäst när hello data lagras i en rad för rad sätt. hello data kan sorteras och snabbt genomsökas med en Söndra och Erövra metod som kallas binary tree eller btree-sökning.

## <a name="data-loading"></a>Läsa in data
Datainläsning är en stor del av hello arbetsbelastningen för informationslager. Företag har vanligtvis ett upptaget OLTP-system som spårar ändringar under hello dagen medan kunderna genererar affärstransaktioner. Med jämna mellanrum, flyttas hello transaktioner ofta på natten under ett underhållsfönster eller kopieras toohello datalagret. När hello data i datalagret hello kan analytiker utföra analyser och fatta affärsbeslut på hello data.

* Traditionellt kallas hello processen för inläsning ETL för extrahering, transformering och belastning. Data behöver vanligtvis omvandlas så att den är konsekvent med andra data i datalagret hello toobe. Förut använde företag dedikerade ETL-servrar tooperform hello transformationer. Nu med sådana snabb, massivt parallell bearbetning kan du läsa in data till SQL Data Warehouse först och utför hello transformationer. Den här processen kallas extrahera, Load and Transform ELT () och blir den nya standarden för hello arbetsbelastningen för informationslager.

> [!NOTE]
> Med SQL Server 2016 kan du nu utföra analyser i realtid på en OLTP-tabell. Detta inte ersätta hello behovet av att ett data warehouse toostore och analysera data, men den ger en sätt tooperform analys i realtid.
> 
> 

### <a name="reporting-and-analysis-queries"></a>Rapporterings- och analysfrågor
Rapporterings- och analysfrågor indelas ofta i små, medel och stora baserat på ett antal kriterier, men vanligtvis baserat på tidsåtgång. I de flesta informationslager finns det blandade arbetsbelastningar med snabba kontra tidskrävande frågor. I varje fall är det viktigt toodetermine denna blandning och toodetermine frekvensen (varje timme, dagligen, månadsslut, kvartalsvis och så vidare). Det är viktigt toounderstand som hello arbetsbelastning med blandade frågor, tillsammans med samtidighet, lead tooproper kapacitetsplanering för ett datalager.

* Kapacitetsplanering kan vara komplicerat för en arbetsbelastning med blandade frågor, speciellt när du behöver ett långt lead tid tooadd kapacitet toohello data warehouse. SQL Data Warehouse tar bort hello angelägenhetsgrad kapacitetsplanering eftersom dig växa eller krympa bearbetningskapaciteten när som helst och sedan lagring och beräkningskapacitet är oberoende av varandra.

### <a name="data-management"></a>Datahantering
Datahantering är viktig, speciellt när du vet du kanske slut på diskutrymme i hello nära framtid. Informationslager delar vanligtvis hello data i meningsfulla intervall som lagras som partitioner i en tabell. Alla SQL Server-baserade produkter låter dig flytta partitioner in och ut hello tabell. Den här partitionsväxlingen gör du flytta äldre data tooless billigare lagring och hålla hello senaste datan tillgänglig i onlinelagring.

* Kolumnlager-index stöder partitionerade tabeller. Partitionerade tabeller i kolumnlagrings-index används för datahantering och arkivering. För tabeller som lagras rad för rad, har partitioner en viktigare roll i frågeprestanda.  
* PolyBase spelar en viktig roll i datahantering. PolyBase har du hello alternativet tooarchive äldre data tooHadoop eller Azure-blobblagring.  Detta ger flera alternativ eftersom hello data fortfarande är online.  Det kan ta längre tooretrieve data från Hadoop, men hello förhållandet hämtning tid kan uppväger hello lagringskostnaden.

### <a name="exporting-data"></a>Exportera data
Enkelriktade toomake data är tillgängliga för rapporter och analys är toosend data från hello data warehouse tooservers dedikerad för att köra rapporter och analys. De här servrarna kallas för dataförråd. Till exempel du Förbearbeta rapportdata och sedan exportera den från hello data warehouse toomany servrar runtom hälsningsmeddelande toomake den tillgänglig för kunder och analytiker.

* För att generera rapporter varje natt skulle du kunna fylla skrivskyddade Rapporteringsservrar med en ögonblicksbild av hello dagliga data. Det ger högre bandbredd för kunder och sänka hello behövs hello-datalagret. Ur en säkerhetsperspektiv dataförråd dig tooreduce hello antalet användare som har åtkomst till toohello data warehouse.
* För analys, du antingen bygga en analyskub i hello data warehouse och köra analys mot hello datalagret eller Förbearbeta data och exportera det toohello analysservern för ytterligare analys.

## <a name="next-steps"></a>Nästa steg
Nu när du vet lite om SQL Data Warehouse, lär du dig hur tooquickly [skapa ett SQL Data Warehouse] [ create a SQL Data Warehouse] och [läsa in exempeldata][load sample data].

<!--Image references-->

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
