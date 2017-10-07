---
title: aaaCopy aktivitet prestanda och prestandajustering guiden | Microsoft Docs
description: "Läs mer om viktiga faktorer som påverkar hello prestanda för flytt av data i Azure Data Factory när du använder Kopieringsaktiviteten."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jingwang
ms.openlocfilehash: b0fb5a76c34752d07e8ddfffbb799a05fb5d6be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiera prestandajustering guide och prestanda för aktiviteten
Azure Data Factory-Kopieringsaktiviteten levererar en förstklassig säker, tillförlitlig och högpresterande datainläsning lösning. Det kan du toocopy flera terabyte data varje dag i omfattande olika molnet och lokala datalager. Blixtsnabb snabb prestanda för datainläsning är viktiga tooensure kan du fokusera på hello core ”big data” problem: skapa lösningar för avancerade analyser och hämtar djupa insikter från alla data.

Azure tillhandahåller en uppsättning företagsklass lösningar för lagring och data warehouse och Kopieringsaktivitet erbjuder en mycket optimerade datainläsning enkelt tooconfigure och konfigurera. Med bara en enda kopia aktivitet, kan du få:

* Läser in data i **Azure SQL Data Warehouse** på **1,2 Gbit/s**. En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).
* Läser in data i **Azure Blob storage** på **1.0 Gbit/s**
* Läser in data i **Azure Data Lake Store** på **1.0 Gbit/s**

Den här artikeln beskrivs:

* [Prestanda referensnummer](#performance-reference) för stöds källa och mottagare data lagras toohelp som du planerar att projektet.
* Funktioner som kan öka hello kopiera genomflöde i olika scenarier, inklusive [molnet data movement enheter](#cloud-data-movement-units), [parallell kopiera](#parallel-copy), och [mellanlagrad kopiera](#staged-copy);
* [Riktlinjer för prestandajustering](#performance-tuning-steps) på hur kopiera tootune hello prestanda och hello viktiga faktorer som kan påverka prestanda.

> [!NOTE]
> Om du inte är bekant med Kopieringsaktiviteten i allmänhet kan se [flytta data med hjälp av Kopieringsaktiviteten](data-factory-data-movement-activities.md) innan du läser den här artikeln.
>

## <a name="performance-reference"></a>Prestandareferens för

Tabellen nedan visar som referens, du hello kopiera genomströmning numret i Mbit/s för hello får källa och mottagare par baserat på interna tester. För jämförelse, visas också hur olika inställningar för [molnet data movement enheter](#cloud-data-movement-units) eller [Data Management Gateway skalbarhet](data-factory-data-management-gateway-high-availability-scalability.md) (flera gateway-noder) hjälper dig att kopiera prestanda.

![Matris för prestanda](./media/data-factory-copy-activity-performance/CopyPerfRef.png)


**Punkter toonote:**
* Genomströmning beräknas med hjälp av hello följande formel: [storleken på data läsas från källan] / [kopia aktivitet kör duration].
* hello prestanda referensnummer i hello tabellen har mäts med [TPC-H](http://www.tpc.org/tpch/) datauppsättningen i en enda kopia aktivitet körs.
* I Azure datalager hello källa och mottagare är i hello samma Azure-region.
* För hybridkopiering mellan lokala och moln datalager, varje gateway-noden körs på en dator som är separat från hello lokalt datalager med nedan specifikation. När en enskild aktivitet kördes på gateway förbrukat hello kopieringsåtgärden bara en liten del av hello testa dator CPU, minne eller bandbredd i nätverket. Mer information från [för Data Management Gateway](#considerations-for-data-management-gateway).
    <table>
    <tr>
        <td>Processor</td>
        <td>32 kärnor 2.20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Minne</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Nätverk</td>
        <td>Internet-gränssnittet: 10 Gbit/s; intranätgränssnittet: 40 Gbit/s</td>
    </tr>
    </table>


> [!TIP]
> Du kan få bättre genomströmning genom att utnyttja mer data movement enheter (DMUs) än hello standard maximala DMUs som är 32 för en moln-to-cloud kopieringsaktiviteten kör. Till exempel med 100 DMUs du kan åstadkomma kopiering av data från Azure Blob till Azure Data Lake Store på **1.0GBps**. Se hello [molnet data movement enheter](#cloud-data-movement-units) finns mer information om den här funktionen och hello stöds scenario. Kontakta [Azure-supporten](https://azure.microsoft.com/support/) toorequest mer DMUs.

## <a name="parallel-copy"></a>Parallell kopia
Du kan läsa från hello datakällan eller skriva data toohello mål **parallellt i en Kopieringsaktivitet kör**. Den här funktionen förbättrar hello genomströmning på en kopieringsåtgärd och minskar hello tid det tar toomove data.

Denna inställning skiljer sig från hello **samtidighet** egenskap i hello aktivitetsdefinitionen. Hej **samtidighet** egenskapen bestämmer hello antal **samtidiga Kopieringsaktiviteten körs** tooprocess data från olika aktivitet windows (1 AM too2 AM, 2 AM too3 är, 3 kl too4 är och så vidare). Den här funktionen är användbart när du utför en historisk belastning. hello parallella kopiera kapaciteten gäller tooa **enkel aktivitet kör**.

Nu ska vi titta på ett exempelscenario. I följande exempel hello, måste flera segment från hello senaste toobe bearbetas. Data Factory kör en instans av Kopieringsaktiviteten (en aktivitet kör) för varje segment:

* Hej datasektorn från hello första aktivitetsfönstret (1 AM too2 är) == > aktivitet köras 1
* Hej datasektorn från hello andra aktivitetsfönstret (2 AM too3 är) == > aktivitet köras 2
* Hej datasektorn från hello andra aktivitetsfönstret (3 kl too4 är) == > aktivitet köras 3

Och så vidare.

I det här exemplet när hello **samtidighet** värdet too2, **aktivitet köras 1** och **aktivitet köras 2** kopiera data från två aktivitet windows **samtidigt**  tooimprove data movement prestanda. Men om flera filer som är associerade med aktiviteten kör 1, kopierar hello data movement service filer från hello toohello mål en källfil i taget.

### <a name="cloud-data-movement-units"></a>Molnet data movement enheter
En **moln data movement enhet (dmu här)** är ett mått som representerar hello styrka (en kombination av CPU, minne och nätverksresursallokering) en enhet i Data Factory. En dmu här kan användas i ett moln-to-cloud kopieringsåtgärden, men inte i en hybrid-kopia.

Som standard använder Data Factory en enstaka moln dmu här tooperform en enskild kopia aktivitet körs. toooverride standard, ange ett värde för hello **cloudDataMovementUnits** egenskapen på följande sätt. Information om hello nivå av prestanda som du kan få när du konfigurerar flera enheter för en specifik kopieringskälla och mottagare finns hello [Prestandareferens](#performance-reference).

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```
Hej **tillåtna värden** för hello **cloudDataMovementUnits** egenskapen är 1 (standard), 2, 4, 8, 16, 32. Hej **faktiska antalet molnet DMUs** att hello kopieringen används vid körning är lika tooor som är lägre än hello konfigurerat värde, beroende på din datamönster.

> [!NOTE]
> Om du behöver mer molnet DMUs för en högre genomströmning Kontakta [Azure-supporten](https://azure.microsoft.com/support/). Inställning av 8 och senare fungerar aktuellt endast när du **kopierar du flera filer från Blob lagring/Datasjölager/Amazon S3 eller ett moln FTP-eller ett moln SFTP tooBlob lagring/Data Lake Store/Azure SQL Database**.
>

### <a name="parallelcopies"></a>parallelCopies
Du kan använda hello **parallelCopies** egenskapen tooindicate hello parallellitet som du vill Kopieringsaktiviteten toouse. Du kan se den här egenskapen som hello maximalt antal trådar inom Kopieringsaktiviteten som kan läsa från käll- eller skriva tooyour sink datalager parallellt.

För varje kopia aktivitet kör, bestämmer Data Factory hello antalet parallella kopierar toouse toocopy data från datalagret för hello källa och toohello importera data till. hello standardantalet parallella kopior som ska användas beror på hello typ av källa och mottagare som du använder.  

| Källa och mottagare | Standardvärdet för parallell Kopiera antal bestäms av tjänsten |
| --- | --- |
| Kopiera data mellan filbaserade butiker (Blob-lagring. Data Lake Store; Amazon S3; ett lokalt filsystem; ett lokalt HDFS) |Mellan 1 och 32. Beror på hello mycket hello filer och hello antal molnet data movement enheter (DMUs) används toocopy data mellan två molnet datalager eller hello fysiska konfigurationen av hello Gateway-datorn som används för hybridkopiering (toocopy data tooor från ett lokalt datalager ). |
| Kopiera data från **alla källdata lagra tooAzure Table storage** |4 |
| Alla andra källa och mottagare par |1 |

Vanligtvis hello standardbeteendet bör ge dig hello bästa genomflöde. Dock toocontrol hello läses in på datorer som är värdar för ditt datalager eller tootune kopiera prestanda, kan du välja toooverride hello standardvärdet och ange ett värde för hello **parallelCopies** egenskapen. hello-värdet måste vara mellan 1 och 32 (båda inkluderande). Vid körning för bästa prestanda hello Kopieringsaktiviteten använder du ett värde som är mindre än eller lika toohello värdet som du anger.

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Punkter toonote:

* När du kopierar data mellan filbaserade lagrar hello **parallelCopies** fastställa hello parallellitet till hello filnivå. Hej högoptimerat inom en enskild fil skulle inträffa under automatiskt och transparent och den är utformad toouse hello bästa lämplig segmentstorleken för en viss källa data store Skriv tooload data i parallella och ortogonala tooparallelCopies. hello faktiskt antal parallella kopior hello data movement service använder för hello kopieringen vid körning är högst hello antalet filer som du har. Om hello kopiera beteendet är **mergeFile**, Kopieringsaktivitet kan inte utnyttja filnivå parallellitet.
* När du anger ett värde för hello **parallelCopies** egenskap, bör du hello belastningen ökar på käll- och mottagarnoderna datalager och toogateway om det är en kopia av hybrid. Detta händer särskilt när du har flera aktiviteter eller samtidiga körningar av hello samma aktiviteter som körs mot hello samma datalager. Om du märker datalagret hello eller Gateway många med hello belastning, minska hello **parallelCopies** värdet toorelieve hello belastningen.
* När du kopierar data från butiker som inte är filbaserade toostores är filbaserade hello data movement service ignorerar hello **parallelCopies** egenskapen. Även om parallellitet anges tillämpas den inte i det här fallet.

> [!NOTE]
> Du måste använda Data Management Gateway version 1.11 eller senare toouse hello **parallelCopies** funktion när du gör en kopia av hybrid.
>
>

toobetter använder de här två egenskaperna och tooenhance din datagenomströmning flytt, se hello [exempel på användningsområden](#case-study-use-parallel-copy). Du behöver inte tooconfigure **parallelCopies** tootake nytta av hello standardbeteendet. Om du konfigurerar och **parallelCopies** är för liten flera molnet DMUs inte kan användas fullt ut.  

### <a name="billing-impact"></a>Fakturering påverkan
Den har **viktiga** tooremember som du debiteras baserat på hello total tid för hello kopieringen. Om en kopieringsjobbet används tootake en timme till ett moln enhet och nu det tar 15 minuter med fyra enheter i molnet, hello hello övergripande faktura förblir nästan samma. Exempelvis kan du använda fyra enheter i molnet. hello första molnet enhet använder 10 minuter, hello andra en, 10 minuter hello tredje ett, 5 minuter och hello fjärde ett, 5 minuter, allt i en Kopieringsaktivitet kör. Du debiteras för hello Totalt antal copy (dataflyttning) gång, vilket är 10 + 10 + 5 + 5 = 30 minuter. Med hjälp av **parallelCopies** påverkar inte fakturering.

## <a name="staged-copy"></a>Stegvis kopia
När du kopierar data från en källa data store tooa sink dataarkiv kan du välja toouse Blob storage som ett tillfälligt fristående Arkiv. Mellanlagring är särskilt användbart i följande fall hello:

1. **Du vill tooingest data från olika datalager till SQL Data Warehouse via PolyBase**. SQL Data Warehouse använder PolyBase som en mekanism för hög genomströmning tooload stora mängder data till SQL Data Warehouse. Dock hello källdata måste vara i Blob storage och det måste uppfylla ytterligare villkor. När du läser in data från ett annat dataarkiv än Blob storage kan du Aktivera kopiering via mellanlagring Blob mellanlagring data. I så fall utför Data Factory hello krävs data transformationer tooensure att den uppfyller hello av PolyBase. Därefter används PolyBase tooload data till SQL Data Warehouse. Mer information finns i [Använd PolyBase tooload data till Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).
2. **Ibland tar en stund tooperform hybrid dataflyttning (det vill säga toocopy mellan ett lokalt datalager och ett datalager i molnet) över en långsam nätverksanslutning**. Du kan komprimera hello data lokalt så att det tar mindre tid toomove data toohello fristående datalager i hello molnet tooimprove prestanda. Sedan kan du expandera hello data i hello mellanlagring store innan du läser in i datalagret för hello mål.
3. **Du inte vill tooopen portar än port 80 och port 443 i brandväggen, på grund av företagets IT-principer**. När du kopierar data från en lokal data store tooan Azure SQL Database sink eller en Azure SQL Data Warehouse sink måste du till exempel tooactivate utgående TCP-kommunikation på port 1433 för företagets brandvägg och hello Windows-brandväggen. I det här scenariot utnyttja hello toofirst kopiera data tooa Blob storage fristående gatewayinstansen via HTTP eller HTTPS på port 443. Hämta sedan hello data till SQL Database eller SQL Data Warehouse från Blob storage mellanlagring. I det här flödet behöver du inte tooenable port 1433.

### <a name="how-staged-copy-works"></a>Hur mellanlagrade Kopiera verk
När du aktiverar hello mellanlagring funktionen första hello data kopieras från hello källa data store toohello mellanlagring datalager (ta med din egen). Därefter kopieras hello data från hello mellanlagring data store toohello sink-datalagret. Data Factory hanterar automatiskt hello två steg flödet för dig. Data Factory också Rensar tillfällig data från hello mellanlagring lagring när hello dataflyttning är klar.

Gateway används inte i hello molnet kopiera scenario (både källa och mottagare data lagras i molnet hello). hello Data Factory-tjänsten utför hello kopia.

![Mellanlagrad kopia: scenariot](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

I hello hybridscenario kopia (källan är lokala och mottagare finns i hello moln) hello gateway flyttar data från hello källdata lagra tooa mellanlagring datalagret. Data Factory-tjänsten flyttar data från hello mellanlagring data lagra toohello sink-datalagret. Kopiera data från ett moln data store tooan lokalt datalager via mellanlagring stöds även med omvänd hello flödet.

![Mellanlagrad kopia: hybridscenario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

När du aktiverar dataflyttning med hjälp av ett fristående Arkiv, kan du ange om du vill hello data toobe komprimerade innan du flyttar data från hello källa data lagra tooan interim eller mellanlagring datalager och expanderas innan du flyttar data från en interimistisk eller fristående datalager toohello sink-datalagret.

För närvarande kan du kopiera data mellan två lokala datalager med hjälp av ett fristående Arkiv. Vi hoppas det här alternativet toobe tillgänglig snart.

### <a name="configuration"></a>Konfiguration
Konfigurera hello **enableStaging** inställningen i Kopieringsaktiviteten toospecify om du vill hello data toobe mellanlagras i Blob storage innan du läser in den i ett dataarkiv som mål. När du anger **enableStaging** tooTRUE, ange hello ytterligare egenskaper som anges i hello nästa tabell. Om du inte har någon, du måste också toocreate ett Azure Storage eller lagring delade åtkomst signatur-länkad tjänst för Förproduktion.

| Egenskap | Beskrivning | Standardvärde | Krävs |
| --- | --- | --- | --- |
| **enableStaging** |Om du vill toocopy data via en interimistisk mellanlagring store. |False |Nej |
| **linkedServiceName** |Ange hello namnet på en [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) eller [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) länkade tjänst som refererar toohello instans av lagring som du använder som ett tillfälligt fristående Arkiv. <br/><br/> Du kan använda lagring med en delad åtkomst signatur tooload data till SQL Data Warehouse via PolyBase. Du kan använda den i andra scenarier. |Saknas |Ja, när **enableStaging** anges tooTRUE |
| **sökväg** |Ange hello Blob storage sökväg som du vill toocontain hello mellanlagrade data. Om du inte anger en sökväg skapar hello tjänsten en behållare toostore tillfälliga data. <br/><br/> Ange en sökväg om du använder lagring med en signatur för delad åtkomst eller du behöver tillfällig data toobe på en viss plats. |Saknas |Nej |
| **enableCompression** |Anger om data ska komprimeras innan den kopierade toohello mål. Den här inställningen minskar hello mängden data som överförs. |False |Nej |

Här är en exempel-definition av Kopieringsaktiviteten med hello egenskaper som beskrivs i föregående tabell hello:

```json
"activities":[  
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Fakturering påverkan
Du debiteras baserat på två steg: kopiera varaktighet och typen.

* När du använder debiteras mellanlagring under en molnkopieringen (kopiera data från ett datalager för molnet data store tooanother molnet), du hello [summan av kopiera varaktighet för steg 1 och 2] x [kopia-enhetspriset för molnet].
* När du använder mellanlagring under en hybridkopiering (kopiera data från ett dataarkiv för lokala data store tooa molnet), debiteras du för [hybrid kopiera duration] x [kopia-enhetspriset för hybrid] + [kopia varaktighet för moln] x [kopia-enhetspriset för molnet].

## <a name="performance-tuning-steps"></a>Prestanda prestandajustering steg
Vi rekommenderar att du har gjort tootune hello prestandan för din Data Factory-tjänsten med Kopieringsaktiviteten:

1. **Upprätta en baslinje för**. Testa din pipeline under hello utvecklingsfasen med hjälp av Kopieringsaktiviteten mot ett representativt exempel. Du kan använda hello Data Factory [segmentering modellen](data-factory-scheduling-and-execution.md) toolimit hello mängden data som du arbetar med.

   Samla in körningstiden och prestandaegenskaper med hjälp av hello **övervakning och hantering av App**. Välj **övervaka och hantera** på startsidan Data Factory. I trädvyn hello väljer hello **utdatauppsättningen**. I hello **aktivitet Windows** Välj hello Kopieringsaktiviteten kör. **Aktiviteten Windows** visar hello Kopiera aktivitetens varaktighet och hello storleken på hello data som kopieras. hello genomströmning listas i **aktivitet fönstret Explorer**. toolearn mer om hello appen finns [övervaka och hantera Azure Data Factory pipelines med hjälp av hello övervakning och hantering av App](data-factory-monitor-manage-app.md).

   ![Aktivitetskörningsinformation](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Senare i hello artikeln kan du jämföra hello prestanda och konfigurationen av din scenariot tooCopy aktivitets [Prestandareferens](#performance-reference) från våra tester.
2. **Diagnostisera och optimera prestanda**. Om hello prestanda du se inte uppfyller dina förväntningar, måste tooidentify flaskhalsar. Sedan kan optimera prestanda tooremove eller minska hello effekten av flaskhalsar. En fullständig beskrivning av Prestandadiagnos ligger utanför hello i den här artikeln, men här är några vanliga överväganden:

   * Avancerade funktioner:
     * [Parallell kopia](#parallel-copy)
     * [Molnet data movement enheter](#cloud-data-movement-units)
     * [Stegvis kopia](#staged-copy)
     * [Data Management Gateway skalbarhet](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Gateway för datahantering](#considerations-for-data-management-gateway)
   * [Källa](#considerations-for-the-source)
   * [Sink](#considerations-for-the-sink)
   * [Serialisering och deserialisering](#considerations-for-serialization-and-deserialization)
   * [Komprimering](#considerations-for-compression)
   * [Kolumnmappningen](#considerations-for-column-mapping)
   * [Andra överväganden](#other-considerations)
3. **Expandera hello configuration tooyour hela datauppsättningen**. När du är nöjd med hello Körningsresultat och prestanda kan du expandera hello definition och pipeline aktiva period toocover hela datauppsättningen.

## <a name="considerations-for-data-management-gateway"></a>Överväganden för Data Management Gateway
**Installationsprogram för gateway**: Vi rekommenderar att du använder en särskild dator toohost Data Management Gateway. Se [överväganden för att använda Data Management Gateway](data-factory-data-management-gateway.md#considerations-for-using-gateway).  

**Gateway-övervakning och upp/skalbar**: en enskild logisk gateway med en eller flera gateway-noder kan hantera flera Kopieringsaktiviteten körs vid hello samma tid samtidigt. Du kan visa nära realtid ögonblicksbild av resursutnyttjande (CPU, minne, network(in/out) osv) på en gateway-dator samt hello antal samtidiga jobb som körs mot gräns i hello Azure-portalen finns [övervakaren gateway i hello portal](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Om du har tunga behov på hybrid dataflyttning med stort antal samtidiga kopiera aktiviteten körs eller med stora mängder data toocopy överväga för[skala upp eller ut gateway](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) så toobetter använda din resurs eller tooprovision mer resurs tooempower kopiera. 

## <a name="considerations-for-hello-source"></a>Överväganden för hello källa
### <a name="general"></a>Allmänt
Var noga med att hello underliggande datalagret inte är överbelastad till följd av andra arbetsbelastningar som körs på eller i förhållande till den.

Microsoft datalager finns [övervaka och justera avsnitt](#performance-reference) som är specifika toodata butiker och hjälper dig att förstå data lagra prestandaegenskaper, minimera svarstider och maximera genomströmningen.

Om du kopierar data från Blob storage tooSQL Data Warehouse, kan du använda **PolyBase** tooboost prestanda. Se [Använd PolyBase tooload data till Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) mer information. En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Filbaserade datakällor
*(Inklusive Blob-lagring, Data Lake Store, Amazon S3, lokalt filsystem och lokala HDFS)*

* **Genomsnittlig storlek och antalet filer**: Kopieringsaktiviteten överför en fil i taget. Med hello samma mängd data toobe flyttas, är hello totala genomflödet lägre om hello data består av flera små filer i stället för ett par stora filer på grund av toohello bootstrap fas för varje fil. Därför om möjligt, kombinera små filer till större filer toogain högre genomströmning.
* **Format och komprimering**: finns flera sätt tooimprove prestanda hello [överväganden för serialisering och deserialisering](#considerations-for-serialization-and-deserialization) och [överväganden för komprimering](#considerations-for-compression) avsnitt.
* För hello **lokalt filsystem** scenario där **Data Management Gateway** är krävs, se hello [överväganden för Data Management Gateway](#considerations-for-data-management-gateway) avsnitt.

### <a name="relational-data-stores"></a>Lagrar relationsdata
*(Omfattar SQL-databasen. SQL Data Warehouse; Amazon Redshift; SQL Server-databaser. och Oracle, MySQL, DB2, Teradata, Sybase och PostgreSQL databaser osv.)*

* **Datamönster**: tabellens schema påverkar kopiera genomflöde. En stor Radstorleken ger en bättre prestanda än små Radstorleken, toocopy hello samma mängd data. hello orsak är att databasen hello mer effektivt kan hämta färre batchar av data som innehåller färre rader.
* **Fråga eller lagrad procedur**: Optimera hello logiken för hello fråga eller en lagrad procedur som du anger i hello Kopieringsaktiviteten toofetch källdata mer effektivt.
* För **lokala relationsdatabaser**, till exempel SQL Server och Oracle, vilket kräver att hello använder **Data Management Gateway**, se hello [överväganden för Data Management Gateway](#considerations-on-data-management-gateway) avsnitt.

## <a name="considerations-for-hello-sink"></a>Överväganden för hello sink
### <a name="general"></a>Allmänt
Var noga med att hello underliggande datalagret inte är överbelastad till följd av andra arbetsbelastningar som körs på eller i förhållande till den.

Microsoft datalager finns för[övervaka och justera avsnitt](#performance-reference) som är specifika toodata Arkiv. Dessa avsnitt kan hjälpa dig att förstå dataegenskaper store prestanda och hur toominimize svarstider och maximera genomströmningen.

Om du kopierar data från **Blob storage** för**SQL Data Warehouse**, Överväg att använda **PolyBase** tooboost prestanda. Se [Använd PolyBase tooload data till Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) mer information. En genomgång med ett användningsfall finns [läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Azure Data Factory](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Filbaserade datakällor
*(Inklusive Blob-lagring, Data Lake Store, Amazon S3, lokalt filsystem och lokala HDFS)*

* **Kopiera beteende**: Om du kopierar data från ett annat filbaserat dataarkiv Kopieringsaktiviteten har tre alternativ via hello **copyBehavior** egenskapen. Bevarar hierarki, förenklas hierarkin eller sammanfogar filer. Bevara eller förenkla hierarkin har liten eller ingen prestanda försämras men Sammanfoga filer orsakar prestanda övergripande tooincrease.
* **Format och komprimering**: Se hello [överväganden för serialisering och deserialisering](#considerations-for-serialization-and-deserialization) och [överväganden för komprimering](#considerations-for-compression) avsnitt för mer sätt tooimprove prestanda .
* **BLOB storage**: för närvarande Blob storage stöder endast blockblobbar för optimerad dataöverföring och genomflöde.
* För **lokalt filsystem** scenarier som kräver hello användning av **Data Management Gateway**, se hello [överväganden för Data Management Gateway](#considerations-for-data-management-gateway) avsnitt.

### <a name="relational-data-stores"></a>Lagrar relationsdata
*(Omfattar SQL-databas, SQL Data Warehouse, SQL Server-databaser och Oracle-databaser)*

* **Kopiera beteende**: beroende på hello egenskaper som har angetts för **sqlSink**, Kopieringsaktiviteten skriver data toohello måldatabasen på olika sätt.
  * Som standard hello hello data movement service använder hello Bulk Copy API tooinsert data i append läge, som ger bästa prestanda.
  * Om du konfigurerar en lagrad procedur i hello sink gäller hello en datarad samtidigt i stället för som en massinläsning hello-databasen. Datorns prestanda sjunker. Om din datauppsättning är stor, i förekommande fall, Överväg att byta toousing hello **sqlWriterCleanupScript** egenskapen.
  * Om du konfigurerar hello **sqlWriterCleanupScript** egenskap för varje kopia aktiviteten kör, hello tjänsten triggers hello skript och sedan använder hello Bulk Copy API tooinsert hello data. Till exempel toooverwrite hello hela tabellen med hello senaste data, du kan ange ett skript toofirst ta bort alla poster innan nya data för massinläsning hello hello källa.
* **Mönstret och batch datastorleken**:
  * Tabellens schema påverkar kopiera genomflöde. toocopy Hej samma mängd data, en stor Radstorleken får du bättre prestanda än en liten Radstorleken eftersom hello databasen mer effektivt kan genomföra färre batchar av data.
  * Kopieringsaktiviteten infogar data i en serie med batchar. Du kan ange hello antalet rader i en batch med hello **writeBatchSize** egenskapen. Om dina data har liten rader, kan du ange hello **writeBatchSize** egenskap med ett högre värde toobenefit från lägre batch kostnader och högre genomströmning. Om hello Radstorleken på dina data är stor, var försiktig när du ökar **writeBatchSize**. Ett högt värde kan det leda tooa kopia misslyckades på grund av överbelastning hello-databasen.
* För **lokala relationsdatabaser** som SQL Server och Oracle, vilket kräver att hello använder **Data Management Gateway**, se hello [överväganden för Data Management Gateway](#considerations-for-data-management-gateway) avsnitt.

### <a name="nosql-stores"></a>NoSQL-lagringsplatser
*(Omfattar tabellagring och Azure Cosmos DB)*

* För **tabell lagring**:
  * **Partitionen**: skriva data toointerleaved partitioner avsevärt försämrar prestanda. Sortera källdata av partitionsnyckel så att hello data infogas effektivt i en partition efter en annan eller justera hello logik toowrite hello data tooa enda partition.
* För **Cosmos Azure DB**:
  * **Batchstorlek**: hello **writeBatchSize** egenskapen anger hello antalet parallella begäranden toohello Azure Cosmos DB toocreate servicedokument. Du kan förvänta dig bättre prestanda om du ökar **writeBatchSize** eftersom fler parallella begäranden skickas tooAzure Cosmos DB. Dock bevaka begränsning när du skriver tooAzure Cosmos DB (hello felmeddelandet är ”begär frekvensen är stor”). Olika faktorer kan orsaka begränsning hello antalet villkoren i hello dokument, inklusive dokumentets storlek och hello målsamlingen indexprincip. tooachieve högre kopiera dataflöde, Överväg att använda en bättre samling, till exempel S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Överväganden för serialisering och deserialisering
Serialisering och deserialisering kan inträffa när din inkommande datauppsättning eller datamängd för utdata är en fil. Se [stöds format och komprimering](data-factory-supported-file-and-compression-formats.md) med information om filformat som stöds av Kopieringsaktiviteten.

**Kopiera beteende**:

* Kopierar filer mellan filbaserade datalager:
  * När inkommande och utgående datauppsättningar som båda har hello samma eller inga inställningar för format, hello data movement service kör en binär kopia utan serialisering eller deserialisering. Du ser ett högre genomflöde jämfört med toohello scenario, i vilken hello källa och mottagare filen formatinställningar skiljer sig från varandra.
  * När indata och utdata datauppsättningar båda finns i textformat och endast hello kodning typen är olika, hello data movement service har endast kodning konvertering. Den gör inte alla serialisering och deserialisering, vilket gör att vissa prestanda försämras jämförs tooa binära kopia.
  * När inkommande och utgående datauppsättningar båda har olika filformat eller olika konfigurationer som avgränsare, hello data movement service deserializes källa data toostream, transformera och serialisera det till hello utdataformat som du angett. Den här åtgärden resulterar i en mycket större prestanda försämras jämfört med tooother scenarier.
* När du kopierar filer till eller från ett dataarkiv som inte är filbaserade (till exempel från ett filbaserat store tooa relationella store) krävs hello serialisering eller deserialisering steg. Det här steget resulterar i betydande prestanda försämras.

**Filformatet**: hello-filformat som du väljer kan påverka prestanda för kopia. Avro är till exempel ett compact binärformat som lagrar metadata med data. Det har brett stöd i hello Hadoop-ekosystemet för bearbetning och frågor. Avro är dock dyrare för serialisering och deserialisering, vilket resulterar i lägre kopiera genomflöde jämfört med tootext format. Kontrollera ditt val av filformatet i hela hello processchema helhetsmässigt. Börja med vilka formuläret hello data lagras i, käll-datalager eller toobe extraheras från externa system. hello bästa format för lagring, analytisk bearbetning och frågar; och i vilket format hello data ska exporteras till dataarkiv för rapportering och visualisering verktyg. Ibland ett format som är något sämre för läsa och skriva prestanda kan vara bra när du hello övergripande analytiska processen.

## <a name="considerations-for-compression"></a>Överväganden för komprimering
När din inkommande eller utgående data är en fil, kan du ange Kopieringsaktiviteten tooperform komprimering eller dekomprimering som skriver data toohello mål. När du väljer komprimering kan du göra en kompromiss mellan indata/utdata (I/O) och processor. Komprimera data hello beräkningsresurser extra kostnader. Men i RETUR minskas från nätverket och lagring. Beroende på dina data, kan du se en höjning av den totala kopiera genomflödet.

**Codec**: Kopieringsaktiviteten stöder gzip, bzip2 och Deflate komprimeringstyper. Azure HDInsight kan använda alla tre typer för bearbetning. Varje komprimerings-codec har fördelar. Till exempel bzip2 har hello lägsta kopiera dataflöde, men du får hello bästa Hive frågeprestanda med bzip2 eftersom du kan dela för bearbetning. Gzip är hello mest belastningsutjämnade alternativ och den används hello oftast. Välj hello codec som passar bäst för ditt scenario för slutpunkt till slutpunkt.

**Nivå**: du kan välja mellan två alternativ för varje komprimerings-codec: snabbaste komprimerade och optimalt komprimerade. hello komprimerar snabbaste komprimerade alternativet hello data så snabbt som möjligt, även om hello resulterande filen inte komprimeras optimalt. hello optimalt komprimerade alternativet ägnar mer tid åt komprimering och ger en minimal mängd data. Du kan testa båda alternativen toosee som ger bättre prestanda i ditt fall.

**En faktor**: toocopy stora mängder data mellan en lokal lagring och hello moln, Överväg att använda tillfälliga blob-lagring med komprimering. Det är praktiskt att använda mellanlagring när hello bandbredd i företagets nätverk och Azure-tjänster är hello begränsande faktor och du vill hello indata datauppsättning och utdata ange båda toobe okomprimerad. Mer specifikt kan du dela upp en enda kopia aktivitet i två kopiera aktiviteter. hello första kopiera aktiviteten kopieras från hello källa tooan interim eller fristående blob i komprimerat format. hello andra kopieringsaktiviteten kopierar hello komprimerade data från mellanlagring och expanderar sedan medan skriver toohello mottagare.

## <a name="considerations-for-column-mapping"></a>Överväganden för kolumnmappningen
Du kan ange hello **columnMappings** egenskap i Kopieringsaktiviteten toomap alla eller en delmängd av hello indatakolumnerna kolumner toohello utdata. När hello data movement service läser hello data från hello källa, måste tooperform kolumnmappningen på hello data innan skrivning hello data toohello mottagare. Den här extra bearbetning minskar kopiera genomflöde.

Om källa datalager frågbar kan till exempel om det är en relationell butik som SQL Database eller SQL Server, eller om det är en NoSQL-butiken som Table storage eller Azure Cosmos DB du sänder hello kolumnen filtrering och omordning logik toohello **fråga**  egenskapen istället för att använda kolumnmappningen. Det här sättet inträffar hello projektion när hello data movement service läser data från hello källdata store, där det är mycket effektivare.

## <a name="other-considerations"></a>Andra överväganden
Om hello storlek logik toofurther partition hello företagsdata med hjälp av du vill använda toocopy är stor, kan du justera hello segmentering mekanism i Data Factory. Schemalägg sedan Kopieringsaktiviteten toorun oftare tooreduce hello Datastorlek för varje kopia-aktivitet körs.

Vara försiktig hello antal datauppsättningar och kopiera aktiviteter som kräver Data Factory tooconnector toohello samma datalager på hello samma tid. Många samtidiga kopiera jobb kan begränsa ett datalager och lead toodegraded prestanda, kopiera jobbet interna försök, och i vissa fall, fel vid körning.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-tooblob-storage"></a>Exempelscenario: kopiera från en lokal SQL Server tooBlob lagringsplats
**Scenariot**: en pipeline bygger toocopy data från en lokal SQL Server tooBlob lagring i CSV-format. toomake Hej kopieringsjobbet snabbare, hello CSV-filer ska komprimeras till bzip2 format.

**Test- och**: hello genomflödet i Kopieringsaktiviteten är mindre än 2 Mbit/s som är mycket långsammare än hello prestanda prestandamått.

**Prestandaanalys och justera**: tootroubleshoot Hej prestandaproblem, ska vi titta på hur hello data behandlas och flyttas.

1. **Läsa data**: Gateway öppnar en anslutning tooSQL Server och skickar hello frågan. SQL Server svarar genom att skicka hello data dataströmmen tooGateway via hello intranät.
2. **Serialisera och komprimera data**: Gateway Serialiserar hello dataformat dataströmmen tooCSV och komprimerar hello dataströmmen tooa bzip2.
3. **Skriva data**: Gateway överför hello bzip2 dataströmmen tooBlob lagring via hello Internet.

Som du ser hello data håller på att behandlas och flyttas i strömmande ordning: SQL Server > LAN > Gateway > WAN > Blob storage. **hello prestandan är gated av hello minsta dataflöde över hello pipeline**.

![Dataflöde](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

En eller flera av följande faktorer hello orsaka hello flaskhalsar:

* **Källan**: SQL Server har låg genomströmning på grund av tunga laster.
* **Data Management Gateway**:
  * **LAN**: Gateway finns är långt från hello SQL Server-dator och har en långsam anslutning.
  * **Gateway**: Gateway har nått sin belastningen begränsningar tooperform hello följande åtgärder:
    * **Serialisering**: serialiseringen hello data dataströmmen tooCSV format har långsam genomflöde.
    * **Komprimering**: du har valt en långsam komprimerings-codec (till exempel bzip2, vilket är 2,8 Mbit/s med Core i7).
  * **WAN**: hello bandbredden mellan hello företagsnätverket och Azure-tjänster är låg (till exempel T1 = 1,544 kbit/s; T2 = 6,312 kbit/s).
* **Sink**: Blob storage har låg genomströmning. (Det här scenariot är inte troligt eftersom dess SLA garanterar minst 60 Mbit/s).

I det här fallet kan bzip2 datakomprimering långsammare hello hela pipeline. Växla tooa gzip komprimerings-codec kan underlättar den här begränsningen.

## <a name="sample-scenarios-use-parallel-copy"></a>Exempelscenarier: Använd parallella kopia
**Scenariot I:** kopiera 1 000 MB-1-filer från hello lokalt system tooBlob lagring.

**Analys- och prestandajustering**: T.ex, om du har installerat gateway på en dator i fyra kärnor Data Factory använder 16 parallella kopierar toomove filer från hello fillagring system tooBlob samtidigt. Den här parallellkörning resulterar i högt dataflöde. Du kan också explicit ange hello parallella kopior count. När du kopierar många små filer, hjälpa parallella kopior kraftigt genomströmning effektivare användning av resurser.

![Scenario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scenariot II**: kopiera 20 blobbar 500 MB från Blob storage tooData Lake Store Analytics och justera prestanda.

**Analys- och prestandajustering**: I det här scenariot kopierar Data Factory hello data från Blob storage tooData Lake Store med hjälp av en kopia (**parallelCopies** ange too1) och enkel molndata flytt enheter. hello genomströmning du se blir Stäng toothat som beskrivs i hello [prestanda referensavsnitt](#performance-reference).   

![Scenario 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scenariot III**: enskilda filstorlek är större än dussintals MB och total volym är stor.

**Analys- och aktivera prestanda**: öka **parallelCopies** inte resulterar i bättre copy prestanda på grund av hello resursbegränsningar av ett enda moln dmu här. I stället bör du ange mer molnet DMUs tooget mer resurser tooperform hello dataflyttning. Ange inte ett värde för hello **parallelCopies** egenskapen. Data Factory hanterar hello parallellitet för dig. I det här fallet, om du ställer in **cloudDataMovementUnits** too4, en genomströmning på ungefär fyra gånger inträffar.

![Scenario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referens
Här är övervakning av programprestanda och justera referenser för några av datalager för hello stöds:

* Azure-lagring (inklusive Blob storage och Table storage): [skalbarhetsmål för Azure Storage](../storage/common/storage-scalability-targets.md) och [Azure Storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md)
* Azure SQL Database: Du kan [övervaka hello prestanda](../sql-database/sql-database-single-database-monitor.md) och kontrollera hello databasen transaktion enhet (DTU) procent
* Azure SQL Data Warehouse: Sin förmåga mäts i informationslagerenheter (dwu: er); Se [hantera beräkningskraft i Azure SQL Data Warehouse (översikt)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [prestandanivåer i Azure Cosmos DB](../documentdb/documentdb-performance-levels.md)
* Lokal SQL Server: [övervakaren och finjustera för prestanda](https://msdn.microsoft.com/library/ms189081.aspx)
* Lokala filserver: [prestandajustering för filservrar](https://msdn.microsoft.com/library/dn567661.aspx)
