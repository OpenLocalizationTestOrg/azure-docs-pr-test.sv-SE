---
title: "aaaData scenarier som rör Data Lake Store | Microsoft Docs"
description: "Förstå hello olika scenarier och verktyg med vilken data kan inhämtas, bearbetas, hämtas och visualiseras i ett Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: caaa3979b8a2532089778c3e3db3c711714d3c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Med hjälp av Azure Data Lake Store för stordata krav
Det finns fyra viktiga steg i stort databearbetning:

* Vill föra in stora mängder data i datalagret i realtid eller i batchar
* Hello databearbetning
* Hämta hello data
* Visualisera data hello

I den här artikeln titta vi på dessa steg med avseende tooAzure Data Lake Store toounderstand hello verktyg och alternativ tillgängliga toomeet dina behov för stordata.

## <a name="ingest-data-into-data-lake-store"></a>Mata in data i Data Lake Store
Det här avsnittet visar hello olika källor data och hello olika sätt att data som kan inhämtas till ett Data Lake Store-konto.

![Mata in data i Data Lake Store](./media/data-lake-store-data-scenarios/ingest-data.png "mata in data i Data Lake Store")

### <a name="ad-hoc-data"></a>Ad hoc-data
Representerar mindre datauppsättningar som används för prototyper ett stort program. Det finns olika sätt att vill föra in ad hoc-data beroende på hello datakällan hello.

| Datakälla | Mata in den med hjälp av |
| --- | --- |
| Lokal dator |<ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure plattformsoberoende CLI 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Med hjälp av Data Lake-verktyg för Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure Storage Blob |<ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)</li> <li>[AdlCopy-verktyget](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp som körs på HDInsight-kluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Direktuppspelat data
Detta representerar data som genereras av olika källor, till exempel program, enheter, sensorer och så vidare. Dessa data kan inhämtas i ett Data Lake Store av olika verktyg. Dessa verktyg vanligtvis kan samla in och bearbeta hello data för en händelse med händelse i realtid, och sedan skriva hello händelser i batchar till Data Lake Store så att de kan bearbetas ytterligare.

Följande är ett verktyg som du kan använda:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -händelser som inhämtas i Händelsehubbar kan skrivas tooAzure Data Lake med Azure Data Lake Store utdata.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) -du kan skriva direkt tooData Datasjölager från hello Storm-kluster.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – du kan ta emot händelser från Event Hubs och sedan skriva den tooData Lake Store med hjälp av hello [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationella data
Du kan också styra data från relationsdatabaser. Samla in stora mängder data som kan ge viktiga insikter om bearbetas via en pipeline för stordata under en viss tidsperiod relationsdatabaser. Du kan använda följande verktyg toomove hello sådana data i Data Lake Store.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Loggdata för Web server (överföringen med hjälp av anpassade program)
Den här typen av datauppsättning framhävs specifikt eftersom analys av loggdata för web server är ett vanligt användningsfall för stordataprogram och kräver stora volymer av loggen filer toobe upp toohello Data Lake Store. Du kan använda någon av följande verktyg toowrite hello egna skript eller program tooupload sådana data.

* [Azure plattformsoberoende CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

För uppladdning av loggdata för web server, och även för uppladdning av andra typer av data (t.ex. social våra data), är det en bra närmar sig toowrite dina egna anpassade skript/program eftersom det ger dig hello flexibilitet tooinclude data överför-komponent för större stordata programmet. I vissa fall kan den här koden ta hello form av ett skript eller ett enkelt kommandoradsverktyg. I annat fall kanske hello kod används toointegrate stor databehandling i ett affärsprogram eller en lösning.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Data som är associerade med Azure HDInsight-kluster
Klustertyper för de flesta HDInsight (Hadoop, HBase, Storm) stöder Data Lake Store som en databas för lagring av data. HDInsight-kluster komma åt data från Azure Storage BLOB (WASB). För bättre prestanda, kan du kopiera hello data från WASB till ett Data Lake Store-konto som är associerade med hello-kluster. Du kan använda följande verktyg toocopy hello data hello.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy Service](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Data som lagras i lokala eller IaaS Hadoop kluster
Stora mängder data kan lagras i befintliga Hadoop-kluster, lokalt på datorer med hjälp av HDFS. Hej Hadoop-kluster kan vara i en lokal distribution eller kan vara i ett IaaS-kluster i Azure. Det kan finnas krav toocopy sådana data tooAzure Data Lake Store oneoff metod eller ett återkommande sätt. Det finns olika alternativ som du kan använda tooachieve detta. Visas nedan en lista med alternativ och hello associerad avvägningarna.

| Metoden | Information | Fördelar | Överväganden |
| --- | --- | --- | --- |
| Använda Azure Data Factory (ADM) toocopy data direkt från Hadoop kluster tooAzure Data Lake Store |[ADF stöder HDFS som en datakälla](../data-factory/data-factory-hdfs-connector.md) |ADF ger out box-stöd för HDFS och första klass från slutpunkt till slutpunkt-hantering och övervakning |Kräver Data Management Gateway toobe distribuerats lokalt eller i hello IaaS-kluster |
| Exportera data från Hadoop som filer. Kopiera sedan hello filer tooAzure Data Lake Store med hjälp av lämplig mekanism. |Du kan kopiera filer tooAzure Data Lake Store med: <ul><li>[Azure PowerShell för Windows OS](data-lake-store-get-started-powershell.md)</li><li>[Azure plattformsoberoende CLI 2.0 för Windows OS](data-lake-store-get-started-cli-2.0.md)</li><li>Anpassad app med hjälp av alla Data Lake Store SDK</li></ul> |Snabb tooget startas. Kan göra anpassade överföringar |Flera steg som omfattar flera tekniker. Hantering och övervakning kommer att växa toobe en utmaning över tid hello anpassade uppbyggnad hello-verktyg |
| Använd Distcp toocopy data från Hadoop tooAzure lagring. Kopiera data från Azure Storage tooData Lake Store med hjälp av lämplig mekanism. |Du kan kopiera data från Azure Storage tooData Lake Store med hjälp av: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy-verktyget](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp körs på HDInsight-kluster](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Du kan använda verktyg med öppen källkod. |Flera steg som omfattar flera tekniker |

### <a name="really-large-datasets"></a>Mycket stora datauppsättningar
För uppladdning av datauppsättningar som finns i många terabyte intervallet kan hello-metoder som beskrivs ovan ibland vara långsam och dyra. I sådana fall kan använda du hello alternativen nedan.

* **Med hjälp av Azure ExpressRoute**. Azure ExpressRoute kan du skapa privata anslutningar mellan Azure-datacenter och infrastruktur lokalt. Detta ger en tillförlitlig alternativet för att överföra stora mängder data. Mer information finns i [Azure ExpressRoute dokumentation](../expressroute/expressroute-introduction.md).
* **”Offline” att överföra data**. Om du inte är möjligt med hjälp av Azure ExpressRoute av någon anledning, kan du använda [Azure Import/Export service](../storage/common/storage-import-export-service.md) tooship hårddiskar med dina data tooan Azure-datacenter. Dina data är första överförda tooAzure Storage-Blobbar. Du kan sedan använda [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) eller [AdlCopy verktyget](data-lake-store-copy-data-azure-storage-blob.md) toocopy data från Azure Storage BLOB tooData Lake Store.

  > [!NOTE]
  > När du använder hello Import/Export service, bör hello filstorlekar på hello diskar som du levererar tooAzure data center inte vara större än 195 GB.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Bearbeta data som lagras i Data Lake Store
När hello data är tillgängliga i Data Lake Store kan du köra analys på att data med hjälp av hello stöds stordataprogram. För närvarande kan du använda Azure HDInsight och Azure Data Lake Analytics toorun data analysis jobb på hello data som lagras i Data Lake Store.

![Analysera data i Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "analysera data i Data Lake Store")

Du kan titta på hello följande exempel.

* [Skapa ett HDInsight-kluster med Data Lake Store som lagring](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Hämta data från Data Lake Store
Du kanske också vill toodownload eller flytta data från Azure Data Lake Store för scenarier som:

* Flytta data tooother databaser toointerface med din befintliga pipelines för databearbetning. Till exempel du kanske vill toomove data från Data Lake Store tooAzure SQL-databas eller lokala SQL Server.
* Hämta data tooyour lokala datorn för bearbetning i IDE miljöer när du skapar program prototyper.

![Utgående data från Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "utgående data från Data Lake Store")

I sådana fall kan använda du hello följande alternativ:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Du kan också använda följande metoder toowrite hello egna skriptprogram/toodownload data från Data Lake Store.

* [Azure plattformsoberoende CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Visualisera data i Data Lake Store
Du kan använda en blandning av tjänster toocreate visuella representationer av data som lagras i Data Lake Store.

![Visualisera data i Data Lake Store](./media/data-lake-store-data-scenarios/visualize-data.png "visualisera data i Data Lake Store")

* Du kan starta med hjälp av [Azure Data Factory toomove data från Data Lake Store tooAzure SQL Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
* Därefter kan du [integrera Power BI med Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) toocreate visuell representation av hello data.
