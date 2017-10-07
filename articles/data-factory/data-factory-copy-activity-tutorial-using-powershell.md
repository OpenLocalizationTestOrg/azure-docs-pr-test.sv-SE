---
title: "Självstudier: Skapa en pipeline toomove data med hjälp av Azure PowerShell | Microsoft Docs"
description: "I den här självstudiekursen kommer du att skapa en Azure Data Factory-pipeline med en kopieringsaktivitet genom att använda Azure PowerShell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Självstudiekurs: Skapa en Data Factory-pipeline som flyttar data med hjälp av Azure PowerShell
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

I den här artikeln får du lära dig hur toouse PowerShell toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas. Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Den här artikeln täcker inte alla hello Data Factory-cmdletar. Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om dessa cmdletar.
> 
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Krav
- Slutföra förutsättningar som anges i hello [förutsättningar för självstudien](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel.
- **Installera Azure PowerShell**. Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell](../powershell-install-configure.md).

## <a name="steps"></a>Steg
Här är hello steg du utför som en del av den här kursen:

1. Skapa en Azure-**datafabrik**. I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactoryPSH. 
2. Skapa **länkade tjänster** i hello data factory. I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas. 
    
    Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   
3. Skapa ingående och utgående **datauppsättningar** i hello data factory.  
    
    hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.  

    På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.
4. Skapa en **pipeline** i hello data factory. I det här steget kan du skapa en pipeline med en kopieringsaktivitet.   
    
    Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas. Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet. I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds. 
5. Övervakaren hello pipeline. I det här steget du **övervakaren** hello segment av inkommande och utgående datamängder med hjälp av PowerShell.

## <a name="create-a-data-factory"></a>Skapa en datafabrik
> [!IMPORTANT]
> Fullständig [förutsättningar för självstudiekursen hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan gjort det..   

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata. Låt oss börja med att skapa hello data factory i det här steget.

1. Starta **PowerShell**. Behåll Azure PowerShell öppen tills hello slutet av den här kursen. Om du stänga och öppna måste toorun hello kommandon igen.

    Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Kör följande kommando tooview hello alla hello prenumerationer för kontot:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Hello kör följande kommando tooselect hello prenumeration som du vill toowork med. Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando hello:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Några av hello stegen i den här självstudiekursen förutsätter att du använder hello resursgrupp med namnet **ADFTutorialResourceGroup**. Om du använder en annan resursgrupp måste toouse den i stället för ADFTutorialResourceGroup i den här självstudiekursen.
3. Kör hello **ny AzureRmDataFactory** cmdlet toocreate en datafabrik som heter **ADFTutorialDataFactoryPSH**:  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    Det här namnet har kanske redan tagits. Därför att hello namn i hello data factory unika genom att lägga till ett prefix eller suffix (till exempel: ADFTutorialDataFactoryPSH05152017) och kör hello kommandot igen.  

Observera följande punkter hello:

* hello namn i hello Azure data factory måste vara globalt unika. Om du får följande fel hello ändra hello namn (till exempel yournameADFTutorialDataFactoryPSH). Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för information om Data Factory-artefakter.

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* toocreate Data Factory instanser måste du vara en deltagare eller en administratör för hello Azure-prenumeration.
* hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.
* Du kan få hello följande fel ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory.**” Gör något av följande hello och försök att publicera igen:

  * Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Kör följande kommando tooconfirm hello som hello Data Factory providern är registrerad:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Logga in med hjälp av hello Azure-prenumeration toohello [Azure-portalen](https://portal.azure.com). Gå tooa Data Factory-bladet, eller skapa en datafabrik i hello Azure-portalen. Den här åtgärden registrerar automatiskt hello-providern för dig.

## <a name="create-linked-services"></a>Skapa länkade tjänster
Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). 

Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.  

Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Skapa en länkad tjänst för ett Azure-lagringskonto
I det här steget kan länka du din Azure storage-konto tooyour data factory.

1. Skapa en JSON-fil med namnet **AzureStorageLinkedService.json** i **C:\ADFGetStartedPSH** mapp med hello följande innehåll: (skapa hello mapp ADFGetStartedPSH om den inte redan finns).

    > [!IMPORTANT]
    > Ersätt &lt;accountname&gt; och &lt;accountkey&gt; med namnet och nyckeln för ditt Azure storage-konto innan du sparar hello-filen. 

    ```json
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
2. I **Azure PowerShell**, växla toohello **ADFGetStartedPSH** mapp.
4. Kör hello **ny AzureRmDataFactoryLinkedService** cmdlet toocreate hello länkade tjänsten: **AzureStorageLinkedService**. Denna cmdlet och andra Data Factory-cmdletar som du använder i den här kursen kräver att du toopass värden för hello **ResourceGroupName** och **DataFactoryName** parametrar. Du kan också skicka hello DataFactory objektet som returnerades av hello ny AzureRmDataFactory cmdlet utan att ange ResourceGroupName och DataFactoryName varje gång du kör en cmdlet. 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    Här är exempel på utdata från hello:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    Andra sätt att skapa den här länkade tjänsten är toospecify resursgruppens namn och datafabriksnamnet istället för att ange hello DataFactory objekt.  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Skapa en länkad tjänst för en Azure SQL Database
I det här steget kan länka du din Azure SQL database tooyour data factory.

1. Skapa en JSON-fil som heter AzureSqlLinkedService.json i C:\ADFGetStartedPSH mappen med hello följande innehåll:

    > [!IMPORTANT]
    > Ersätt &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; och &lt;password&gt; med namnen för Azure SQL-servern, databasen, användarkontot och lösenordet.
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. Kör följande kommando toocreate hello en länkad tjänst:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    Här är exempel på utdata från hello:

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   Bekräfta att **och ge åtkomst tooAzure tjänster** inställningen är aktiverad för SQL-databasservern. tooverify och den aktiveras, hello följande steg:

    1. Logga in toohello [Azure-portalen](https://portal.azure.com)
    2. Klicka på **fler tjänster >** hello vänster och klicka på **SQL-servrar** i hello **databaser** kategori.
    3. Markera din server i hello lista över SQL-servrar.
    4. Klicka på hello SQL server-bladet **visa brandväggsinställningar** länk.
    5. I hello **brandväggsinställningar** bladet, klickar du på **ON** för **och ge åtkomst tooAzure tjänster**.
    6. Klicka på **spara** hello i verktygsfältet. 

## <a name="create-datasets"></a>Skapa datauppsättningar
I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory. I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.

hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.  

På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage. 

### <a name="create-an-input-dataset"></a>Skapa en indatauppsättning
I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten. Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål. I kursen får ange du ett värde för hello filnamnet.  

1. Skapa en JSON-fil med namnet **InputDataset.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:

    ```json
    {
        "name": "InputDataset",
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
                "fileName": "emp.txt",
                "folderPath": "adftutorial/",
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
2. Kör följande kommando toocreate hello Data Factory dataset hello.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    Här är exempel på utdata från hello:

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a>Skapa en datauppsättning för utdata
I den här delen av hello steget skapar du en datamängd för utdata som heter **OutputDataset**. Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService**. 

1. Skapa en JSON-fil med namnet **OutputDataset.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:

    ```json
    {
        "name": "OutputDataset",
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
2. Kör följande kommando toocreate hello data factory dataset hello.

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    Här är exempel på utdata från hello:

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a>Skapa en pipeline
I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.

Datamängd för utdata är för närvarande vilka enheter hello schema. I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen. hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av hello pipeline. 


1. Skapa en JSON-fil med namnet **ADFTutorialPipeline.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
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
    - Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**. 
    - I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen. En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.  
     
    Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag. Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid. Till exempel ”2016-02-03”, vilket motsvarar för ”2016-02-03T00:00:00Z”
     
    Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2016-10-14T16:32:41Z. Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen. 
     
    Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”. toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.
     
    I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.

    Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md). Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md). Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md). Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).
2. Kör följande kommando toocreate hello data factory-tabell hello.

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    Här är exempel på utdata från hello: 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

**Grattis!** Du har skapat ett Azure data factory med en rörledning toocopy data från Azure blob storage tooan Azure SQL-databas. 

## <a name="monitor-hello-pipeline"></a>Övervakaren hello pipeline
I det här steget använder du Azure PowerShell toomonitor vad som händer i ett Azure data factory.

1. Ersätt &lt;DataFactoryName&gt; med hello namn för din data factory och kör **Get-AzureRmDataFactory**, och tilldela hello utdata tooa variabeln $df.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    Exempel:
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    Kör sedan skriva ut hello innehållet i $df toosee hello följande utdata: 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. Kör **Get-AzureRmDataFactorySlice** tooget information om alla segment av hello **OutputDataset**, vilket är hello utdatauppsättningen för hello pipeline.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   Den här inställningen ska matcha hello **starta** värdet i hello pipeline-JSON. Du bör se 24 segment, en för varje timme från 12: 00 av hello dagens too12 är av hello nästa dag.

   Här följer tre exempel segment från hello utdata: 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Kör **Get-AzureRmDataFactoryRun** tooget hello information om aktivitet körs för en **specifika** sektorn. Kopiera hello datetime-värde från hello utdata från hello föregående kommando toospecify hello värde för hello StartDateTime parameter. 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   Här är exempel på utdata från hello: 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om Data Factory-cmdletar.

## <a name="summary"></a>Sammanfattning
I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas. Du kan använda PowerShell toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline. Här följer hello anvisningar som du utförde i den här kursen:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade **länkade tjänster**:

   a. En **Azure Storage** länkade tjänsten toolink ditt Azure storage-konto som innehåller data som indata.     
   b. En **Azure SQL** länkade tjänsten toolink din SQL-databas som innehåller hello utdata.
3. Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.
4. Skapa en **pipeline** med **Kopieringsaktiviteten**, med **BlobSource** som hello källa och **SqlSink** som hello mottagare.

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell. 

