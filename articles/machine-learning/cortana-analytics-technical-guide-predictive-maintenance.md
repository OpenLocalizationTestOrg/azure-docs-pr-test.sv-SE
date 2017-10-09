---
title: "aaaPredictive Underhåll investerar med Azure - Cortana Intelligence tekniska lösningsguide | Microsoft Docs"
description: "En teknisk guide toohello Lösningsmall med Microsoft Cortana Intelligence för förebyggande underhåll i aerospace, verktyg och transport."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: 30ddc1c101007546ae1b303bccebae3ecdacb442
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="technical-guide-toohello-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Tekniska guide toohello Cortana Intelligence Lösningsmall för förebyggande underhåll i aerospace och andra företag

## <a name="important"></a>**Viktigt**
Den här artikeln har ersatts. hello information är fortfarande relevanta toohello problemet, d.v.s. förutsägande Underhåll investerar, men hello senaste artikel med hello mest in toodate information hittar du [här](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**Bekräftelser**
Den här artikeln har skrivits av datavetare Yan Zhang Gauher Shaheen, Fidan Boylu Uz och programvara tekniker Dan Grecoe på Microsoft.

## <a name="overview"></a>**Översikt**
Lösningsmallar är utformade tooaccelerate hello processen för att skapa en E2E demo ovanpå Cortana Intelligence Suite. En mall för distribuerade ska etablera din prenumeration med nödvändiga komponenter i Cortana Intelligence och bygga hello relationer mellan dem. Det lägger också hello data pipeline med exempeldata som genereras från ett generator program som du kan hämta och installera på den lokala datorn när du har distribuerat hello lösningsmall. hello-data som genereras från hello generator hydrate hello data pipeline och börja generera maskininlärning förutsägelser som kan sedan visualiserade på hello Power BI-instrumentpanelen. hello distributionsprocessen vägleder dig igenom flera steg tooset in autentiseringsuppgifterna lösning. Kontrollera att du registrera dessa autentiseringsuppgifter, till exempel lösningens namn, användarnamn och lösenord som du anger under distributionen av hello.  

hello syftet med det här dokumentet är tooexplain hello-Referensarkitektur och olika komponenter som har etablerats i din prenumeration som en del av den här lösningen mallen. hello dokument också talar om hur tooreplace exempeldata med verkliga data toobe kan toosee insikter och förutsägelser från dina egna data. Dessutom beskriver hello dokumentet delarna av hello Lösningsmall som behöver toobe ändras om du vill använda toocustomize hello lösning med dina egna data. Anvisningar om hur toobuild hello Power BI-instrumentpanel för den här lösningen mallen tillhandahålls hello slutet.

> [!TIP]
> Du kan hämta och skriva ut en [PDF-version av det här dokumentet](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Helhetsbild**
![Arkitektur för förutsägande Underhåll](media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

När hello-lösningen distribueras olika Azure-tjänster i Cortana Analytics Suite aktiveras (*dvs.* Event Hub, Stream Analytics, HDInsight, Data Factory Maskininlärning *etc.*). hello arkitekturdiagrammet ovan visas på en hög nivå hur hello förutsägande Underhåll för flyg Lösningsmall är uppbyggd från slutpunkt till slutpunkt. Du kommer att kunna tooinvestigate tjänsterna i hello azure portal genom att klicka på dem på hello lösning mallen diagram som skapats med hello distribution av hello lösning med hello undantag av HDInsight eftersom den här tjänsten har etablerats på begäran när hello relaterade Pipeline-aktiviteter är nödvändiga toorun och bort efteråt.
Du kan hämta en [i full storlek version av hello diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

hello följande avsnitt beskrivs varje del.

## <a name="data-source-and-ingestion"></a>**Datakällan och införandet**
### <a name="synthetic-data-source"></a>Syntetiska datakälla
För den här mallen hello data genereras källa används från ett program som du kan hämta och kör lokalt efter slutförd distribution. Du hittar hello instruktioner toodownload och installera det här programmet i fält för hello egenskaper när du väljer hello första nod kallas förutsägande Underhåll Datagenerator på hello lösning mallen diagram. Det här programmet feeds hello [Azure Event Hub](#azure-event-hub) med datapunkter, eller händelser som ska användas i hello resten av hello lösning flödet. Den här datakällan består av eller härledd från offentligt tillgängliga data från den [NASA data databasen](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) med hello [turbofläktmotorer endast motorn försämring simuleringen datauppsättning](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

hello händelse generation program kommer att fylla i hello Azure Event Hub endast när det körs på datorn.

### <a name="azure-event-hub"></a>Azure händelsehubb
Hej [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) service är hello mottagaren av hello indata hello syntetiska datakälla som beskrivs ovan.

## <a name="data-preparation-and-analysis"></a>**Förberedelse av data och analys**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Hej [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) tjänsten är används tooprovide nära analys i realtid på hello Indataströmmen från hello [Azure Event Hub](#azure-event-hub) tjänst och publicera resultatet till en [Power BI](https://powerbi.microsoft.com) instrumentpanelen som arkiverar alla rådata inkommande händelser toohello [Azure Storage](https://azure.microsoft.com/services/storage/) tjänsten för senare bearbetning av hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service.

### <a name="hd-insights-custom-aggregation"></a>HD insikter anpassade aggregering
hello Azure HD Insight service är används toorun [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript (styrd av Azure Data Factory) tooprovide aggregeringar på raw hello-händelser som har arkiverats hello Azure Stream Analytics-tjänsten.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hej [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) tjänsten används (styrd av Azure Data Factory) toomake förutsägelser på hello återstående livslängd (RUL) för en viss flygplan motor ges hello indata togs emot.

## <a name="data-publishing"></a>**Publicering av data**
### <a name="azure-sql-database-service"></a>Azure SQL Database-tjänsten
Hej [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) tjänsten är används toostore (hanteras av Azure Data Factory) hello förutsägelser som tagits emot av hello Azure Machine Learning-tjänst som ska användas i hello [Power BI](https://powerbi.microsoft.com) instrumentpanelen.

## <a name="data-consumption"></a>**Användning av data**
### <a name="power-bi"></a>Power BI
Hej [Power BI](https://powerbi.microsoft.com) tjänsten är används tooshow en instrumentpanel som innehåller aggregeringar och aviseringar som tillhandahålls av hello [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) tjänsten samt RUL förutsägelser som lagras i [Azure SQL Databasen](https://azure.microsoft.com/services/sql-database/) som skapas med hjälp av hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service. Anvisningar om hur toobuild hello Power BI-instrumentpanel för den här lösningen-mallen finns i toohello nedan.

## <a name="how-toobring-in-your-own-data"></a>**Hur toobring i dina egna data**
Det här avsnittet beskrivs hur toobring tooAzure egna data och vilka områden kräver ändringar för hello data du sätta i den här arkitekturen.

Det är inte troligt att alla dataset som du kan aktivera matchar hello dataset som används av hello [turbofläktmotorer endast motorn försämring simuleringen datauppsättning](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) används för den här lösningen mallen. Förstå dina data och kraven kommer att vara avgörande i hur du ändrar den här mallen toowork med dina egna data. Om det här är din första exponering toohello tjänsten Azure Machine Learning du får en introduktion tooit med hjälp av hello exemplet i [hur toocreate ditt första experiment](machine-learning-create-experiment.md).

hello följande avsnitt innehåller information om hello avsnitt av hello-mall som kräver ändringar när en ny datamängd införs.

### <a name="azure-event-hub"></a>Azure Event Hub
hello Azure Event Hub-tjänsten är mycket generisk så att data kan läggas upp toohello hubb i CSV- eller JSON-format. Ingen särskild bearbetning sker i hello Azure Event Hub, men det är viktigt att du förstår hello data som matas in.

Det här dokumentet beskriver inte hur tooingest dina data, men du kan enkelt skicka händelser eller data tooan Azure Event Hub med hello Event Hub API.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
hello Azure Stream Analytics-tjänsten är används tooprovide nära analys i realtid genom att läsa från dataströmmar och mata ut data tooany antal källor.

Hello förutsägande Underhåll för flyg Lösningsmall består Azure Stream Analytics-fråga av fyra underfrågor varje konsumerande händelser från hello Azure Event Hub-tjänsten och utdata behöver fyra olika platser. Dessa utdata består av tre Power BI-datauppsättningar och en Azure-lagringsplats.

hello Azure Stream Analytics-fråga kan hittas av:

* Logga in på hello Azure-portalen
* Hitta hello Stream Analytics-jobb ![Stream Analytics-ikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) som genererades när hello lösningen har distribuerats (*t.ex.*, **maintenancesa02asapbi** och **maintenancesa02asablob** för lösningen förutsägande underhåll)
* Att välja
  
  * ***INDATA*** tooview hello frågan indata
  * ***FRÅGAN*** tooview hello själva frågan
  * ***Matar ut*** tooview hello olika utdata

Information om Azure Stream Analytics query konstruktionen kan hittas i hello [referens för Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx) på MSDN.

I den här lösningen utdata hello frågor tre DataSet med nästan realtidsanalys information om hello inkommande data dataströmmen tooa Power BI-instrumentpanel som har angetts som en del av den här lösningen mallen. Eftersom implicit kunskap om hello format för inkommande data behöver de här frågorna toobe ändras baserat på dataformat.

hello-fråga i hello andra Stream Analytics-jobbet **maintenancesa02asablob** bara matar ut alla [Händelsehubb](https://azure.microsoft.com/services/event-hubs/) händelser till [Azure Storage](https://azure.microsoft.com/services/storage/) och därför kräver inga ändringar oavsett dina dataformat som hello strömmas fullständig händelseinformation toostorage.

### <a name="azure-data-factory"></a>Azure Data Factory
Hej [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service samordnar hello rörelser och bearbetning av data. I det förutsägande Underhåll för flyg Lösningsmall hello data factory består av tre [pipelines](../data-factory/data-factory-create-pipelines.md) att flytta och bearbeta hello data med olika teknologier.  Du kan komma åt din data factory genom att öppna hello hello Data Factory nod längst hello hello lösning mallen diagram som skapats med hello distribution av hello lösning. Detta tar du toohello data factory på Azure-portalen. Om du ser fel under dina datauppsättningar kan du ignorera de som de är på grund av toodata fabriken som distribueras innan hello datagenerator startades. Dessa fel förhindrar inte din data factory från att fungera.

![Data Factory dataset fel](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Det här avsnittet beskrivs nödvändiga hello [pipelines](../data-factory/data-factory-create-pipelines.md) och [aktiviteter](../data-factory/data-factory-create-pipelines.md) i hello [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Nedan visas hello diagramvyn hello lösning.

![Azure Data Factory](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Två av hello pipelines i den här fabriken innehåller [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript som används toopartition och sammanställd hello data. När anges hello skript kommer att finnas i hello [Azure Storage](https://azure.microsoft.com/services/storage/) konto som skapades under installationen. Deras plats: maintenancesascript\\\\skriptet\\\\hive\\ \\ (eller https://[Your lösning name].blob.core.windows.net/maintenancesascript).

Liknande toohello [Azure Stream Analytics](#azure-stream-analytics-1) frågor, den [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript har implicit kunskap om hello format för inkommande data, måste de här frågorna toobe ändras baserat på dina dataformat och [egenskapsval](machine-learning-feature-selection-and-engineering.md) krav.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Detta [pipeline](../data-factory/data-factory-create-pipelines.md) innehåller en enskild aktivitet – en [HDInsightHive](../data-factory/data-factory-hive-activity.md) med en [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) som kör en [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptet toopartition hello data placeras i [Azure Storage](https://azure.microsoft.com/services/storage/) under den [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobb.

Den [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript för uppgiften partitionering är ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Detta [pipeline](../data-factory/data-factory-create-pipelines.md) innehåller flera aktiviteter och vars slutresultatet är hello bedömas förutsägelser från hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment som är associerade med den här lösningen mallen.

hello aktiviteter i det här är:

* [HDInsightHive](../data-factory/data-factory-hive-activity.md) med en [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) som kör en [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript tooperform aggregeringar och egenskapsval som krävs för hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment.
  Den [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript för uppgiften partitionering är ***PrepareMLInput.hql***.
* [Kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello resultaten från den [HDInsightHive](../data-factory/data-factory-hive-activity.md) aktiviteten tooa enda [Azure Storage](https://azure.microsoft.com/services/storage/) blob som kan vara åtkomst av den [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivitet.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivitet som anropar hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment som resulterar i hello resultat som är put i en enda [Azure Storage](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Detta [pipeline](../data-factory/data-factory-create-pipelines.md) innehåller en enskild aktivitet – en [kopiera](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivitet som flyttar hello resultaten av hello [Azure Machine Learning](#azure-machine-learning) experiment från den *** MLScoringPipeline*** toohello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) som har etablerats som en del av installationen mallen.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Hej [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment som används för den här lösningen mallen innehåller hello återstående användbar livslängd (RUL) ett flygplan-motorn. hello experiment är särskilda toohello datamängden som förbrukats och därför kräver ändras eller ersättas specifika toohello data som tas i.

Information om hur hello Azure Machine Learning-experiment skapades finns [förutsägande Underhåll: steg 1 av 3, förberedelse av data och funktionen tekniker](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Övervaka förloppet**
 När hello Datagenerator har startats hello pipeline börjar tooget ur och hello olika komponenterna i din lösning starta lanseras i åtgärden följande hello-kommandon som utfärdats av hello Data Factory. Det finns två sätt som du kan övervaka hello pipeline.

1. En av hello Stream Analytics-jobbet skriver hello rådata inkommande tooblob datalagring. Om du klickar på Blob Storage-komponenten i din lösning från hello-skärmen du har distribuerats hello lösningen och klicka på Öppna i hello högra panelen, tar du toohello [hanteringsportalen](https://portal.azure.com/). När det, klickar du på Blobbar. Nästa hello på panelen visas en lista över behållare. Klicka på **maintenancesadata**. Nästa hello på panelen visas hello **\data** mapp. I hello \data mapp visas en mapp med namn som timme = 17, timme = 18 osv. Om du ser dessa mappar, betyder det att hello rådata är korrekt som genereras på datorn och lagras i blob storage. Du bör se csv-filer som ska ha begränsad storlek i MB i mapparna.
2. hello sista steget i hello pipeline är toowrite data (t.ex. förutsägelser från maskininlärning) till SQL-databas. Du kanske toowait högst tre timmar för hello data tooappear i SQL-databas. Enkelriktade toomonitor hur mycket data är tillgängliga i SQL-databasen är via [azure-portalen](https://manage.windowsazure.com/). Leta upp SQL-databaser på hello vänsterpanelen ![SQL ikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) och klicka på den. Leta upp din databas **pmaintenancedb** och klicka på den. Klicka på Hantera på hello nästa sida längst ned hello
   
    ![Hantera ikon](media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    Här kan du klicka på ny fråga och fråga för hello antal rader (t.ex. väljer count(*) från PMResult). Län, öka hello antalet rader i tabellen hello.

## <a name="power-bi-dashboard"></a>**Power BI-instrumentpanel**
### <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur tooset in Power BI dashboard toovisualize dina realtidsdata från Azure Stream Analytics (varm sökväg) samt batch förutsägelse resultat av Azure machine learning (kalla sökväg).

### <a name="setup-cold-path-dashboard"></a>Installationsprogrammet cold sökväg instrumentpanelen
I hello cold sökväg data pipeline är hello väsentliga målet tooget förutsägande RUL (återstående livslängd) varje flygplan-motorn när den är klar svarta (cykel). hello förutsägelse resultatet uppdateras var 3: e timme för att förutsäga hello flygplan motorer som är klar med en svarta under hello senaste tre timmar.

Powerbi ansluter tooan Azure SQL-databas som datakälla, där förutsägelse resultatet lagras. Obs: 1) om hur du distribuerar lösningen en verklig förutsägelse visas i hello databas inom tre timmar.
Hej pbix-fil som medföljde hello Generator download innehåller vissa seed-data så att du kan skapa hello Power BI-instrumentpanel direkt. 2) i det här steget hello förutsättning är toodownload och installera hello kostnadsfri programvara [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

hello följande steg får du anvisningar om hur tooconnect hello pbix-filen till hello SQL-databas som har de för närvarande hello av lösningsdistribution som innehåller data (*t.ex.*. resultat för förutsägelse) för visualisering.

1. Hämta hello Databasautentiseringsuppgifter.
   
   Du behöver **databasen servernamn, databasnamn, användarnamn och lösenord** innan du flyttar toonext steg. Här följer hello steg tooguide du hur toofind dem.
   
   * En gång **Azure SQL-databas** på din lösningsmall diagram blir grön, klickar du på den och klicka sedan på **'Öppen'**.
   * Ser du ett nytt fliken/webbläsarfönster som visar hello Azure Portalsida. Klicka på **'Resursgrupper'** på hello vänstra panelen.
   * Välj hello-prenumeration som du använder för att distribuera lösningen hello och välj sedan **' YourSolutionName\_ResourceGroup'**.
   * I hello nya pop i Kontrollpanelen klickar du på hello ![SQL ikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) ikonen tooaccess din databas. Databasnamnet är nästa toohello ikonen (*t.ex.*, **'pmaintenancedb'**), och hello **Databasservernamnet** anges under hello Server namnegenskapen och bör vara liknande för**YourSoutionName.database.windows.net**.
   * Databasen **användarnamn** och **lösenord** hello samma som hello användarnamn och lösenord registreras tidigare under distributionen av hello lösning.
2. Uppdatera hello datakällan hello cold sökväg rapportfilen med Power BI Desktop.
   
   * Hello-mappen på datorn där du hämtade och unzipped Generator-filen, dubbelklicka på den **PowerBI\\PredictiveMaintenanceAerospace.pbix** fil. Om du ser alla varningsmeddelanden när du öppnar filen hello ignorera dem. Hello längst upp i hello-fil klickar du på **redigera frågor**.
     
     ![Redigera frågor](media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Du ser två tabeller **RemainingUsefulLife** och **PMResult**. Välj hello första tabellen och klickar på ![frågan inställningsikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) nästa för**'Source'** under **tillämpas steg** på hello rätt **'Frågeinställningar'**panelen. Ignorera alla varningsmeddelanden som visas.
   * Ersätt i hello ut fönster, **'Server'** och **”databas”** med dina egna och databasnamn, och klicka sedan på **'OK'**. Kontrollera att du anger hello-port 1433 för servernamn (**YourSoutionName.database.windows.net 1433**). Lämna hello databasfält som **pmaintenancedb**. Ignorera hello varningsmeddelanden som visas på hello-skärmen.
   * I hello nästa pop ut fönster, ser du två alternativ hello vänster (**Windows** och **databasen**). Klicka på **”databas”**, Fyll i din **'Användarnamn'** och **'Password'** (detta är hello användarnamn och lösenord som du angav när du distribuerade hello lösning och skapat en Azure SQL database). I ***Välj vilken nivå tooapply inställningarna ska***, kontrollera nivån databasalternativet. Klicka på **”Anslut”**.
   * Klicka på hello andra tabellen **PMResult** Klicka ![Navigeringsikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) nästa för**'Source'** under **tillämpas steg** på hello rätt **Inställningar för frågan** panelen, och uppdatera hello server och databasnamn som hello senare steg och klicka på OK.
   * Stäng hello-fönstret när du är interaktiv tillbaka toohello föregående sida. Ett meddelande visas out - Klicka **tillämpa**. Klicka slutligen på hello **spara** knappen toosave hello ändras. Power BI-fil har nu upprätta anslutningen toohello server. Om din visualiseringar är tomma, kontrollera att du avmarkerar hello val på hello visualiseringar toovisualize alla hello data genom att klicka på hello Radera ikon på hello övre högra hörnet av hello förklaringar. Använd hello uppdatera knappen tooreflect nya data på hello visualiseringar. Först visas bara hello fördefiniera data på din visualiseringar hello data factory är schemalagda toorefresh var 3: e timme. Efter 3 timmar visas nya förutsägelser som visas i din visualiseringar när du uppdaterar hello data.
3. (Valfritt) Publicera hello cold sökväg instrumentpanelen för[Power BI online](http://www.powerbi.com/). Observera att det här steget behöver ett Power BI-konto (eller Office 365-konto).
   
   * Klicka på **”publicera”** senare några sekunder visas ett fönster med ”publicera tooPower BI lyckades”! med en grön bock. Klicka på hello länkarna ”öppna PredictiveMaintenanceAerospace.pbix i Power BI”. toofind detaljerade instruktioner, se [publicera från Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * toocreate en ny instrumentpanel: Klicka på hello  **+**  logga nästa toothe **instrumentpaneler** avsnittet hello vänster. Ange hello namn ”förutsägande Underhåll Demo” för den här nya instrumentpanelen.
   * När du öppnar hello rapport, klickar du på ![fästikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) toopin alla visualiseringar tooyour infopanelen. toofind detaljerade instruktioner, se [fästa en panel tooa Power BI-instrumentpanel från en rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Gå toohello instrumentpanelens sida och justera hello storlek och plats för din visualiseringar och redigera sin titlar. toofind detaljerade instruktioner om hur tooedit dina paneler Se [redigera en sida vid sida - storlek, flytta, Byt namn, PIN-kod, ta bort, lägga till hyperlänk](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Här är ett exempel instrumentpanel med vissa cold sökväg visualiseringar Fäst tooit.  Dina nummer på hello visualiseringar kan vara olika beroende på hur länge du kör din datagenerator.
     <br/>
     ![Vyn slutgiltig](media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * tooschedule uppdatering av hello data håller muspekaren över hello **PredictiveMaintenanceAerospace** dataset, klickar du på ![ellips ikonen](media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) och välj sedan **Uppdatera schema**.
     <br/>
     **Obs:** om du ser en varning massage, klickar du på **redigera autentiseringsuppgifter** och se till att dina autentiseringsuppgifter på databasen är hello samma som de som beskrivs i steg 1.
     <br/>
     ![Schemalägga en uppdatering](media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Expandera hello **Uppdatera schema** avsnitt. Aktivera ”hålla data uppdaterade”.
     <br/>
   * Hello schemauppdatering baserat på dina behov. toofind mer information finns i [uppdatering av Data i Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Installationsprogrammet varm sökväg instrumentpanelen
hello följande steg vägleder dig hur toovisualize realtidsdata utdata från Stream Analytics jobb som genererades under hello tidpunkten för lösningsdistribution av. En [Power BI online](http://www.powerbi.com/) konto är obligatoriskt tooperform hello följande steg. Om du inte har ett konto kan du [skapar du en](https://powerbi.microsoft.com/pricing).

1. Lägga till Power BI-utdata i Azure Stream Analytics (ASA).
   
   * Du behöver toofollow hello instruktionerna i [Azure Stream Analytics & Power BI: en analys i realtid instrumentpanel för realtid synligheten för strömmande data](../stream-analytics/stream-analytics-power-bi-dashboard.md) tooset in hello utdata för din Azure Stream Analytics-jobb som din Power BI-instrumentpanelen.
   * hello ASA frågan har tre utdata som **aircraftmonitor**, **aircraftalert**, och **flightsbyhour**. Du kan visa hello frågan genom att klicka på fliken fråga. Motsvarande tooeach i dessa tabeller måste tooadd en tooASA utdata. När du lägger till hello första utdata (*t.ex.* **aircraftmonitor**) se till att hello **Utdataalias**, **Dataset-namnet** och  **Tabellnamn** är hello samma (**aircraftmonitor**). Upprepa hello steg tooadd matar ut för **aircraftalert**, och **flightsbyhour**. När du har lagt till alla tre utdata tabeller och startats hello ASA jobb kan du bör få ett bekräftelsemeddelande (*t.ex.*, ”Start Stream Analytics-jobbet maintenancesa02asapbi lyckades”).
2. Logga in för[Power BI online](http://www.powerbi.com)
   
   * På vänster panel datauppsättningar sektion i Min arbetsyta hello den ***DATASET*** namn **aircraftmonitor**, **aircraftalert**, och **flightsbyhour**ska visas. Detta är hello strömmande data du flyttas från Azure Stream Analytics i hello tidigare step.hello dataset **flightsbyhour** kanske inte visas på hello samma tid som hello andra två datauppsättningar på grund av toohello uppbyggnad hello SQL-frågan bakom den. Men bör det visas efter en timme.
   * Se till att hello ***visualiseringar*** fönstret är öppet och visas på höger sida av hello-skärmen.
3. När du har hello data som flödar till Power BI kan börja du visualisera hello strömmande data. Nedan visas ett exempel instrumentpanel med vissa varm sökväg visualiseringar Fäst tooit. Du kan skapa andra instrumentpaneler baserat på lämplig datauppsättningar. Dina nummer på hello visualiseringar kan vara olika beroende på hur länge du kör din datagenerator.

    ![Instrumentpanelsvy](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Här följer några steg toocreate en hello paneler ovan – hello ”flottan vy av Sensor 11 vs. Tröskelvärdet 48,26 ”panelen:
   
   * Klicka på dataset **aircraftmonitor** på hello vänster panel datauppsättningar.
   * Klicka på hello **linjediagram** ikon.
   * Klicka på **bearbetas** i hello **fält** rutan så att den visar under ”axel” i hello **visualiseringar** fönstret.
   * Klicka på ”s11” och ”s11\_aviseringen” så att de båda visas under ”värden”. Klicka på hello lilla pilen bredvid för**s11** och **s11\_avisering**, ändra ”Sum” för ”medel”.
   * Klicka på **spara** hello upp och namn på rapporten till hello ”aircraftmonitor”. Rapporten med namnet ”aircraftmonitor” visas i hello **rapporter** avsnitt i hello **Navigator** fönstret hello vänster.
   * Klicka på hello **PIN-kod Visual** ikon på hello övre högra hörnet i det här linjediagrammet. Ett ”PIN-kod tooDashboard” fönster kan visas som du kan välja en instrumentpanel. Markera ”förutsägande Underhåll Demo” och klicka på ”Pin”.
   * Håller hello markören över den här panelen på hello instrumentpanelen, klickar du på hello ”edit”-ikon på hello övre högra hörnet toochange rubriken för ”flottan vy av Sensor 11 vs. Tröskelvärde för 48,26 ”och underrubrik för” genomsnittlig över flottan över tid ”.

## <a name="how-toodelete-your-solution"></a>**Hur toodelete din lösning**
Se till att stoppa hello datagenerator när du använder inte aktivt hello lösning som kör hello datagenerator orsakar högre kostnader. Ta bort hello lösning om du inte använder den. Din lösning bort tas alla hello-komponenter som etablerats i din prenumeration när du distribuerade hello lösning. toodelete hello lösningen klickar du på din lösningsnamn hello vänstra panelen i hello lösningsmall och klicka på Ta bort.

## <a name="cost-estimation-tools"></a>**Kostnad uppskattning verktyg**
hello är följande två verktyg tillgängliga toohelp bättre förstå totala kostnaderna för körs hello förutsägande Underhåll för flyg Lösningsmall i din prenumeration:

* [Microsoft Azure kostnaden exteriörbedömning-verktyget (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure kostnaden exteriörbedömning Tool (stationär dator)](http://www.microsoft.com/download/details.aspx?id=43376)

