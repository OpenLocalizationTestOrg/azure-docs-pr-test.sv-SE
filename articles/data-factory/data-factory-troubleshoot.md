---
title: aaaTroubleshoot Azure Data Factory-problem
description: "Lär dig hur tootroubleshoot problem med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a>Felsök Data Factory-problem
Den här artikeln innehåller felsökningstips för problem när du använder Azure Data Factory. Den här artikeln innehåller inte alla hello möjliga problem när du använder hello service, men omfattar vissa problem och allmänna felsökningstips.   

## <a name="troubleshooting-tips"></a>Felsökningstips
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a>Fel: hello prenumerationen är inte registrerad toouse namnområdet 'Microsoft.DataFactory'
Om du får detta felmeddelande registrerats hello Azure Data Factory-resursprovidern inte på datorn. Hej du följande:

1. Starta Azure PowerShell.
2. Logga in tooyour Azure-konto med hjälp av följande kommando hello.

    ```powershell
    Login-AzureRmAccount
    ```
3. Kör hello efter kommandot tooregister hello Azure Data Factory-providern.

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problem: Obehörig fel när du kör en Data Factory-cmdlet
Du använder antagligen inte hello höger Azure-konto eller prenumeration med hello Azure PowerShell. Använd hello följande cmdlets tooselect hello höger Azure-konto och prenumeration toouse med hello Azure PowerShell.

1. Login-AzureRmAccount - Använd hello rätt användar-ID och lösenord
2. Get-AzureRmSubscription – visa alla hello prenumerationer för hello-kontot.
3. SELECT-AzureRmSubscription &lt;prenumerationsnamn&gt; -Välj rätt hello-prenumeration. Använd hello samma du använder toocreate en datafabrik på hello Azure-portalen.

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a>Problem: Misslyckas toolaunch Express installation av Data Management Gateway från Azure-portalen
hello Express-installationen för hello Data Management Gateway kräver Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce. Om hello snabbinstallationen misslyckas toostart, gör du något av följande hello:

* Använd Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.

    Om du använder Chrome, gå toohello [Chrome web store](https://chrome.google.com/webstore/), söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.

    Hello samma för Firefox (installera tillägget). Klicka på Öppna menyn i verktygsfältet hello (tre horisontella raderna i hello längst upp till höger), klickar du på tilläggskomponenter, söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.
* Använd hello **manuell installation** länk som visas på hello samma bladet i hello portal. Du använder den här metoden toodownload installationsfilen och kör den manuellt. När hello-installationen har slutförts visas dialogrutan för hello Data Management Gateway-konfigurationen. Kopiera hello **nyckeln** från portalen hello-skärmen och använder den i hello configuration manager toomanually registrera hello gateway med hello-tjänsten.  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a>Problem: Misslyckas tooconnect tooon lokala SQL Server
Starta **Data Management Gateway Configuration Manager** hello gateway-datorn och använda hello **felsökning** fliken tootest hello anslutning tooSQL Server från hello gateway-datorn. Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problem: Indata segment som finns i väntan på tillstånd till någonsin
hello segment kan vara i **väntar på** tillstånd på grund av toovarious skäl. En av hello vanliga orsaker är att hello **externa** egenskapen har inte angetts för**SANT**. En datamängd som är producerade utanför hello omfånget för Azure Data Factory bör markeras med **externa** egenskapen. Den här egenskapen anger att hello data är extern och inte säkerhetskopieras av några pipelines i hello data factory. Hej datasektorer är markerade som **klar** när hello data är tillgängliga i hello respektive store.

Se följande exempel för hello användning av hello hello **externa** egenskapen. Du kan också ange **externalData*** om du ställer in externa tootrue.

Se [datauppsättningar](data-factory-create-datasets.md) artikeln för mer information om den här egenskapen.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

tooresolve Hej fel, Lägg till hello **externa** egenskap och hello valfria **externalData** avsnittet toohello JSON-definitionen av hello indatatabellen och återskapa hello tabell.

### <a name="problem-hybrid-copy-operation-fails"></a>Problem: Hybrid inte går att kopiera
Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) för tootroubleshoot problem med att kopiera till/från en lokal lagring med hjälp av stegen hello Data Management Gateway.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problem: På begäran HDInsight etablerar misslyckas
När du använder en länkad tjänst av typen HDInsightOnDemand måste toospecify en linkedServiceName som pekar tooan Azure Blob Storage. Data Factory-tjänsten använder den här lagring toostore loggar och stödfiler för ditt HDInsight-kluster på begäran.  Ibland är etableringen av ett HDInsight-kluster på begäran misslyckas med hello följande fel:

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Det här felet tyder vanligtvis på att hello hello storage-konto som angetts i hello linkedServiceName inte lagras i hello samma Datacenter plats där hello HDInsight etablerar sker. Exempel: om din data factory finns i USA, västra och hello Azure storage i östra USA, hello på begäran etablering misslyckas i USA, västra.

Dessutom finns det en andra JSON-egenskap, additionalLinkedServiceNames, där ytterligare lagringskonton kan anges i HDInsight på begäran. Dessa ytterligare länkade storage-konton måste vara i hello samma plats som hello HDInsight-kluster, eller så misslyckas med hello samma fel.

### <a name="problem-custom-net-activity-fails"></a>Problem: Anpassad .NET-aktiviteten misslyckas
Se [felsöka en pipeline med anpassad aktivitet](data-factory-use-custom-activities.md#troubleshoot-failures) detaljerade anvisningar.

## <a name="use-azure-portal-tootroubleshoot"></a>Använda Azure portal tootroubleshoot
### <a name="using-portal-blades"></a>Med hjälp av portalen blad
Se [övervakaren pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) anvisningar.

### <a name="using-monitor-and-manage-app"></a>Övervaka och hantera app
Se [övervaka och hantera data factory pipelines med övervaka och hantera appen](data-factory-monitor-manage-app.md) mer information.

## <a name="use-azure-powershell-tootroubleshoot"></a>Använda Azure PowerShell tootroubleshoot
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a>Använda Azure PowerShell tootroubleshoot ett fel
Se [övervakaren Data Factory rörledningar med hjälp av Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) mer information.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
