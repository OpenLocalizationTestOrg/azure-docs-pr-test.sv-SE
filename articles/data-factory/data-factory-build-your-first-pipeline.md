---
title: "Data Factory-Självstudier: första data pipeline | Microsoft Docs"
description: "Den här Azure Data Factory-kursen visar hur toocreate och schemalägga en datafabrik som bearbetar data med hjälp av Hive-skript på ett Hadoop-kluster."
services: data-factory
keywords: "Azure data factory självstudier, hadoop-kluster, hadoop hive"
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: ed9c0ade4500d4ac1f7c2c2312c1fa675e0b1f02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-pipeline-tootransform-data-using-hadoop-cluster"></a>Självstudier: Skapa din första pipeline tootransform data med Hadoop-kluster
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

I den här självstudiekursen skapar du din första Azure data factory med en rörledning för data. hello pipeline omvandlar inkommande data genom att köra Hive-skript på en Azure HDInsight (Hadoop) kluster tooproduce utdata.  

Den här artikeln innehåller översikt och krav för hello kursen. När du har slutfört hello krav kan du göra hello självstudier med något av följande verktyg/SDK hello: Azure-portalen, Visual Studio, PowerShell, Resource Manager-mall, REST-API. Välj en hello alternativ i listrutan för hello hello inledande (eller) länkar hello slutet av den här artikeln toodo hello självstudier med något av följande alternativ.    

## <a name="tutorial-overview"></a>Självstudier – översikt
I den här kursen kan du utföra hello följande steg:

1. Skapa en **datafabriken**. En datafabrik kan innehålla en eller flera pipelines för data som flyttas och transformera data. 

    I den här självstudiekursen skapar du en pipeline i hello data factory. 
2. Skapa en **pipeline**. En pipeline kan ha en eller flera aktiviteter (exempel: Kopieringsaktiviteten, HDInsight Hive aktivitet). Det här exemplet använder hello HDInsight Hive-aktivitet som kör en Hive-skript på ett HDInsight Hadoop-kluster. hello skriptet skapar en tabell som refererar till hello rådata web loggdata som lagras i Azure blob storage först och sedan partitioner hello rådata per år och månad.

    I den här kursen använder hello pipeline hello Hive tootransform aktivitetsdata genom att köra en Hive-fråga på ett Azure HDInsight Hadoop-kluster. 
3. Skapa **länkade tjänster**. Du skapar en länkad tjänst toolink ett datalager eller en beräkning service toohello data factory. Ett datalager, till exempel Azure Storage innehåller in-/ utdata-data för aktiviteter i hello pipeline. En tjänst för beräkning, till exempel HDInsight Hadoop-kluster processer/transformeringar data.

    I den här självstudiekursen skapar du två länkade tjänster: **Azure Storage** och **Azure HDInsight**. hello Azure Storage länkade tjänsten länkar ett Azure Storage-konto som innehåller hello indata/utdata toohello data factory. Azure HDInsight länkade tjänsten länkar ett Azure HDInsight-kluster som används tootransform data toohello data factory. 
3. Skapa ingående och utgående **datauppsättningar**. En inkommande datauppsättning representerar hello indata för en aktivitet i pipelinen hello och en datamängd för utdata representerar hello utdata för hello-aktivitet.

    I den här självstudiekursen, hello inkommande och utgående datauppsättningar ange platser för indata och utdata i hello Azure Blob Storage. hello länkad Azure Storage-tjänst anger vad Azure Storage-konto används. En inkommande datauppsättning anger hello indatafiler finns och en datamängd för utdata anger var hello utdatafilerna finns där. 


Se [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel en detaljerad översikt över Azure Data Factory.
  
Här är hello **diagramvy** i hello exempel data factory du skapar i den här kursen. **MyFirstPipeline** har en aktivitet av typen Hive som förbrukar **AzureBlobInput** dataset som indata och ger **AzureBlobOutput** dataset som utdata. 

![Diagramvy i självstudiekursen Data Factory](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


I den här självstudiekursen **inputdata** för hello **adfgetstarted** Azure blob-behållare innehåller en fil med namnet input.log. Loggfilen har poster från tre månader: januari, februari och mars 2016. Här följer hello exempel rader för varje månad i hello indatafilen. 

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

När hello fil bearbetas av hello pipeline med HDInsight Hive aktivitet hello aktiviteten körs en Hive-skript på hello HDInsight-kluster att partitioner indata per år och månad. hello skriptet skapar tre utdata mapparna som innehåller en fil med poster från varje månad.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Från hello exempel rader som visas ovan, hello först en (med 2016-01-01) skrivs toohello 000000_0 filen hello månaden = 1 mapp. På liknande sätt hello andra skrivs toohello filen hello månaden = 2 mapp och hello tredje en skrivs toohello filen hello månaden = 3 mapp.  

## <a name="prerequisites"></a>Krav
Innan du börjar den här kursen måste du ha hello följande krav:

1. **Azure-prenumeration** -om du inte har en Azure-prenumeration kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Se hello [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/) artikel om hur du kan skaffa ett kostnadsfritt utvärderingskonto.
2. **Azure Storage** – du använder en generell Azure storage-standardkonto för lagring av hello data i den här självstudiekursen. Om du inte har en generell Azure storage-standardkonto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel. När du har skapat hello storage-konto, notera hello **kontonamn** och **åtkomstnyckeln**. Se [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).
3. Hämta och granska hello Hive frågefilen (**HQL**) finns på: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Den här frågan transformerar indata tooproduce utdata. 
4. Hämta och granska hello exempel indatafilen (**input.log**) finns på: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Skapa en blobbbehållare med namnet **adfgetstarted** i Azure Blob Storage. 
6. Överför **partitionweblogs.hql** filen toohello **skriptet** mapp i hello **adfgetstarted** behållare. Använd verktyg som [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/). 
7. Överför **input.log** filen toohello **inputdata** mapp i hello **adfgetstarted** behållare. 

Välj något av följande verktyg/SDK toodo hello kursen hello när du har slutfört hello krav: 

- [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure-portalen och Visual Studio tillhandahåller GUI sätt för att skapa din datafabriker. Medan PowerShell, Resource Manager-mall och REST-API ger scripting/programmering sätt för att skapa din datafabriker.

> [!NOTE]
> hello data pipeline i den här handledningen omvandlar indata tooproduce utdata. Den kopierar inte data från en källa data store tooa målarkiv data. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory). 





  
