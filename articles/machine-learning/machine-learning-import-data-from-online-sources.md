---
title: "aaaImport data till Machine Learning Studio från online datakällor | Microsoft Docs"
description: "Hur tooimport utbildningsdata Azure Machine Learning Studio från olika källor för online."
keywords: "Importera data, dataformatet, datatyper, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: aae6907cdd0b4dc373ae08c2569caa276c198b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-hello-import-data-module"></a>Importera data till Azure Machine Learning Studio från olika online datakällor med hello importera Data modul
Den här artikeln beskriver hello stöd för import av online-data från olika källor och hello information behövs toomove data från dessa källor till ett Azure Machine Learning-experiment.

> [!NOTE]
> Den här artikeln innehåller allmän information om hello [importera Data] [ import-data] modul. Mer detaljerad information om hello typer av data som du kan komma åt format, parametrar och svar toocommon frågor finns hello modulen referensavsnittet för hello [importera Data] [ import-data] modul.
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Introduktion
Med hjälp av hello [importera Data] [ import-data] modulen, du kan komma åt data från flera källor för online-data när experimentet körs i [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* En URL med HTTP
* Hadoop med HiveQL
* Azure-blobblagring
* Azure-tabellen
* Azure SQL database eller SQL Server på Azure VM
* Lokal SQL Server-databas
* En datafeed för närvarande OData-providern
* Azure CosmosDB (tidigare kallade DocumentDB)

tooaccess online datakällor i experimentet Studio lägga till hello [importera Data] [ import-data] modulen tooyour, Välj hello **datakällan**, och sedan ange hello-parametrar som behövs tooaccess hello data. hello online datakällor som stöds är specificerade i hello nedan. Den här tabellen sammanfattar också hello-filformat som stöds och parametrar som används tooaccess hello data.

Observera att eftersom den här utbildningsdata används när experimentet är igång, det är bara tillgängliga i försöket. Informationen som har lagrats i en dataset-modul kan däremot tillgängliga tooany experiment på arbetsytan.

> [!IMPORTANT]
> För närvarande hello [importera Data] [ import-data] och [exportera Data] [ export-data] moduler kan läsa och skriva data endast från Azure storage som skapats med hjälp av hello Klassiska distributionsmodellen. Med andra ord hello nya Azure Blob Storage-kontotypen som erbjuder en frekvent åtkomstnivå eller lågfrekvent åtkomstnivå stöds inte ännu. 
> 
> I allmänhet alla Azure storage-konton som du har skapat innan det här alternativet om tjänsten blev tillgängliga påverkas inte. 
> Om du behöver toocreate ett nytt konto, Välj **klassiska** för hello distribution modell, eller använda Resource manager och välj **generella** snarare än **Blob storage** för **Konto kind**. 
> 
> Mer information finns i [Azure Blob Storage: frekvent och lågfrekvent lagringsnivåer](../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Online datakällor som stöds
Azure Machine Learning **importera Data** modulen stöder hello följande datakällor:

| Datakälla | Beskrivning | Parametrar |
| --- | --- | --- |
| URL för Web via HTTP |Läser data i fil med kommaavgränsade värden (CSV), tabbavgränsade värden (TVS), Attributrelation filformatet (ARFF) och Support Vector datorer (SVM lätt)-format från varje webbtjänst-URL som använder HTTP |<b>URL: en</b>: Anger hello fullständigt namn på hello-filen, inklusive hello Webbadress och hello filnamn, med alla tillägg. <br/><br/><b>Dataformatet</b>: Anger en av hello stöds formaterar: CSV TVS, ARFF eller SVM ljus. Hello data har en rubrikrad, är det använda tooassign kolumnnamn. |
| Hadoop/HDFS |Läser data från distribuerade lagringsutrymmen i Hadoop. Anger du hello data genom att använda HiveQL, ett SQL-liknande frågespråk. HiveQL kan också använda tooaggregate data och utföra filtrering innan du lägger till hello data tooMachine Learning Studio. |<b>Hive databasfrågan</b>: Anger hello Hive-fråga används toogenerate hello data.<br/><br/><b>HCatalog server URI </b> : angiven hello namnet på klustret hello formatet  *&lt;klusternamnet&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop användarkontonamnet</b>: Anger hello Hadoop användarkonto namn används tooprovision hello klustret.<br/><br/><b>Hadoop lösenord</b> : Anger hello autentiseringsuppgifter som används vid etablering av hello klustret. Mer information finns i [skapa Hadoop-kluster i HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Platsen för utdata</b>: Anger om hello data lagras i en Hadoop distributed file system (HDFS) eller i Azure. <br/><ul>Om du lagrar utdata i HDFS kan du ange hello HDFS server URI. (Inte till toouse hello HDInsight-klustrets namn hello HTTPS:// prefix). <br/><br/>Om du lagrar dina utdata i Azure måste du ange kontonamn för hello Azure storage, lagringsåtkomstnyckel och namnet på lagringsbehållaren.</ul> |
| SQL-databas |Läser data som lagras i en Azure SQL database eller i en SQL Server-databas som körs på en virtuell Azure-dator. |<b>Databasservernamnet</b>: Anger hello namn hello servern vilken hello databasen körs.<br/><ul>Ange hello servernamn som genereras vid Azure SQL Database. Har vanligtvis hello formuläret  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Vid en SQLServer finns på en virtuell Azure-dator anger *tcp:&lt;virtuella DNS-namnet&gt;, 1433*</ul><br/><b>Databasnamnet </b>: Anger hello namnet på databasen som hello på hello-servern. <br/><br/><b>Servern användarkontonamnet</b>: anger ett användarnamn för ett konto som har behörighet för hello-databasen. <br/><br/><b>Serverlösenord</b>: Anger hello lösenordet för användarkontot för hello.<br/><br/><b>Acceptera alla servercertifikat</b>: Använd det här alternativet (mindre säkert) om du vill tooskip granska hello platscertifikat innan du läser dina data.<br/><br/><b>Databasfrågan</b>: Ange en SQL-instruktion som beskriver hello data som du vill tooread. |
| Lokal SQL-databas |Läser data som lagras i en lokal SQL-databas. |<b>Datagatewayen</b>: Anger hello namn hello Data Management Gateway installeras på en dator där den kan komma åt SQL Server-databasen. Information om hur du konfigurerar hello gateway finns [utför avancerade analyser med Azure Machine Learning med hjälp av data från en lokal SQLServer](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Databasservernamnet</b>: Anger hello namn hello servern vilken hello databasen körs.<br/><br/><b>Databasnamnet </b>: Anger hello namnet på databasen som hello på hello-servern. <br/><br/><b>Servern användarkontonamnet</b>: anger ett användarnamn för ett konto som har behörighet för hello-databasen. <br/><br/><b>Användarnamn och lösenord</b>: Klicka på <b>ange värden</b> tooenter dina autentiseringsuppgifter på databasen. Du kan använda Windows-integrerad autentisering eller beroende på hur dina lokala SQL Server är konfigurerat för SQL Server-autentisering.<br/><br/><b>Databasfrågan</b>: Ange en SQL-instruktion som beskriver hello data som du vill tooread. |
| Azure-tabellen |Läser data från hello tabelltjänsterna i Azure Storage.<br/><br/>Om du läsa stora mängder data mer sällan använda hello Azure Table-tjänsten. Det ger en flexibel och icke-relationella (NoSQL), mycket skalbar, prisvärda och hög tillgänglighet lagringslösning. |Hej alternativ i hello **importera Data** ändras beroende på om du ansluter till offentliga information eller en privat lagringskontot som kräver autentiseringsuppgifter för inloggning. Detta bestäms av hello <b>autentiseringstyp</b> som kan ha värdet ”PublicOrSAS” eller ”konto”, som har en egen uppsättning parametrar. <br/><br/><b>Offentlig eller delad signatur åtkomst (SAS) URI</b>: hello parametrar är:<br/><br/><ul><b>Tabell URI</b>: Anger hello offentliga eller SAS-URL för hello tabell.<br/><br/><b>Anger hello rader tooscan för egenskapsnamn</b>: hello värden är <i>TopN</i> tooscan hello angivet antal rader, eller <i>ScanAll</i> tooget alla rader i tabellen hello. <br/><br/>Om hello data är homogena och förutsägbar, rekommenderar vi att du väljer *TopN* och ange ett nummer för N. För stora tabeller kan detta resultera i snabbare läsning gånger.<br/><br/>Om hello data är strukturerade med uppsättningar med egenskaper som varierar utifrån hello djup och positionen för hello tabell, Välj hello *ScanAll* alternativet tooscan alla rader. Detta säkerställer hello integriteten hos dina resulterande egenskapen och metadata för konvertering.<br/><br/></ul><b>Privata Lagringskonto</b>: hello parametrar är: <br/><br/><ul><b>Kontonamn</b>: Anger hello-konto som innehåller hello tabell tooread hello namn.<br/><br/><b>Kontonyckel</b>: Anger hello lagringsnyckeln som associeras med hello-konto.<br/><br/><b>Tabellnamnet</b> : Anger hello-tabell som innehåller hello data tooread hello namn.<br/><br/><b>Rader tooscan för egenskapsnamn</b>: hello värden är <i>TopN</i> tooscan hello angivet antal rader, eller <i>ScanAll</i> tooget alla rader i tabellen hello.<br/><br/>Om hello data är homogena och förutsägbar, rekommenderar vi att du väljer *TopN* och ange ett nummer för N. För stora tabeller kan detta resultera i snabbare läsning gånger.<br/><br/>Om hello data är strukturerade med uppsättningar med egenskaper som varierar utifrån hello djup och positionen för hello tabell, Välj hello *ScanAll* alternativet tooscan alla rader. Detta säkerställer hello integriteten hos dina resulterande egenskapen och metadata för konvertering.<br/><br/> |
| Azure Blob Storage |Läser data som lagras i hello Blob-tjänsten i Azure Storage, inklusive bilder, Ostrukturerade text eller binära data.<br/><br/>Du kan använda hello Blob-tjänsten toopublicly exponera data eller tooprivately store programdata. Du kan komma åt dina data från var som helst genom att använda HTTP eller HTTPS-anslutningar. |Hej alternativ i hello **importera Data** modul ändras beroende på om du ansluter till offentliga information eller en privat lagringskontot som kräver autentiseringsuppgifter för inloggning. Detta bestäms av hello <b>autentiseringstyp</b> som kan ha ett värde för ”PublicOrSAS” eller ”konto”.<br/><br/><b>Offentlig eller delad signatur åtkomst (SAS) URI</b>: hello parametrar är:<br/><br/><ul><b>URI</b>: Anger hello offentliga eller SAS-URL för hello storage-blob.<br/><br/><b>Filformat</b>: Anger hello dataformatet hello i hello Blob-tjänsten. hello stöds formaten är CSV, TVS och ARFF.<br/><br/></ul><b>Privata Lagringskonto</b>: hello parametrar är: <br/><br/><ul><b>Kontonamn</b>: Anger hello-konto som innehåller hello blob som du vill tooread hello namn.<br/><br/><b>Kontonyckel</b>: Anger hello lagringsnyckeln som associeras med hello-konto.<br/><br/><b>Sökvägen toocontainer, katalogen eller blob </b> : Anger hello blob som innehåller hello data tooread hello namn.<br/><br/><b>BLOB-filformatet</b>: Anger hello dataformatet hello i hello blob-tjänsten. hello är stöds dataformat CSV, TVS, ARFF, CSV med angiven kodning och Excel. <br/><br/><ul>Om hello format är CSV- eller TVS kan vara att tooindicate om hello-filen innehåller en rubrikrad.<br/><br/>Du kan använda hello Excel alternativet tooread data från Excel-arbetsböcker. I hello <i>Excel dataformat</i> alternativ, ange om hello data är i ett intervall för Excel-kalkylblad eller i en Excel-tabell. I hello <i>Excel-kalkylblad eller inbäddad tabell </i>, anger du hello namnet på hello blad eller tabell som du vill använda tooread från.</ul><br/> |
| Data Feed Provider |Läser data från en feed provider som stöds. För närvarande bara hello Open Data Protocol (OData)-formatet stöds. |<b>Data innehållstyp</b>: Anger hello OData-format.<br/><br/><b>Käll-URL</b>: Anger hello fullständig URL för datafeeden hello. <br/>Till exempel hello följande URL-läsningar från hello exempeldatabasen Northwind: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Nästa steg

[Distribuera Azure ML-webbtjänster som använder Data importera och exportera Data moduler](machine-learning-web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
