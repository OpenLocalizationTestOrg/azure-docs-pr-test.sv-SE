---
title: aaaIdentify scenarier och planera din analytics-process - Azure | Microsoft Docs
description: "Planera för avancerade analyser av ett antal viktiga frågor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 421520dd-7728-4d29-889c-ebe6a0a6fb07
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: e445973be0d020a4f9949e5c9d8554fbbd4b515f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooidentify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Hur tooidentify scenarier och planera för advanced analytics databearbetning
Vilka resurser bör du planera tooinclude när du konfigurerar en miljö toodo avancerade analyser bearbetning på en datamängd? Den här artikeln föreslår ett antal frågor tooask som hjälper dig att identifiera hello uppgifter och resurser som är relevanta för ditt scenario. hello ordning anvisningar för förutsägelseanalys beskrivs i [vad är hello Team Data vetenskap processen (TDSP)?](data-science-process-overview.md). Var och en av dessa steg kräver särskilda resurser för hello uppgifter relevanta tooyour visst scenario. Hej viktiga frågor tooidentify ditt scenario problem data logistik, egenskaper, hello kvalitet hello datauppsättningar och hello verktyg och språk som du föredrar toodo hello analys.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistic frågor: data platser och flytt
Hej logistic frågor rör hello platsen för hello **datakällan**, hello **målområdet** i Azure och krav för glidande hello data, inklusive hello schema, belopp och resurser delta. hello data behöva toobe flyttas flera gånger under hello analytics processen. Ett vanligt scenario är toomove lokala data till någon form av lagring på Azure och sedan till Machine Learning Studio.

1. **Vad är din datakälla?** Är den lokala eller i molnet hello? Exempel:
   
   * hello data är offentligt tillgänglig på en HTTP-adress.
   * hello data finns på en lokal fil/nätverksplats.
   * hello data är i en SQL Server-databas.
   * hello data lagras i en Azure storage-behållare
2. **Vad är hello Azure mål?** Där behöver den toobe för bearbetning eller modeling? Exempel:
   
   * Azure Blob Storage
   * SQL Azure-databaser
   * SQL Server på virtuella Azure-datorer
   * HDInsight (Hadoop på Azure) eller Hive-tabeller
   * Azure Machine Learning
   * Monteras Azure virtuella hårddiskar.
3. **Hur ska du toomove hello data?**  hello procedurer och resurser tillgängliga tooingest eller läsa in data i en mängd olika lagringsplatser och standardmiljöer för bearbetning beskrivs i följande avsnitt hello.
   
   * [Läs in data till lagring miljöer för analys](machine-learning-data-science-ingest-data.md)
   * [Importera utbildningsdata till Azure Machine Learning Studio från olika datakällor](machine-learning-data-science-import-data.md).
4. **Behöver hello data toobe flyttas enligt ett schema eller ändras under migreringen?** Överväg att använda Azure Data Factory (ADM) när data måste toobe migreras kontinuerligt, särskilt om ett hybridscenario som har åtkomst till både lokalt och molnresurser ingår, eller när hello data är överförd eller måste toobe ändras eller har affärslogik lägga till tooit i hello kursen migreras. Mer information finns i [flytta data från en lokal SQL server tooSQL Azure med Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)
5. **Hur mycket av hello data är toobe flyttas tooAzure?** Datauppsättningar som är mycket stora kan överskrida hello lagringskapaciteten för vissa miljöer. Ett exempel finns i hello beskrivning av storleksgränser för Machine Learning Studio i hello nästa avsnitt. I sådana fall kan ett exempel på hello data används under hello analys. Mer information om hur toodown sampel en datamängd i olika Azure-miljöer finns [exempeldata i hello Team datavetenskap Process](machine-learning-data-science-sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Data egenskaper frågor: typ, format och storlek
Dessa frågor är viktiga tooplanning din lagring och bearbetning av miljöer, som är lämpliga toovarious typer av data och som har vissa begränsningar.

1. **Vad är hello datatyper?** Exempel:
   
   * Numeriskt
   * Kategoriska
   * Strängar
   * Binär
2. **Hur formateras dina data?** Exempel:
   
   * Kommaavgränsad (CSV) eller tabbavgränsade (TVS) flata filer
   * Komprimerad eller okomprimerad
   * Azure BLOB
   * Hadoop Hive-tabeller
   * SQL Server-tabeller
3. **Hur stor är dina data?**
   
   * Liten: mindre än 2GB
   * Medel: Större än 2GB och mindre än 10GB
   * Stora: Större än 10GB

Ta till exempel hello Azure Machine Learning Studio-miljön:

* En lista över hello dataformat och typer som stöds av Azure Machine Learning Studio finns [dataformat och datatyper som stöds](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) avsnitt.
* Information om begränsningar för Azure Machine Learning Studio finns hello **hur stor kan datauppsättningen hello vara för Mina moduler?** avsnitt i [importera och exportera data för Machine Learning](machine-learning-faq.md#machine-learning-studio-questions)

Mer information om hello begränsningar av andra Azure-tjänster används i hello analytics finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Data quality frågor: undersökning och föregående bearbetning
1. **Vad kan du om dina data?** Utforska data när du behöver toogain en förstå de grundläggande egenskaperna. Vad mönster eller trender den bevisföremål, vad avvikare har eller hur många värden som saknas. Det här steget är viktigt för att fastställa hello omfattningen av föregående bearbetning som krävs för utformningen av hypoteser som kan föreslå hello lämpligaste funktioner eller typ av analys och utforma planer för ytterligare data samlas. Beräkna beskrivande statistik och rita visualiseringar är värdefulla metoder för kontroll av data. Mer information om hur tooexplore en datamängd i olika miljöer i Azure, se [utforska data i hello Team datavetenskap Process](machine-learning-data-science-explore-data.md).
2. **Kräver hello data förbearbetning eller rensa?**
   Förbearbetning och rensa data är viktiga uppgifter som vanligtvis utföras innan dataset kan användas effektivt för machine learning. Rådata är ofta mycket brus och otillförlitliga och saknar värden. Med hjälp av dessa data för modellering kan ge missvisande resultat. En beskrivning finns [uppgifter tooprepare data för förbättrad machine learning](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Verktyg och språk-frågor
Det finns många alternativ beroende på vilka språk och utvecklingsmiljöer eller verktyg du behöver eller mest conformable använder.

1. **Språk gör du föredrar toouse för analys?**  
   
   * R
   * Python
   * SQL
2. **Vilka verktyg för dataanalys ska du använda?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -ett skriptspråk som används tooadminister Azure-resurser i ett skriptspråk.
   * [Azure Machine Learning Studio](machine-learning-what-is-ml-studio.md)
   * [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
   * [RStudio](http://www.rstudio.com)
   * [Python Tools för Visual Studio](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter-anteckningsböcker](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identifiera avancerade analyser-scenario
När du har besvarat frågorna för hello hello föregående avsnitt, är du redo toodetermine vilket scenario som bäst passar ditt ärende. hello exempelscenarier beskrivs i [scenarier för avancerade analyser i Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).

