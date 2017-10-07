---
title: "aaaDeep fördjupa dig i förutsäga vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a>Vehicle telemetri analytics lösning playbook: ingående i hello lösning
Detta **menyn** länkar toohello avsnitt i den här playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Det här avsnittet flyttar ned till de enskilda hello stegen som beskrivs i hello lösningsarkitektur med instruktioner och pekare för anpassning. 

## <a name="data-sources"></a>Datakällor
hello lösningen använder två olika datakällor:

* **simulerade vehicle signaler och diagnostik dataset** och 
* **vehicle katalog**

Vehicle telematik simulator ingår som en del av den här lösningen. Den genererar diagnostisk information och signalerar till motsvarande toohello tillståndet för hello vehicle och toohello körning mönster vid en viss tidpunkt. Klicka på [Vehicle telematik Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematik Simulator lösning i Visual Studio** anpassningar baserat på dina krav. hello vehicle katalogen innehåller en referens datamängd med en VIN toomodel mappning.

![Vehicle telematik simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Bild 1 – Vehicle telematik Simulator*

Det här är en JSON-formaterad datamängd som innehåller hello följer schemat.

| Kolumn | Beskrivning | Värden |
| --- | --- | --- |
| VIN |Slumpmässigt genererat Vehicle ID-nummer |Detta hämtas från en övergripande lista med 10 000 slumpmässigt genererat vehicle ID-nummer. |
| Utanför temperatur |hello utanför temperatur där hello vehicle kör |Slumptal mellan 0-100 |
| Motorn temperatur |hello motorn temperatur hello vehicle |Slumpmässigt genererat tal mellan 0 och 500 |
| Hastighet |hello hastighet på vilka hello vehicle körning |Slumptal mellan 0-100 |
| Bränsle |hello bränslenivå hello fordon |Slumptal mellan 0-100 (anger bränsle nivån procent) |
| EngineOil |hello motorn olja andelen hello vehicle |Slumptal mellan 0-100 (anger motorn olja nivån procent) |
| Däck hög belastning |hello däck trycket hello vehicle |Slumpmässigt tal mellan 0-50 (anger däck trycket nivån procent) |
| Mätarställning |hello Mätarställning hello fordon |Slumpmässigt genererat nummer från 0 200000 |
| Accelerator_pedal_position |hello accelerator cyklar positionen för hello vehicle |Slumptal mellan 0-100 (anger accelerator nivån procent) |
| Parking_brake_status |Anger om hello vehicle parkerade eller inte |True eller False |
| Headlamp_status |Anger om hello strålkastaren är på eller inte |True eller False |
| Brake_pedal_status |Anger om hello bromspedal är nedtryckt eller inte |True eller False |
| Transmission_gear_position |hello överföring växeln positionen för hello vehicle |Tillstånd: första, andra, tredje, fjärde, femte, sjätte, sjunde, åttonde |
| Ignition_status |Anger om hello vehicle är igång eller Stoppad |True eller False |
| Windshield_wiper_status |Anger om hello vindrutan vindrutetorkare är aktiverat eller inte |True eller False |
| ABS |Anger om ABS används eller inte |True eller False |
| tidsstämpel |hello tidsstämpel när hello datapunkt har skapats |Date |
| Ort |hello platsen för hello vehicle |4 orter i den här lösningen: Bellevue, Redmond, Sammamish, Seattle |

referensdatauppsättningen för hello vehicle modellen innehåller VIN toohello Modellmappning. 

| VIN | Modellen |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Hybrid |
| WORG68Z2PLTNZDBI7 |Family sedan |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Hybrid |
| MHTP9N792PHK08WJM |Family sedan |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Hybrid |
| W9NSZ423XZHAONYXB |Family sedan |
| 26WJSGHX4MA5ROHNL |Kan konverteras |
| GHLUB6ONKMOSI7E77 |Station vagn |
| 9C2RHVRVLMEJDBXLP |Bil |
| BRNHVMZOUJ6EOCP32 |Liten Stadsjeep |
| VCYVW0WUZNBTM594J |Sport bil |
| HNVCE6YFZSA5M82NY |Medelhög Stadsjeep |
| 4R30FOR7NUOBL05GJ |Station vagn |
| WYNIIY42VKV6OQS1J |Stora Stadsjeep |
| 8Y5QKG27QET1RBK7I |Stora Stadsjeep |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Hybrid |
| VHRA1Y2TGTA84F00H |Family sedan |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Hybrid |
| T8DNDN5UDCWL7M72H |Family sedan |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Hybrid |
| XUF99EW9OIQOMV7Q7 |Family sedan |
| 8OMCL3LGI7XNCC21U |Kan konverteras |
| ……. | |

### <a name="references"></a>Referenser
[Vehicle telematik Simulator Visual Studio-lösning](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure Event Hub](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Införandet
Kombinationer av Händelsehubbar i Azure Stream Analytics och Data Factory är balanserad tooingest hello vehicle signaler, hello diagnostiska händelser, och realtid och batch-analytics. Alla dessa komponenter skapas och konfigureras som en del av hello lösningsdistribution. 

### <a name="real-time-analysis"></a>Analys i realtid
hello händelser som genererats av hello Vehicle telematik Simulator publiceras toohello Event Hub med hello Event Hub SDK. hello Stream Analytics-jobbet en dessa händelser från hello Event Hub och processer hello data i realtid tooanalyze hello vehicle hälsa. 

![Event hub instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Bild 4 - Event Hub-instrumentpanelen*

![Strömma Analytics-jobbet databearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Bild 5 - Stream Analytics-jobbet bearbetning av data*

hello Stream Analytics-jobb.

* en data från hello Event Hub 
* Utför en koppling med hello referens toomap hello vehicle VIN toohello motsvarande datamodell 
* sparar dem i Azure blob-lagring för omfattande batch analytics. 

hello följande Stream Analytics-fråga är används toopersist hello data till Azure-blobblagring. 

![Stream Analytics-jobbet frågan för datapåfyllning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Bild 6 - Stream Analytics-jobbet frågan för datapåfyllning*

### <a name="batch-analysis"></a>Batchanalys
Vi också genererar en ytterligare volym av simulerade vehicle signaler och diagnostik datamängden för bättre batch analytics. Detta är obligatorisk tooensure en bra representativt datavolym för batch-bearbetning. För detta ändamål använder du en pipeline med namnet ”PrepareSampleDataPipeline” i hello Azure Data Factory arbetsflöde toogenerate ett år kan du se simulerade vehicle signaler och diagnostik dataset. Klicka på [Data Factory anpassad aktivitet](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello Data Factory anpassad DotNet aktivitet Visual Studio-lösning för anpassningar baserat på dina krav. 

![Förbereda exempeldata för batchbearbetning arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Figur 7 – förbereda exempeldata för arbetsflöde för batch-bearbetning*

hello försäljningsförlopp består av en anpassad ADF .net aktivitet, visas här:

![PrepareSampleDataPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Figur 8 - PrepareSampleDataPipeline*

När hello pipelinen körs korrekt och ”RawCarEventsTable” dataset markeras ”klar” års värt simulerade vehicle signaler och diagnostiska som data produceras. Du ser hello följande mapp och fil som skapats i ditt lagringskonto i hello ”connectedcar”-behållaren:

![PrepareSampleDataPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Bild 9 - PrepareSampleDataPipeline utdata*

### <a name="references"></a>Referenser
[Azure Event Hub-SDK för inhämtning av dataströmmar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure Data Factory data movement funktioner](../data-factory/data-factory-data-movement-activities.md)
[DotNet-aktivitet för Azure Data Factory](../data-factory/data-factory-use-custom-activities.md)

[Azure Data Factory DotNet aktivitet visual studio-lösning för att förbereda exempeldata](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a>Partitionen hello dataset
hello rådata halvstrukturerade vehicle signaler och diagnostik dataset partitioneras i hello data förberedelsen till formatet år/månad. Den här partitionering befordrar mer effektiva frågor och skalbar långsiktig lagring genom att aktivera fel-over från en blob konto toohello bredvid hello första kontot fylls. 

>[!NOTE] 
>Det här steget i hello lösningen är tillämpliga endast toobatch bearbetning.

Indata och utdata hantering av data:

* Hej **utdata** (märkta *PartitionedCarEventsTable*) toobe lagras under en längre tidsperiod som hello grundläggande / ”rawest” form av data i hello kundens ”Data Lake”. 
* Hej **indata** toothis pipeline skulle normalt ignoreras eftersom hello utdata har fullständig återgivning toohello indata - lagras bara (partitionerad) bättre för senare användning.

![Partitionen bil händelser arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Figur 10 – Partition bil händelser arbetsflöde*

hello rådata är partitionerad med hjälp av en HDInsight Hive-aktivitet i ”PartitionCarEventsPipeline”. hello exempeldata genereras i steg 1 för ett år har partitionerats med år/månad. hello partitioner är används toogenerate vehicle signaler och diagnostikdata för varje månad (totalt 12 partitioner) för ett år. 

![PartitionCarEventsPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Figur 11 - PartitionCarEventsPipeline*

***PartitionConnectedCarEvents Hive-skript***

hello följande Hive-skript med namnet ”partitioncarevents.hql” används för partitionering och finns i hello ”\demo\src\connectedcar\scripts” mapp för hello hämtade zip. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.

![Partitionerade utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Figur 12 - partitionerade utdata*

hello data optimeras nu, hantera och redo för vidare bearbetning toogain omfattande batch insikter. 

## <a name="data-analysis"></a>Dataanalys
I det här avsnittet visas hur toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory och Azure HDInsight för omfattande avancerade analyser på vehicle hälsa och köra vanor. Det finns tre underavsnitt här:

1. **Maskininlärning**: detta avsnitt innehåller information om hello avvikelseidentifiering identifiering experiment som används i den här lösningen toopredict fordon kräver Underhåll underhåll och fordon som kräver återställning av på grund av problem med toosafety.
2. **Realtid analysis**: detta avsnitt innehåller information om hello analys i realtid med hello Stream Analytics-frågespråket och operationalizing hello maskininlärning experiment i realtid med ett anpassat program.
3. **Batch-analys**: detta avsnitt innehåller information om hello omvandla och bearbetning av hello batch data med Azure HDInsight och Azure Machine Learning operationalized av Azure Data Factory.

### <a name="machine-learning"></a>Machine Learning
Vårt mål här är toopredict hello fordon som kräver underhåll eller återkallas baserat på vissa hed statistik. Vi göra hello följande antaganden

* Om en av hello följande tre villkor är uppfyllda, hello fordon kräver **servicing Underhåll**:
  
  * Däck trycket är låg
  * Motorn olja nivån är låg
  * Motorn är hög
* Om en av hello följande villkor är uppfyllda, hello fordon kan ha en **säkerhet problemet** och kräver **återkallning**:
  
  * Motorn temperatur är hög men utanför temperatur är låg
  * Motorn temperatur är låg men utanför temperatur är hög

Baserat på hello tidigare krav har vi skapat två separata modeller toodetect avvikelser, en för identifiering av vehicle underhåll och en för vehicle återkallning identifiering. I båda dessa modeller används hello inbyggda huvudnamn komponenten analys (PCA) algoritm för avvikelseidentifiering. 

**Underhåll identifiering av modellen**

Om en av tre indikatorer däck trycket, motorolja eller motorn temperatur - uppfyller sitt respektive tillstånd, rapporterar avvikelser hello Underhåll identifiering av modellen. Därför kan behöver vi bara tooconsider dessa tre variabler i att skapa hello modellen. I vårt experiment i Azure Machine Learning vi först använda en **Välj kolumner i datauppsättning** modulen tooextract dessa tre variabler. Vi använder bredvid hello PCA-baserad avvikelseidentifiering identifiering modulen toobuild hello avvikelseidentifiering identifiering modell. 

Huvudnamn komponenten analys (PCA) är en etablerad teknik i machine learning som kan vara tillämpade toofeature markeringen, klassificering och avvikelseidentifiering identifiering. PCA konverterar en uppsättning fallet med eventuellt korrelerade variabler i en uppsättning värden som kallas huvudkomponenter. hello viktiga uppfattning om PCA-baserad modellering är tooproject data till ett lägre endimensionell utrymme så att funktioner och avvikelser mer lätt kan identifieras.

För varje ny indata hello för identifiering av modellen, hello avvikelseidentifiering detektor först beräknar dess projektion på hello eigenvectors och sedan beräknar hello normaliserade återuppbyggnad fel. Felet normaliserade är hello avvikelseidentifiering poäng. hello högre hello fel hello mer avvikande hello-instansen är. 

Hello Underhåll identifiering problem kan varje post ses som en punkt i 3-dimensionell kan definieras av däck tryck, motorolja och motorn temperatur koordinater. toocapture dessa avvikelser kan vi projicera hello ursprungliga data i hello 3-dimensionell utrymme på 2-dimensionell kan använda PCA. Därför som vi hello parametern antal komponenter toouse i PCA toobe 2. Den här parametern spelar en viktig roll vid tillämpning av PCA-baserad avvikelseidentifiering. Efter projicera data med hjälp av PCA, kan vi identifiera dessa avvikelser enklare.

**Återkalla avvikelseidentifiering identifiering av modellen** i hello återkallning avvikelseidentifiering identifiering av modellen vi använda hello Välj kolumner i datauppsättning och PCA-baserad avvikelseidentifiering identifiering moduler på ett liknande sätt. Mer specifikt vi först extrahera tre variabler - motorn temperatur, utanför temperatur och hastighet - med hello **Välj kolumner i datauppsättning** modul. Vi också innehålla hello hastighet variabeln eftersom hello motorn temperatur är vanligtvis korrelerade toohello hastighet. Vi använder bredvid PCA-baserad avvikelseidentifiering modulen tooproject hello avkänningsdata från hello 3-dimensionell utrymme till en 2-dimensionell. hello återkallning villkor är uppfyllda och hello vehicle kräver återkallning när motorn temperatur- och utanför temperatur hög negativt korrelerade. Vi använder PCA-baserad algoritm för avvikelseidentifiering kan du avbilda hello avvikelser när du utför PCA. 

Vid inlärning antingen modellen måste toouse normal data, som inte kräver underhåll eller återkallas som hello indata tootrain hello PCA-baserad avvikelseidentifiering identifiering av modellen. I hello bedömningen experiment, använder vi hello tränats avvikelseidentifiering identifiering av modellen toodetect huruvida hello vehicle kräver underhåll eller återkallas. 

### <a name="real-time-analysis"></a>Analys i realtid
följande SQL-frågan i Stream Analytics hello används tooget hello medelvärdet av alla hello viktiga vehicle parametrar som hastigheten, bränslenivå, motorn temperatur, mätarställning, däck tryck, motorn olja nivå och andra. hello medelvärden är används toodetect avvikelser utfärda aviseringar och fastställa hello övergripande hälsa villkor fordon drivas på ett visst område och sedan korrelera toodemographics. 

![Stream Analytics-fråga för realtidsbearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Figur 13-Stream Analytics-fråga för realtidsbearbetning*

Alla hello medelvärden beräknas under en TumblingWindow 3 sekunder. Vi använder TubmlingWindow i det här fallet eftersom vi kräver inte överlappar och sammanhängande intervall. 

toolearn mer om alla hello ”fönsterhantering” funktioner i Azure Stream Analytics, klickar du på [fönsterhantering (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Realtid förutsägelse**

Ett program ingår som en del av hello lösning toooperationalize hello machine learning-modellen i verkliga tid. Det här programmet som kallas ”RealTimeDashboardApp” har skapats och konfigurerats som en del av hello lösningsdistribution. hello program utför hello följande:

1. Lyssnar tooan Event Hub-instans där Stream Analytics publicerar hello händelser i ett mönster kontinuerligt. ![Stream Analytics-fråga för att publicera hello data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *figur 14 – Stream Analytics-fråga för att publicera hello data tooan utdata Event Hub-instans* 
2. För varje händelse som tar emot det här programmet: 
   
   * Processer hello data med hjälp av Machine Learning-svar på begäranden bedömningen (RR) slutpunkt. hello Resursposter endpoint publiceras automatiskt som en del av hello-distribution.
   * hello Resursposter utdata är publicerade tooa Power BI dataset med hello push API: er.

Det här mönstret är också tillämpliga tooscenarios som du vill toointegrate ett Line of Business (LoB)-program med hello analys i realtid flödet för scenarier, till exempel aviseringar, meddelanden och meddelanden.

Klicka på [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio-lösning för anpassningar. 

**tooexecute hello realtid instrumentpanelen för program**
1. Extrahera och spara lokalt ![RealtimeDashboardApp mappen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *bild 16 – RealtimeDashboardApp mapp*  
2. Köra programmet hello RealtimeDashboardApp.exe
3. Ange giltiga autentiseringsuppgifter för Power BI, logga in och klicka på Acceptera ![Realtid instrumentpanelen app inloggning tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Realtid instrumentpanelsapp Slutför inloggningen tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Bild 17 – RealtimeDashboardApp: Logga in tooPower BI*

>[!NOTE] 
>Om du vill tooflush hello Power BI dataset, kör du hello RealtimeDashboardApp med hello ”flushdata”-parametern: 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batchanalys
hello målet är tooshow hur Contoso motorer använder hello Azure compute funktioner tooharness stordata toogain omfattande insikter om körning mönster, användning beteende och vehicle hälsa. Detta gör det möjligt att:

* Förbättra kundupplevelsen hello och gör den billigare genom att ge insikter på Driver vanor och bränsle effektivt intresseväckande beteenden
* Lär dig att kunder och deras intresseväckande patters toogovern affärsbeslut och ange hello bäst i klassen produkter och tjänster

I den här lösningen utvecklar vi hello följande mått:

1. **Styr beteendet aggressivt**: identifierar hello trend hello modeller, platser, intresseväckande villkor och tiden för hello år toogain insikter om aggressivt intresseväckande mönster. Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, driver nya anpassade funktioner och användningsbaserad insurance.
2. **Bränsle effektivt intresseväckande beteende**: identifierar hello trend hello modeller, platser, intresseväckande villkor och tiden för hello år toogain insikter om bränsle effektivt intresseväckande mönster. Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, köra nya funktioner och proaktiv reporting toohello drivrutiner för effektiv och miljö eget intresseväckande vanor. 
3. **Återkalla modeller**: identifierar modeller som kräver återställning av operationalizing hello avvikelseidentifiering identifiering machine learning-experiment

Nu ska vi titta hello detaljer om var och en av de här måtten

**Aggressiv intresseväckande mönster**

hello partitioneras vehicle signaler och diagnostikdata bearbetas i pipeline-hello med namnet ”AggresiveDrivingPatternPipeline” med hjälp av Hive toodetermine hello modeller, plats, vehicle, körning villkor och andra parametrar som uppvisar aggressivt intresseväckande mönster.

![Aggressiv intresseväckande mönster arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*bild 18 – aggressiv intresseväckande mönster-arbetsflöde*


***Aggressiv intresseväckande mönster Hive-fråga***

hello Hive-skript som heter ”aggresivedriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns på ”\demo\src\connectedcar\scripts” hello hämtade zip mapp. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Den använder hello kombination av fordon överföring växeln position, bromsar cyklar status och hastighet toodetect reckless/aggressivt körning beteenden, baserat på bromsar mönster med hög hastighet. 

När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.

![AggressiveDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Bild 19 – AggressiveDrivingPatternPipeline utdata*

**Bränsle effektivt intresseväckande mönster**

hello partitionerad vehicle signaler och diagnostikdata bearbetas i pipeline-hello med namnet ”FuelEfficientDrivingPatternPipeline”. Hive är används toodetermine hello modeller, plats, vehicle, intresseväckande villkor och andra egenskaper som uppvisar bränsle effektivt intresseväckande mönster.

![Bränsleeffektiva intresseväckande mönster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Figur 20 – bränsleeffektiva intresseväckande mönster-arbetsflöde*

***Bränsle effektivt intresseväckande mönster Hive-fråga***

hello Hive-skript som heter ”fuelefficientdriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns på ”\demo\src\connectedcar\scripts” hello hämtade zip mapp. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Den använder hello kombination av fordon överföring växeln position, bromsar cyklar status, hastighet och accelerator cyklar position toodetect bränsle effektivt intresseväckande beteende baserat på acceleration, bromsar och processorhastighet mönster. 

När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.

![FuelEfficientDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Figur 21 – FuelEfficientDrivingPatternPipeline utdata*

**Återkalla förutsägelser**

Hej maskininlärningsexperiment etablerats och publiceras som en webbtjänst som en del av hello lösningsdistribution. i det här arbetsflödet, registrerats som data factory länkad tjänst och operationalized med hjälp av data factory-batchbedömningsaktivitet utnyttjas hello batchbedömningsjobbet slutpunkt.

![Machine Learning-slutpunkt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Figur 22 – Machine learning-slutpunkt som är registrerat som en länkad tjänst i data factory*

hello används registrerade länkade tjänsten i hello DetectAnomalyPipeline tooscore hello data med hjälp av hello avvikelseidentifiering identifiering av modellen. 

![Datorn Learning batchbedömningsaktivitet i data factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Figur 23 – Azure Machine Learning-Batchbedömningen aktivitet i data factory* 

Det finns några steg utförs i den här pipelinen för förberedelse av data så att den kan operationalized med hello batchbedömningsjobbet webbtjänsten. 

![DetectAnomalyPipeline för att förutsäga fordon som kräver återställning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Figur 24 – DetectAnomalyPipeline för att förutsäga fordon som kräver återställning* 

***Avvikelseidentifiering identifiering Hive-fråga***

När hello bedömningen är klar, är en HDInsight-aktivitet används tooprocess och sammanställd hello data som är kategoriserade som avvikelser av hello modell med en sannolikhet poäng 0,60 eller högre.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.

![DetectAnomalyPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Bild 25 – DetectAnomalyPipeline utdata*

## <a name="publish"></a>Publicera

### <a name="real-time-analysis"></a>Analys i realtid
En av hello frågor i hello Stream Analytics-jobbet publicerar hello händelser tooan utdata Event Hub-instans. 

![Stream Analytics-jobbet publicerar tooan utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Bild 26 – Stream Analytics-jobbet publicerar tooan utdata Event Hub-instans*

![Stream Analytics query toopublish toohello utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Bild 27 – Stream Analytics query toopublish toohello utdata Event Hub-instans*

Den här dataströmmen av händelser som förbrukas av hello RealTimeDashboardApp ingår i hello lösningen. Det här programmet utnyttjar hello Machine Learning begäran och svar webbtjänst för realtid poäng och publicerar hello resulterande data tooa Power BI dataset för användning. 

### <a name="batch-analysis"></a>Batchanalys
hello resultatet av hello batch och realtidsbearbetning är publicerade toohello Azure SQL Database-tabeller för användning. hello Azure SQL Server, databas och hello tabeller skapas automatiskt som en del av hello installationsskriptet. 

![Resultat för batch-bearbetning kopiera toodata mart arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Bild 28 – batchbearbetning resultat kopiera toodata mart arbetsflöde*

![Stream Analytics-jobbet publicerar toodata mart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Bild 29 – Stream Analytics-jobbet publicerar toodata mart*

![Data mart-inställningen i Stream Analytics-jobbet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Bild 30 – dataarkiv i Stream Analytics-jobbet*

## <a name="consume"></a>Förbruka
Powerbi ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar. 

Klicka här för detaljerade anvisningar om hur du konfigurerar hello Power BI-rapporter och hello instrumentpanel. hello slutliga instrumentpanelen ser ut så här:

![Power BI-instrumentpanel](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Bild 31 - Power BI-instrumentpanel*

## <a name="summary"></a>Sammanfattning
Det här dokumentet innehåller en detaljerad nedåt i hello Vehicle telemetri Analytics lösning. Detta prov på ett lambda-arkitektur mönster för realtid och batch-analytics med förutsägelser och åtgärder. Det här mönstret gäller tooa mängd användningsområden som kräver varm sökväg (realtid) och kalla sökväg (batch) analyser. 

