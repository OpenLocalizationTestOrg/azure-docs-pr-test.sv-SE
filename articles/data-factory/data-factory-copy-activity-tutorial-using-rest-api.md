---
title: "Självstudier: Använd REST API toocreate ett Azure Data Factory-pipelinen | Microsoft Docs"
description: "I den här kursen använder du REST API toocreate ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten toocopy data från ett Azure blob storage en Azure SQL database."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a>Självstudier: Använd REST API toocreate ett Azure Data Factory pipeline toocopy data 
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

I den här artikeln lär du dig hur toouse REST-API toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas. Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Den här artikeln täcker inte alla hello Data Factory REST API. Omfattande dokumentation om Data Factory-cmdlets finns i [referensen för REST-API:et för Data Factory](/rest/api/datafactory/).
>  
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Krav
* Gå igenom [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och fullständig hello **nödvändiga** steg.
* Installera [Curl](https://curl.haxx.se/dlwiz/) på din dator. Verktyget hello Curl med övriga kommandon toocreate en datafabrik. 
* Gör följande genom att följa anvisningarna i [den här artikeln](../azure-resource-manager/resource-group-create-service-principal-portal.md): 
  1. Skapa en webbapp med namnet **ADFCopyTutorialApp** i Azure Active Directory.
  2. Hämta ett **klient-ID** och en **hemlig nyckel**. 
  3. Hämta ett **klientorganisations-ID**. 
  4. Tilldela hello **ADFCopyTutorialApp** programmet toohello **Data Factory deltagare** roll.  
* [Installera Azure PowerShell](/powershell/azure/overview).  
* Starta **PowerShell** och hello följande steg. Behåll Azure PowerShell öppen tills hello slutet av den här kursen. Om du stänga och öppna måste toorun hello kommandon igen.
  
  1. Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen:
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Kör följande kommando tooview hello alla hello prenumerationer för kontot:

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Hello kör följande kommando tooselect hello prenumeration som du vill toowork med. Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello:  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Om hello resursgruppen finns redan, anger du om tooupdate den (Y) eller låta (n). 
     
      Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup. Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.

## <a name="create-json-definitions"></a>Skapa JSON-definitioner
Skapa följande JSON-filer i hello mapp där curl.exe finns. 

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Namnet måste vara globalt unika, så du kanske vill tooprefix-suffix ADFCopyTutorialDF toomake den ett unikt namn. 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Ersätt **accountname** och **accountkey** med namnet och nyckeln för ditt Azure-lagringskonto. toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

Mer information om egenskaper som JSON finns i [Azure Storage länkade tjänster](data-factory-azure-blob-connector.md#azure-storage-linked-service).

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> Ersätt **servername**, **databasename**, **användarnamn**, och **lösenord** med namnet på din Azure SQL-servern, namnet på SQL-databas användare konto och lösenord för hello-kontot.  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

Mer information om egenskaper som JSON finns i [Azure SQL länkade tjänster](data-factory-azure-sql-connector.md#linked-service-properties).

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "AzureStorageLinkedService",
    "typeProperties": {
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ","
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

| Egenskap | Beskrivning |
|:--- |:--- |
| typ | hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage. |
| linkedServiceName | Refererar toohello **AzureStorageLinkedService** som du skapade tidigare. |
| folderPath | Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar. I den här självstudiekursen adftutorial är hello blob-behållaren och mappen är hello rotmappen. | 
| fileName | Den här egenskapen är valfri. Om du utesluter den här egenskapen har alla filer från hello folderPath plockats. I den här självstudiekursen **emp.txt** har angetts för hello filnamn, så att endast filen hämtas för bearbetning. |
| format -> typ |hello indatafilen är i hello-format, så vi använder **TextFormat**. |
| columnDelimiter | hello kolumner i hello indatafilen avgränsas med **kommatecken tecken (`,`)**. |
| frekvens/intervall | hello frekvens har angetts för**timme** och intervallet anges för**1**, vilket innebär att hello indata segment är tillgängliga **varje timme**. Med andra ord hello Data Factory-tjänsten söker efter indata varje timme i hello rotmapp blob-behållaren (**adftutorial**) du har angett. Det ser ut för hello data inom hello pipeline start- och tider, inte före eller efter dessa tider.  |
| extern | Den här egenskapen anges för**SANT** om hello data inte genereras av denna pipeline. hello inkommande data i den här självstudiekursen har hello emp.txt fil, som genereras av denna pipeline, så vi ställa in den här egenskapen tootrue. |

Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "emp"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:

| Egenskap | Beskrivning |
|:--- |:--- |
| typ | hello typegenskapen har ställts in för**AzureSqlTable** eftersom data är kopierade tooa tabell i Azure SQL-databas. |
| linkedServiceName | Refererar toohello **AzureSqlLinkedService** som du skapade tidigare. |
| tableName | Angivna hello **tabell** toowhich hello data kopieras. | 
| frekvens/intervall | hello frekvens har angetts för**timme** och är **1**, vilket innebär att hello utdata segment produceras **varje timme** mellan hello pipeline start och slut tider inte före eller När dessa tider.  |

Det finns tre kolumner – **ID**, **Förnamn**, och **efternamn** – i hello tomma tabellen i hello-databasen. ID: T är en identitetskolumn, så du behöver bara toospecify **Förnamn** och **efternamn** här.

Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).

### <a name="pipelinejson"></a>pipeline.json

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2017-05-11T00:00:00Z",
    "end": "2017-05-12T00:00:00Z"
  }
}
```

Observera följande punkter hello:

- Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**. Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md). I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).
- Indata för hello aktiviteten är inställd för**AzureBlobInput** och utdata för hello aktiviteten är inställd för**AzureSqlOutput**. 
- I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen. En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.  
 
Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag. Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid. Till exempel ”2017-02-03”, vilket motsvarar för ”2017-02-03T00:00:00Z”
 
Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2016-10-14T16:32:41Z. Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen. 
 
Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”. toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.
 
I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.

Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md). Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md). Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md). Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).

## <a name="set-global-variables"></a>Ange globala variabler
Kör följande kommandon när du ersätter hello värdena med dina egna hello i Azure PowerShell:

> [!IMPORTANT]
> Anvisningar för hur du hämtar klient-ID, klienthemlighet, klientorganisations-ID och prenumerations-ID finns i [kravavsnittet](#prerequisites).   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

Kör följande kommando när du har uppdaterat hello namn i hello data factory du använder hello: 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>Autentisera med AAD
Kör följande kommando tooauthenticate med Azure Active Directory (AAD) hello: 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Skapa en datafabrik
I det här steget ska du skapa en Azure Data Factory med namnet **ADFCopyTutorialDF**. En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel lagra en Kopieringsaktiviteten toocopy data från en källa tooa mål. En HDInsight Hive aktiviteten toorun en Hive-skript tootransform inkommande data tooproduct utdata. Kör följande kommandon toocreate hello data factory hello: 

1. Tilldela hello kommandot toovariable med namnet **cmd**. 
   
    > [!IMPORTANT]
    > Bekräfta det hello-namnet i hello data factory som du anger här (ADFCopyTutorialDF) matchar hello namnet som angetts i hello **datafactory.json**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datafabriken har skapats visas hello JSON för hello data factory i hello **resultat**, annars visas ett felmeddelande.  
   
    ```
    Write-Host $results
    ```

Observera följande punkter hello:

* hello namnet på hello Azure Data Factory måste vara globalt unika. Om du ser hello fel i resultat: **datafabriksnamnet ”ADFCopyTutorialDF” är inte tillgänglig**, hello följande steg:  
  
  1. Ändra hello namn (till exempel yournameADFCopyTutorialDF) i hello **datafactory.json** fil.
  2. I hello första kommandot där hello **$cmd** variabeln har tilldelats ett värde, Ersätt ADFCopyTutorialDF med hello nytt namn och kör hello-kommando. 
  3. Kör hello följande två kommandon tooinvoke hello REST API toocreate hello data factory och Skriv ut hello resultaten av hello igen. 
     
     Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
* toocreate Data Factory instanser måste toobe deltagare/administratören av hello Azure-prenumeration
* hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.
* Om felmeddelandet hello ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**”, gör du något av följande hello och försök att publicera igen: 
  
  * Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell: 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen. Den här åtgärden registrerar automatiskt hello-providern för dig.

Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först. Du först skapa länkade tjänster toolink källa och mål data lagras tooyour data lagras. Definiera inkommande och utgående datauppsättningar toorepresent data i länkade datalager. Skapa slutligen hello pipeline med en aktivitet som använder dessa data.

## <a name="create-linked-services"></a>Skapa länkade tjänster
Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.  

Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure storage-konto tooyour data factory. Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet. Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.  

1. Tilldela hello kommandot toovariable med namnet **cmd**. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Skapa en länkad Azure SQL-tjänst
I det här steget kan länka du din Azure SQL database tooyour data factory. Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet. Se [Azure SQL länkade tjänsten](data-factory-azure-sql-connector.md#linked-service-properties) mer information om toodefine för JSON-egenskaper som används för en Azure SQL länkade tjänsten.

1. Tilldela hello kommandot toovariable med namnet **cmd**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Skapa datauppsättningar
I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory. I det här steget definierar du två datamängder som heter AzureBlobInput och AzureSqlOutput som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.

hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning (AzureBlobInput) anger hello-behållaren och hello-mapp som innehåller hello indata.  

På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage. 

### <a name="create-input-dataset"></a>Skapa indatauppsättning
I det här steget skapar du en datauppsättning med namnet AzureBlobInput som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten. Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål. I kursen får ange du ett värde för hello filnamnet. 

1. Tilldela hello kommandot toovariable med namnet **cmd**. 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
hello Azure SQL Database länkade tjänsten anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. hello SQL tabell utdatauppsättningen (OututDataset) du skapar i det här steget anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **AzureBlobInput** som indata och **AzureSqlOutput** som utdata.

Datamängd för utdata är för närvarande vilka enheter hello schema. I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen. hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av hello pipeline. 

1. Tilldela hello kommandot toovariable med namnet **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Kör hello kommando med hjälp av **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Visa hello resultat. Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.  

    ```PowerShell   
    Write-Host $results
    ```

**Grattis!** Du har skapat ett Azure data factory med en rörledning som kopierar data från Azure Blob Storage tooAzure SQL-databas.

## <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget kan använda du Data Factory REST API: et toomonitor segment som produceras av hello pipeline.

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> Kontrollera att hello start- och tider anges i hello efter kommandot Matcha hello start- och sluttider för hello pipeline. 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

Kör hello Invoke-Command och hello nästa tills du ser en sektor i **klar** tillstånd eller **misslyckades** tillstånd. När hello sektorn är i tillståndet Ready, kontrollera hello **tomma** tabell i Azure SQL database för hello utdata. 

Två rader med data från hello källfilen finns kopierade toohello tomma tabellen i hello Azure SQL-databas för varje segment. Därför se 24 nya poster i hello tomma tabellen när alla hello segment har bearbetat (statusen Ready). 

## <a name="summary"></a>Sammanfattning
I den här kursen används REST API toocreate ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas. Här följer hello anvisningar som du utförde i den här kursen:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade **länkade tjänster**:
   1. Ett Azure Storage länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.     
   2. En Azure SQL länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata. 
3. Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.
4. Du skapade en **pipeline** med en kopieringsaktivitet med BlobSource som källa och SqlSink som mottagare. 

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.
