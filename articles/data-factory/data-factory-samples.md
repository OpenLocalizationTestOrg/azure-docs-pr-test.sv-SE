---
title: aaaAzure Data Factory - exempel
description: "Innehåller information om prover som levereras med hello Azure Data Factory-tjänsten."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: aa1c15eca21b34b7bcc64358b685d7606baaf691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---samples"></a>Azure Data Factory - exempel
## <a name="samples-on-github"></a>Exemplen på GitHub
Hej [GitHub Azure-DataFactory databasen](https://github.com/azure/azure-datafactory) innehåller flera exempel som hjälper dig snabbt ramp in med Azure Data Factory-tjänsten (eller) Ändra hello skript och använda den i egna program. Hej Samples\JSON mappen innehåller JSON kodavsnitt för vanliga scenarier.

| Exempel | Beskrivning |
|:--- |:--- |
| [ADF genomgång](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Det här exemplet innehåller en genomgång för slutpunkt till slutpunkt för bearbetning av loggfiler med hjälp av Azure Data Factory tooturn data från loggfiler i tooinsights. <br/><br/>I den här genomgången hello Data Factory-pipelinen samlar in exempel loggar, processer och förbättra hello data från loggar med referensdata och omvandlar hello data tooevaluate hello effektivitet med en marknadsföringskampanj startades senast. |
| [JSON-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Det här exemplet innehåller JSON-exempel för vanliga scenarier. |
| [HTTP-Installationshämtaren datasampel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |Det här exemplet showcases hämtning av data från en HTTP-slutpunkt tooAzure Blob Storage med hjälp av anpassad .NET-aktivitet. |
| [Mellan AppDomain-punkt Net aktivitet exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Det här exemplet kan du tooauthor en anpassad .NET-aktivitet som inte är begränsad tooassembly-versioner som används av hello ADF programstart (till exempel WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x osv.). |
| [Kör R-skriptet](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Det här exemplet innehåller hello Data Factory anpassad aktivitet som kan använda tooinvoke RScript.exe. Det här exemplet fungerar bara med din egen (inte på begäran) HDInsight-kluster som redan har R installerat på den. |
| [Anropa jobb Spark på HDInsight Hadoop-kluster](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Det här exemplet visas hur toouse MapReduce aktiviteten tooinvoke ett Spark-program. hello kopieras spark bara data från en Azure Blob-behållaren tooanother. |
| [Twitter-analys med Azure Machine Learning bedömningen batchaktiviteten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Det här exemplet visas hur toouse AzureMLBatchScoringActivity tooinvoke en Azure Machine Learning modell som utför twitter sentiment analys, bedömningen förutsägelse osv. |
| [Twitter-analys med anpassad aktivitet](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Det här exemplet visas hur toouse en anpassad .NET aktiviteten tooinvoke en Azure Machine Learning-modell som utför twitter-sentiment analys, bedömningen förutsägelse osv. |
| [Parametriserade Pipelines för Azure Machine Learning](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |hello exemplet innehåller en slutpunkt till slutpunkt C#-kod toodeploy N pipelines för bedömningen och omtränings var och en med en annan region parameter var hello listan över regioner kommer från en parameters.txt-fil som ingår i det här exemplet. |
| [Referens för datauppdatering för Azure Stream Analytics-jobb](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Det här exemplet visas hur toouse Azure Data Factory och Azure Stream Analytics tillsammans toorun hello frågor med referens data och inställningar hello uppdatera för referensdata enligt ett schema. |
| [Hybrid Pipeline med lokala Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |hello används ett lokalt Hadoop-kluster som beräkning mål för att köra jobb i Data Factory precis som du lägger till andra beräknings-mål som ett HDInsight-baserat Hadoop-kluster i molnet. |
| [JSON-konverteringsverktyg](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Det här verktyget kan du tooconvert JSONs från version tidigare too2015-07-01-preview toolatest eller 2015-07-01-preview (standard). |
| [Indatafilen för U-SQL-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Den här filen är en exempelfil som används av en U-SQL-aktivitet. |
| [Ta bort blob-fil](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Det här exemplet innehåller en C#-fil som kan användas som en del av ADF anpassade .net aktiviteten toodelete filer från hello källa Azure Blob-plats när hello filerna har kopierats.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-mallar
Du kan hitta hello följande Azure Resource Manager-mallar för Data Factory på GitHub.

| Mall | Beskrivning |
| --- | --- |
| [Kopiera från Azure Blob Storage tooAzure SQL-databas](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Distribuera den här mallen skapar ett Azure data factory med en rörledning för att kopierar data från hello angetts Azure blob storage toohello Azure SQL-databas |
| [Kopiera från Salesforce tooAzure Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Distribuera den här mallen skapar ett Azure data factory med en rörledning för att kopierar data från hello angetts Salesforce-konto toohello Azure blob storage. |
| [Transformera data genom att köra Hive-skript på Azure HDInsight-kluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Distribuera den här mallen skapar ett Azure data factory med en rörledning som omvandlar data genom att köra Hive-hello exempelskript på ett Azure HDInsight Hadoop-kluster. |

## <a name="samples-in-azure-portal"></a>Exempel på Azure-portalen
Du kan använda hello **exempel pipelines** panelen på hello startsida din data factory toodeploy exempel pipelines och deras associerade enheterna (datauppsättningar och länkade tjänster) i tooyour data factory.

1. Skapa en datafabrik eller öppna en befintlig datafabrik. Se [kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för steg toocreate en datafabrik.
2. I hello **DATAFABRIKEN** bladet för hello data factory klickar du på hello **exempel pipelines** panelen.

    ![Exempel pipelines sida vid sida](./media/data-factory-samples/SamplePipelinesTile.png)
3. I hello **exempel pipelines** bladet, klickar du på hello **exempel** som du vill toodeploy.

    ![Exempel pipelines bladet](./media/data-factory-samples/SampleTile.png)
4. Ange konfigurationsinställningar för hello exemplet. Till exempel ditt Azure storage-konto namn och kontonyckel, Azure SQL-servernamnet, databas, användar-ID och lösenord, osv.

    ![Exempel-bladet](./media/data-factory-samples/SampleBlade.png)
5. När du är klar med att ange hello konfigurationsinställningar, klickar du på **skapa** toocreate/distribuera hello exempel pipelines och länkade tjänster/tabeller som används av hello pipelines.
6. Du ser hello status för distributionen på hello exempel panelen som du tidigare klickade på hello **exempel pipelines** bladet.

    ![Status för distribution](./media/data-factory-samples/DeploymentStatus.png)
7. När du ser hello **distributionen lyckades** meddelandet på hello ikonen för hello exemplet, Stäng hello **exempel pipelines** bladet.  
8. På **DATA FACTORY** bladet som du ser att länkade tjänster, datauppsättningar och pipelines läggs tooyour data factory.  

    ![Bladet Datafabrik](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Exempel i Visual Studio
### <a name="prerequisites"></a>Krav
Du måste ha hello följande installerat på datorn:

* Visual Studio 2013 eller Visual Studio 2015
* Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015. Navigera för[Azure-hämtningssida](https://azure.microsoft.com/downloads/) och på **VS 2013** eller **VS 2015** i hello **.NET** avsnitt.
* Hämta hello senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Om du använder Visual Studio 2013, du kan också uppdatera hello plugin-programmet genom att göra följande hello: på menyn hello **verktyg** -> **tillägg och uppdateringar**  ->  **Online** -> **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio**  ->  **Uppdatering**.

### <a name="use-data-factory-templates"></a>Använd Data Factory-mallar
1. Klicka på **filen** hello, pekar på menyn för**ny**, och klicka på **projekt**.
2. I hello **nytt projekt** dialogrutan rutan, hello följande steg:

   1. Välj **DataFactory** under **mallar**.
   2. Välj **Data Factory mallar** i hello till höger.
   3. Ange en **namn** för hello-projekt.
   4. Välj en **plats** för hello-projekt.
   5. Klicka på **OK**.

      ![Dialogrutan Nytt projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. I hello **Data Factory mallar** dialogrutan, Välj hello exempelmall från hello **användningsfall mallar** avsnittet och klicka på **nästa**. hello följande steg beskriver hur du använder hello **kunden profilering** mall. Stegen är liknande för hello andra exempel.

    ![Dialogrutan för data Factory-mallar](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. I hello **Konfigurationshanteraren för Data Factory** dialogrutan klickar du på **nästa** på hello **Data Factory grunderna** sidan.
5. På hello **konfigurera datafabriken** sidan, hello följande steg:
   1. Välj **skapa nya Data Factory**. Du kan också välja **använda befintlig datafabrik**.
   2. Ange en **namn** för hello data factory.
   3. Välj hello **Azure-prenumeration** som du vill att hello data factory toobe skapas.
   4. Välj hello **resursgruppen** för hello data factory.
   5. Välj hello **västra USA**, **östra USA**, eller **Nordeuropa** för hello **region**.
   6. Klicka på **Nästa**.
6. I hello **konfigurera datalager** anger du en befintlig **Azure SQL database** och **Azure storage-konto** (eller) skapa databaslagring/och klicka på Nästa.
7. I hello **konfigurerar beräknings** , Välj standardvärden och klicka på **nästa**.
8. I hello **sammanfattning** , granska alla inställningar och klicka på **nästa**.
9. I hello **Distributionsstatus** , vänta tills hello distributionen är klar och klicka på **Slutför**.
10. Högerklicka på projektet i hello Solution Explorer och klicka på **publicera**.
11. Om du ser **logga in tooyour Microsoft-konto** dialogrutan, ange dina autentiseringsuppgifter för hello-konto med Azure-prenumeration och på **logga in**.
12. Du bör se följande dialogrutan hello:

    ![Dialogrutan Publicera](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. I hello **konfigurera datafabriken** sidan, hello följande steg:

    1. Bekräfta att **använda befintlig datafabrik** alternativet.
    2. Välj hello **datafabriken** du hade väljer när du använder hello mall.
    3. Klicka på **nästa** tooswitch toohello **publicera objekt** sidan. (Tryck på **FLIKEN** toomove utanför hello namnet fältet tooif hello **nästa** är inaktiverat.)
14. I hello **publicera objekt** , se till att alla hello Datafabriker entiteter är markerade och klickar på **nästa** tooswitch toohello **sammanfattning** sidan.     
15. Granska hello sammanfattning och klicka på **nästa** toostart hello distribution processen och visa hello **Distributionsstatus**.
16. I hello **Distributionsstatus** sida, bör du se hello status hello distributionsprocessen. Klicka på Slutför när hello distributionen är klar.

Se [skapa din första data factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) information om hur du använder Visual Studio tooauthor Data Factory entiteter och publicera dem tooAzure.          
