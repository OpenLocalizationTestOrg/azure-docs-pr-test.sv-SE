---
title: aaaDemand prognos i energi Technical Guide | Microsoft Docs
description: "En teknisk guide toohello Lösningsmall med Microsoft Cortana Intelligence för begäran vid en prognos i energiförbrukning."
services: cortana-analytics
documentationcenter: 
author: yijichen
manager: ilanr9
editor: yijichen
ms.assetid: 7f1a866b-79b7-4b97-ae3e-bc6bebe8c756
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: inqiu;yijichen;ilanr9
ms.openlocfilehash: c97b7c19c9e3a317aecc329e61a0692d2f1ec53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Tekniska guide toohello Cortana Intelligence Lösningsmall för begäran vid en prognos i energi
## <a name="overview"></a>**Översikt**
Lösningsmallar är utformade tooaccelerate hello processen för att skapa en E2E demo ovanpå Cortana Intelligence Suite. En mall för distribuerade etablera din prenumeration med nödvändiga Cortana Intelligence-komponenten och bygga hello relationer mellan. Det lägger också hello data pipeline med exempeldata komma genereras från ett simuleringen program. Hämta hello data simulator från hello länken och installera den på din lokala dator finns i Viktigt.txt toohello anvisningar om hur du använder hello simulatorn. Data som genereras från hello simulator kommer hydrate hello data pipeline och börja generera machine learning förutsägelse som kan sedan visualiserade på hello Power BI-instrumentpanelen.

Hej lösningsmall hittar [här](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

hello distributionsprocessen vägleder dig igenom flera steg tooset in autentiseringsuppgifterna lösning. Kontrollera att du registrera dessa autentiseringsuppgifter, till exempel lösningens namn, användarnamn och lösenord som du anger under distributionen av hello.

hello syftet med det här dokumentet är tooexplain hello-Referensarkitektur och olika komponenter som har etablerats i din prenumeration som en del av den här lösningen mallen. hello dokumentet berättar också om hur tooreplace hello exempeldata med verkliga data i din egen toobe kan toosee insikter/förutsägelser som du vann data. Dessutom hello dokumentet pratar om hello delar av hello Lösningsmall som behöver toobe ändras om du vill toocustomize hello-lösningen med dina egna data. Anvisningar om hur toobuild hello Power BI-instrumentpanel för den här lösningen mallen tillhandahålls hello slutet.

## <a name="big-picture"></a>**Stor bild**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Arkitekturen beskrivs
När hello-lösningen distribueras olika Azure-tjänster i Cortana Analytics Suite aktiveras (*dvs.* Event Hub, Stream Analytics, HDInsight, Data Factory Maskininlärning *etc.*). hello arkitekturdiagrammet ovan visas på en hög nivå hur hello begäran prognoser för energi Lösningsmall är uppbyggd från slutpunkt till slutpunkt. Du kommer att kunna tooinvestigate tjänsterna genom att klicka på dem på hello lösning mallen diagram som skapats med hello distribution av hello lösning. hello följande avsnitt beskrivs varje del.

## <a name="data-source-and-ingestion"></a>**Datakällan och införandet**
### <a name="synthetic-data-source"></a>Syntetiska datakälla
För den här mallen hello data genereras källa används från ett program som du kan hämta och kör lokalt efter slutförd distribution. Du hittar hello instruktioner toodownload och installera det här programmet i fält för hello egenskaper när du väljer hello första noden uppmanade hello lösning mallen diagram energi prognoser Data simulatorn. Det här programmet feeds hello [Azure Event Hub](#azure-event-hub) med datapunkter, eller händelser som ska användas i hello resten av hello lösning flödet.

hello händelse generation program kommer att fylla i hello Azure Event Hub endast när det körs på datorn.

### <a name="azure-event-hub"></a>Azure Event Hub
Hej [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) service är hello mottagaren av hello indata hello syntetiska datakälla som beskrivs ovan.

## <a name="data-preparation-and-analysis"></a>**Förberedelse av data och analys**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hej [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) tjänsten är används tooprovide nära analys i realtid på hello Indataströmmen från hello [Azure Event Hub](#azure-event-hub) tjänst och publicera resultatet till en [Power BI](https://powerbi.microsoft.com) instrumentpanelen som arkiverar alla rådata inkommande händelser toohello [Azure Storage](https://azure.microsoft.com/services/storage/) tjänsten för senare bearbetning av hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service.

### <a name="hd-insights-custom-aggregation"></a>HD insikter anpassade aggregering
hello Azure HD Insight service är används toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript (styrd av Azure Data Factory) tooprovide aggregeringar på raw hello-händelser som har arkiverats hello Azure Stream Analytics-tjänsten.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hej [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) tjänsten används (styrd av Azure Data Factory) toomake prognos på framtida energiförbrukningen för en viss region anges hello indata togs emot.

## <a name="data-publishing"></a>**Publicering av data**
### <a name="azure-sql-database-service"></a>Azure SQL Database-tjänsten
Hej [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) tjänsten är används toostore (hanteras av Azure Data Factory) hello förutsägelser som tagits emot av hello Azure Machine Learning-tjänst som ska användas i hello [Power BI](https://powerbi.microsoft.com) instrumentpanelen.

## <a name="data-consumption"></a>**Användning av data**
### <a name="power-bi"></a>Power BI
Hej [Power BI](https://powerbi.microsoft.com) tjänsten är används tooshow en instrumentpanel som innehåller aggregeringar som tillhandahålls av hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) tjänsten samt begäran prognos resultat lagras i [Azure SQL Databasen](https://azure.microsoft.com/services/sql-database/) som skapas med hjälp av hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service. Anvisningar om hur toobuild hello Power BI-instrumentpanel för den här lösningen-mallen finns i toohello nedan.

## <a name="how-toobring-in-your-own-data"></a>**Hur toobring i dina egna data**
Det här avsnittet beskrivs hur toobring tooAzure egna data och vilka områden kräver ändringar för hello data du sätta i den här arkitekturen.

Det är inte troligt att alla dataset som du kan aktivera matchar hello dataset används för den här lösningen mallen. Förstå dina data och kraven kommer att vara avgörande i hur du ändrar den här mallen toowork med dina egna data. Om det här är din första exponering toohello tjänsten Azure Machine Learning du får en introduktion tooit med hjälp av hello exemplet i [hur toocreate ditt första experiment](machine-learning/machine-learning-create-experiment.md).

hello följande avsnitt innehåller information om hello avsnitt av hello-mall som kräver ändringar när en ny datamängd införs.

### <a name="azure-event-hub"></a>Azure Event Hub
Hej [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) tjänsten är mycket allmänna, så att data kan läggas upp toohello hubb i CSV- eller JSON-format. Ingen särskild bearbetning sker i hello Azure Event Hub, men det är viktigt att du förstår hello data som matas in.

Det här dokumentet beskriver inte hur tooingest dina data, men en enkelt skicka händelser eller data tooan Azure Event Hub med hello [Event Hub API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hej [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) tjänsten är används tooprovide nära analys i realtid genom att läsa från dataströmmar och mata ut data tooany antal källor.

Hello begäran prognoser för energi Lösningsmall består hello Azure Stream Analytics-fråga av två underfrågor varje förbrukar händelser från hello Azure Event Hub-tjänsten som indata och utdata tootwo olika platser med. Dessa utdata består av en Power BI datauppsättning och en Azure-lagringsplats.

Hej [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) fråga kan hittas av:

* Logga in på hello [Azure-hanteringsportalen](https://manage.windowsazure.com/)
* Hitta hello stream analytics-jobb ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) som genererades när hello lösningen har distribuerats. En är för push-överföring tooblob datalagring (t.ex. mytest1streaming432822asablob) och hello andra en är push-överföring av data tooPower BI (t.ex. mytest1streaming432822asapbi).
* Att välja

  * ***INDATA*** tooview hello frågan indata
  * ***FRÅGAN*** tooview hello själva frågan
  * ***Matar ut*** tooview hello olika utdata

Information om Azure Stream Analytics query konstruktionen kan hittas i hello [referens för Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx) på MSDN.

Hello Azure Stream Analytics-jobb som matar ut datamängd med nästan realtidsanalys information om hello inkommande data dataströmmen tooa Power BI-instrumentpanel som har angetts som en del av den här lösningen mallen i den här lösningen. Eftersom implicit kunskap om hello format för inkommande data behöver de här frågorna toobe ändras baserat på dataformat.

hello andra Azure Stream Analytics-jobbet matar ut alla [Händelsehubb](https://azure.microsoft.com/services/event-hubs/) händelser till [Azure Storage](https://azure.microsoft.com/services/storage/) och därför kräver inga ändringar oavsett format för dina data som strömmas hello fullständig händelseinformation toostorage.

### <a name="azure-data-factory"></a>Azure Data Factory
Hej [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service samordnar hello rörelser och bearbetning av data. I hello begäran prognoser för energi Lösningsmall hello data factory består av tolv [pipelines](data-factory/data-factory-create-pipelines.md) att flytta och bearbeta hello data med olika teknologier.

  Du kan komma åt din data factory genom att öppna hello Data Factory nod längst hello hello lösning mallen diagram som skapats med hello distribution av hello lösning. Detta tar du toohello data factory på Azure-hanteringsportalen. Om du ser fel under dina datauppsättningar kan du ignorera de som de är på grund av toodata fabriken som distribueras innan hello datagenerator startades. Dessa fel förhindrar inte din data factory från att fungera.

Det här avsnittet beskrivs nödvändiga hello [pipelines](data-factory/data-factory-create-pipelines.md) och [aktiviteter](data-factory/data-factory-create-pipelines.md) i hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Nedan visas hello diagramvyn hello lösning.

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Fem hello pipelines i den här fabriken innehåller [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript som används toopartition och sammanställd hello data. När anges hello skript kommer att finnas i hello [Azure Storage](https://azure.microsoft.com/services/storage/) konto som skapades under installationen. Deras plats: demandforecasting\\\\skriptet\\\\hive\\ \\ (eller https://[Your lösning name].blob.core.windows.net/demandforecasting).

Liknande toohello [Azure Stream Analytics](#azure-stream-analytics-1) frågor, den [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript har implicit kunskap om hello format för inkommande data, måste de här frågorna toobe ändras baserat på dina dataformat och [egenskapsval](machine-learning/machine-learning-feature-selection-and-engineering.md) krav.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Detta [pipeline](data-factory/data-factory-create-pipelines.md) pipelinen innehåller en enskild aktivitet – en [HDInsightHive](data-factory/data-factory-hive-activity.md) med en [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) som kör en [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx)skript tooaggregate hello var 10: e sekund strömmas i begäran data i omformaren nivå toohourly regionsnivån och placera i [Azure Storage](https://azure.microsoft.com/services/storage/) via hello Azure Stream Analytics-jobbet.

Den [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript för uppgiften partitionering är ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Detta [pipeline](data-factory/data-factory-create-pipelines.md) innehåller två aktiviteter:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) med en [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) som kör en Hive skript tooaggregate hello timvis begäran historikdata i omformaren nivå toohourly regionsnivån och placera i Azure Storage under hello Azure Stream Analytics-jobbet
* [Kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello aggregerade data från Azure Storage blob toohello Azure SQL-databas som har etablerats som en del av hello lösning mallen installation.

Hej [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript för den här uppgiften är ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Dessa [pipelines](data-factory/data-factory-create-pipelines.md) innehåller flera aktiviteter och vars slutresultatet hello beräknas förutsägelser från hello Azure Machine Learning-experiment som är associerade med den här lösningen mallen. De är nästan identisk förutom dem endast hanterar hello annan region som görs av olika RegionID som skickades i hello ADF pipeline och hello hive-skript för varje region.  
hello aktiviteter i det här är:

* [HDInsightHive](data-factory/data-factory-hive-activity.md) med en [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) som kör en Hive skript tooperform aggregeringar och egenskapsval som krävs för hello Azure Machine Learning-experiment. hello Hive-skript för den här uppgiften är respektive ***PrepareMLInputRegionX.hql***.
* [Kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello resultat från hello [HDInsightHive](data-factory/data-factory-hive-activity.md) aktiviteten tooa enda Azure Storage blob som kan vara åtkomst av hello [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivitet.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivitet som anropar hello Azure Machine Learning-experiment som resulterar i hello resultat som placeras i en enda Azure Storage blob.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Dessa [pipelines](data-factory/data-factory-create-pipelines.md) innehåller en enskild aktivitet – en [kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello resultaten av hello Azure Machine Learning-experiment från hello respektive ***MLScoringRegionXPipeline *** toohello Azure SQL-databas som har etablerats som en del av hello lösning mallen installation.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Detta [pipelines](data-factory/data-factory-create-pipelines.md) innehåller en enskild aktivitet – en [kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello samman pågående begäran data från ***LoadHistoryDemandDataPipeline*** toohello Azure SQL-databas som har etablerats som en del av hello lösning mallen installation.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline CopySubstationDataPipeline, CopyTopologyDataPipeline*
Dessa [pipelines](data-factory/data-factory-create-pipelines.md) innehåller en enskild aktivitet – en [kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello referensdata för Topologygeo-Region/understation som har överförts tooAzure Storage blob som en del av hello lösning mallen installation toohello Azure SQL-databas som har etablerats som en del av hello lösning mallen installation.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hej [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentera används för den här lösningen mallen innehåller hello förutsägelser av begäran av region. hello experiment är särskilda toohello datamängden som förbrukats och därför kräver ändras eller ersättas specifika toohello data som tas i.

## <a name="monitor-progress"></a>**Övervaka förloppet**
När hello Datagenerator har startats hello pipeline börjar tooget ur och hello olika komponenterna i din lösning starta lanseras i åtgärden följande hello-kommandon som utfärdats av hello Data Factory. Det finns två sätt som du kan övervaka hello pipeline.

1. Kontrollera hello data från Azure Blob Storage.

    En av hello Stream Analytics-jobbet skriver hello rådata inkommande tooblob datalagring. Om du klickar på **Azure Blob Storage** komponent i din lösning från hello skärmen har distribuerats hello lösningen och klicka sedan på **öppna** i hello högra panelen, tar du toohello [Azure-hanteringsportalen](https://portal.azure.com). När det, klickar du på på **Blobbar**. Nästa hello på panelen visas en lista över behållare. Klicka på **”energysadata”**. Nästa hello på panelen visas hello **”demandongoing”** mapp. I hello \data mapp visas en mapp med namn som datum = 2016-01-28 osv. Om du ser dessa mappar, betyder det att hello rådata är korrekt som genereras på datorn och lagras i blob storage. Du bör se filer som ska ha begränsad storlek i MB i mapparna.
2. Kontrollera hello data från Azure SQL Database.

    hello sista steget i hello pipeline är toowrite data (t.ex. förutsägelser från maskininlärning) till SQL-databas. Du kanske toowait högst 2 timmar för hello data tooappear i SQL-databas. Enkelriktade toomonitor hur mycket data är tillgängliga i SQL-databasen är via [Azure-hanteringsportalen](https://manage.windowsazure.com/). Leta reda på SQL-databaser på hello vänstra panelen![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) och klicka på den. Leta upp din databas (d.v.s. demo123456db) och klicka på den sedan. På nästa sida du hello under **”Anslut tooyour databas”** klickar du på **”kör Transact-SQL-frågor mot din SQL-databas”**.

    Här kan du klicka på ny fråga och fråga för hello antal rader (t.ex. ”Välj count(*) från DemandRealHourly)” län, hello antalet rader i tabellen hello ska öka.)
3. Kontrollera hello data från Power BI-instrumentpanelen.

    Du kan ställa in Power BI varm sökväg instrumentpanelen toomonitor hello inkommande rådata. Följ hello instruktion hello ”Power BI-instrumentpanel” avsnittet.

## <a name="power-bi-dashboard"></a>**Power BI-instrumentpanel**
### <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur tooset in Power BI dashboard toovisualize dina realtidsdata från Azure strömma analytics (varm sökväg), som också som prognosen resultat från Azure machine learning (kalla sökväg).

### <a name="setup-hot-path-dashboard"></a>Installationsprogrammet varm sökväg instrumentpanelen
hello följande steg vägleder dig hur toovisualize realtidsdata utdata från Stream Analytics jobb som genererades under hello tidpunkten för lösningsdistribution av. En [Power BI online](http://www.powerbi.com/) konto är obligatoriskt tooperform hello följande steg. Om du inte har ett konto kan du [skapar du en](https://powerbi.microsoft.com/pricing).

1. Lägga till Power BI-utdata i Azure Stream Analytics (ASA).

   * Du behöver toofollow hello instruktionerna i [Azure Stream Analytics & Power BI: en analys i realtid instrumentpanel för realtid synligheten för strömmande data](stream-analytics/stream-analytics-power-bi-dashboard.md) tooset in hello utdata för din Azure Stream Analytics-jobb som din Power BI-instrumentpanelen.
   * Leta upp hello stream analytics-jobbet i din [Azure-hanteringsportalen](https://manage.windowsazure.com). hello namnet på hello jobb bör vara: YourSolutionName + ”streamingjob” + slumpmässiga nummer + ”asapbi” (d.v.s. demostreamingjob123456asapbi).
   * Lägga till en PowerBI-utdata för hello ASA jobbet. Ange hello **Utdataalias** som **'PBIoutput'**. Ange din **Dataset-namnet** och **tabellnamn** som **'EnergyStreamData'**. När du har lagt till hello utdata, klickar du på **”Start”** längst hello hello sidan toostart hello Stream Analytics-jobbet. Du bör få ett bekräftelsemeddelande (*t.ex.*, ”Start stream analytics-jobbet myteststreamingjob12345asablob lyckades”).
2. Logga in för[Power BI online](http://www.powerbi.com)

   * På vänster panel datauppsättningar sektion i Min arbetsyta hello, bör du kunna toosee en ny datamängd som visar på hello vänstra panelen i Power BI. Detta är hello strömmande data du flyttas från Azure Stream Analytics i hello föregående steg.
   * Se till att hello ***visualiseringar*** fönstret är öppet och visas på höger sida av hello-skärmen.
3. Skapa hello ”begäran av tidsstämpel” panelen:

   * Klicka på dataset **'EnergyStreamData'** på hello vänster panel datauppsättningar.
   * Klicka på **”linjediagram”** ikonen ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Klicka på 'EnergyStreamData' i **fält** panelen.
   * Klicka på **”tidsstämpel”** och kontrollera att den visar under ”axel”. Klicka på **”belastningen”** och se till att det visas under ”värden”.
   * Klicka på **spara** hello upp och namn på rapporten hello som ”EnergyStreamDataReport”. hello rapport med namnet ”EnergyStreamDataReport” visas i rapporter i hello Navigator rutan vänster.
   * Klicka på **”PIN-kod Visual”** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) ikon i övre högra hörnet av den här linjediagram, ett ”PIN-kod tooDashboard” fönster kan visas för du toochoose en instrumentpanel. Välj ”EnergyStreamDataReport” och klicka på ”Pin”.
   * Hovra hello muspekaren över den här panelen på hello instrumentpanelen klickar du på ”Redigera”-ikonen i övre högra hörnet toochange rubrik som ”begäran av tidsstämpel”
4. Skapa andra instrumentpaneler baserat på lämplig datauppsättningar. hello slutliga instrumentpanelsvy visas nedan.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Installationsprogrammet Cold sökväg instrumentpanelen
I cold sökväg data pipeline är hello väsentliga målet tooget hello begäran vid en prognos av varje region. Powerbi ansluter tooan Azure SQL-databas som datakälla, där hello förutsägelse resultat lagras.

> [!NOTE]
> 1) Det tar några timmar toocollect tillräckligt prognosen resultat för hello instrumentpanelen. Vi rekommenderar att du startar processen 2 – 3 timmar efter att du lunch hello datagenerator. 2) i det här steget hello förutsättning är toodownload och installera hello kostnadsfri programvara [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Hämta hello Databasautentiseringsuppgifter.

   Du behöver **databasen servernamn, databasnamn, användarnamn och lösenord** innan du flyttar toonext steg. Här följer hello steg tooguide du hur toofind dem.

   * En gång **”Azure SQL Database”** på din lösningsmall diagram blir grön, klickar du på den och klicka sedan på **”öppen”**. Kommer du att den interaktiva tooAzure hanteringsportalen och databasen informationssidan öppnas också.
   * Du kan hitta ett avsnitt ”databas” hello på sidan. Den visar ut hello-databasen som du har skapat. hello namnet på din databas bör vara **”din lösningsnamn slumpmässiga nummer + 'db'”** (t.ex. ”mytest12345db”).
   * Klicka på din databas i hello nya pop ut Kontrollpanelen, du kan hitta din Databasservernamnet hello längst upp. Databasservernamn bör vara **”din lösningsnamn slumpmässiga nummer + 'database.windows.net,1433'”** (t.ex. ”mytest12345.database.windows.net,1433”).
   * Databasen **användarnamn** och **lösenord** hello samma som hello användarnamn och lösenord registreras tidigare under distributionen av hello lösning.
2. Uppdatera hello datakällan hello cold sökväg Power BI-fil

   * Kontrollera att du har installerat hello senaste versionen av [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * I hello **”DemandForecastingDataGeneratorv1.0”** mapp som du har hämtat, dubbelklicka på hello **'Power BI Template\DemandForecastPowerBI.pbix'** fil. hello inledande grafik baserad på falska data. **Obs:** om du ser felet massage, kontrollera att du har installerat hello senaste versionen av Power BI Desktop.

     När du öppnar den hello längst upp i hello-fil klickar du på **redigera frågor**. Hello ut fönster, dubbelklicka på **'Source'** på hello högra panelen.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Ersätt i hello ut fönster, **”Server”** och **”databas”** med dina egna och databasnamn, och klicka sedan på **”OK”**. Kontrollera att du anger hello-port 1433 för servernamn (**YourSolutionName.database.windows.net 1433**). Ignorera hello varningsmeddelanden som visas på hello-skärmen.
   * I hello nästa pop ut fönster, ser du två alternativ hello vänster (**Windows** och **databasen**). Klicka på **”databas”**, Fyll i din **”användarnamn”** och **”Password”** (detta är hello användarnamn och lösenord som du angav när du distribuerade hello lösning och skapat en Azure SQL database). I ***Välj vilken nivå tooapply inställningarna ska***, kontrollera nivån databasalternativet. Klicka på **”Anslut”**.
   * Stäng hello-fönstret när du är interaktiv tillbaka toohello föregående sida. Ett meddelande visas out - Klicka **tillämpa**. Klicka slutligen på hello **spara** knappen toosave hello ändras. Power BI-fil har nu upprätta anslutningen toohello server. Om din visualiseringar är tomma, kontrollera att du avmarkerar hello val på hello visualiseringar toovisualize alla hello data genom att klicka på hello Radera ikon på hello övre högra hörnet av hello förklaringar. Använd hello uppdatera knappen tooreflect nya data på hello visualiseringar. Först visas bara hello fördefiniera data på din visualiseringar hello data factory är schemalagda toorefresh var 3: e timme. Efter 3 timmar visas nya förutsägelser som visas i din visualiseringar när du uppdaterar hello data.
3. (Valfritt) Publicera hello cold sökväg instrumentpanelen för[Power BI online](http://www.powerbi.com/). Observera att det här steget behöver ett Power BI-konto (eller Office 365-konto).

   * Klicka på **”publicera”** senare några sekunder visas ett fönster med ”publicera tooPower BI lyckades”! med en grön bock. Klicka på hello länkarna ”öppna demoprediction.pbix i Powerbi”. toofind detaljerade instruktioner, se [publicera från Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * toocreate en ny instrumentpanel: Klicka på hello  **+**  logga nästa toothe **instrumentpaneler** avsnittet hello vänster. Ange hello namn ”begäran prognoser Demo” för den här nya instrumentpanelen.
   * När du öppnar hello rapport, klickar du på ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) toopin alla visualiseringar tooyour infopanelen. toofind detaljerade instruktioner, se [fästa en panel tooa Power BI-instrumentpanel från en rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Gå toohello instrumentpanelens sida och justera hello storlek och plats för din visualiseringar och redigera sin titlar. toofind detaljerade instruktioner om hur tooedit dina paneler Se [redigera en sida vid sida - storlek, flytta, Byt namn, PIN-kod, ta bort, lägga till hyperlänk](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Här är ett exempel instrumentpanel med vissa cold sökväg visualiseringar Fäst tooit.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Valfritt) Schemalägga en uppdatering av hello-datakälla.

   * tooschedule uppdatering av hello data håller muspekaren över hello **EnergyBPI-slutlig** dataset, klickar du på ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) och välj sedan **Uppdatera schema**.
     **Obs:** om du ser en varning massage, klickar du på **redigera autentiseringsuppgifter** och se till att dina autentiseringsuppgifter på databasen är hello samma som de som beskrivs i steg 1.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Expandera hello **Uppdatera schema** avsnitt. Aktivera ”hålla data uppdaterade”.
   * Hello schemauppdatering baserat på dina behov. toofind mer information finns i [uppdatering av Data i Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-toodelete-your-solution"></a>**Hur toodelete din lösning**
Se till att stoppa hello datagenerator när du använder inte aktivt hello lösning som kör hello datagenerator orsakar högre kostnader. Ta bort hello lösning om du inte använder den. Din lösning bort tas alla hello-komponenter som etablerats i din prenumeration när du distribuerade hello lösning. toodelete hello lösningen klickar du på din lösningsnamn hello vänstra panelen i hello lösningsmall och klicka på Ta bort.

## <a name="cost-estimation-tools"></a>**Kostnad uppskattning verktyg**
hello är följande två verktyg tillgängliga toohelp bättre förstå totala kostnaderna för körs hello begäran prognoser för energi Lösningsmall i din prenumeration:

* [Microsoft Azure kostnaden exteriörbedömning-verktyget (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure kostnaden exteriörbedömning Tool (stationär dator)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Bekräftelser**
Den här artikeln har skrivits av data forskare Yijing Chen och programvara tekniker Qiu Min hos Microsoft.
