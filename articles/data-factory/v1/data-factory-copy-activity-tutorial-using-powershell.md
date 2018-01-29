---
title: "Självstudiekurs: Skapa en pipeline för att flytta data med hjälp av Azure PowerShell | Microsoft-dokument"
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
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 66e054aa13c7a3555758268be676e93ff7a3b5d1
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Självstudiekurs: Skapa en Data Factory-pipeline som flyttar data med hjälp av Azure PowerShell
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Den här artikeln gäller för version 1 av Data Factory, som är allmänt tillgänglig (GA). Läs [copy activity tutorial in version 2 documentation](../quickstart-create-data-factory-powershell.md) (kopiera aktivitetssjälvstudien i dokumentationen för version 2) om du använder version 2 av Data Factory-tjänsten, som finns tillgänglig som förhandsversion. 

I den här artikeln får du lära dig hur du använder PowerShell för att skapa en datafabrik med en pipeline som kopierar data från en Azure-bloblagring till en Azure SQL-databas. Om du inte har använt Azure Data Factory, bör du läsa igenom artikeln [Introduktion till Azure Data Factory](data-factory-introduction.md) innan du genomför den här självstudien.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Kopieringsaktiviteten kopierar data från källans datalager till mottagarens datalager. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt. Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.

En pipeline kan ha fler än en aktivitet. Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE]
> Den här artikeln beskriver inte alla Data Factory-cmdletar. Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om dessa cmdletar.
> 
> Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager. Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster).

## <a name="prerequisites"></a>Nödvändiga komponenter
- Slutför stegen i artikeln [Självstudier – förhandskrav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
- **Installera Azure PowerShell**. Följ instruktionerna i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="steps"></a>Steg
Här är de steg du utför som en del av de här självstudierna:

1. Skapa en Azure-**datafabrik**. I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactoryPSH. 
2. Skapa **länkade tjänster** i den här datafabriken. I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas. 
    
    AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken. Du har skapat en behållare och överfört data till det här lagringskontot som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken. Data som kopieras från blob-lagringen sparas i den här databasen. Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   
3. Skapa **datauppsättningar** för indata och utdata i datafabriken.  
    
    Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto. Och en Azure Blob-datauppsättning anger vilken blobbehållare och mapp som innehåller data.  

    Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas. Och utdatauppsättningen för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.
4. Skapa en **pipeline** i datafabriken. I det här steget kan du skapa en pipeline med en kopieringsaktivitet.   
    
    Kopieringsaktiviteten kopierar data från en Azure-blob till en tabell i Azure SQL-databasen. Du kan använda en kopieringsaktivitet i en pipeline för att kopiera data från alla datakällor som stöds till ett mål som stöds. I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds. 
5. Övervaka pipeline. I det här steget ska du **övervaka** sektorer från indata- och utdatauppsättningar med hjälp av PowerShell.

## <a name="create-a-data-factory"></a>Skapa en datafabrik
> [!IMPORTANT]
> Slutför [förutsättningarna för självstudiekursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan har utfört dessa.   

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Det kan exempelvis vara en kopieringsaktivitet som kopierar data från en källa till ett måldataarkiv och en HDInsight Hive-aktivitet som kör Hive-skript för att transformera indata till produktutdata. Låt oss börja med att skapa datafabriken i det här steget.

1. Starta **PowerShell**. Låt Azure PowerShell vara öppet tills du är klar med självstudien. Om du stänger och öppnar det igen måste du köra kommandona en gång till.

    Kör följande kommando och ange det användarnamn och lösenord som du använder för att logga in i Azure Portal:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Kör följande kommando för att visa alla prenumerationer för det här kontot:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Kör följande kommando för att välja den prenumeration som du vill arbeta med. Ersätt **&lt;NameOfAzureSubscription**&gt; med namnet på din Azure-prenumeration:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Vissa av stegen i den här självstudien förutsätter att du använder resursgruppen med namnet **ADFTutorialResourceGroup**. Om du använder en annan resursgrupp måste du använda den i stället för ADFTutorialResourceGroup i den här självstudiekursen.
3. Kör cmdleten **New-AzureRmDataFactory** och skapa en datafabrik med namnet: **ADFTutorialDataFactoryPSH**:  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    Det här namnet har kanske redan tagits. Därför bör namnet på datafabriken göras unikt genom att lägga till ett prefix eller suffix (till exempel: ADFTutorialDataFactoryPSH05152017) och köra kommandot igen.  

Observera följande punkter:

* Namnet på Azure Data Factory måste vara globalt unikt. Om du får följande fel ändrar du namnet (till exempel dittnamnADFTutorialDataFactoryPSH). Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för information om Data Factory-artefakter.

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* Om du vill skapa Data Factory-instanser måste du vara deltagare/administratör för Azure-prenumerationen.
* Namnet på datafabriken kan registreras som ett DNS-namn i framtiden och blir då synligt offentligt.
* Följande fel kan visas: ”**Prenumerationen har inte registrerats för användning av namnområdet Microsoft.DataFactory.**” Gör något av följande och försök publicera igen:

  * I Azure PowerShell kör du följande kommando för att registrera Data Factory-providern:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Du kan köra följande kommando om du vill kontrollera att Data Factory-providern är registrerad:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Logga in i [Azure Portal](https://portal.azure.com) via Azure-prenumerationen. Gå till ett Data Factory-blad, eller skapa en datafabrik i Azure Portal. Med den här åtgärden registreras providern automatiskt.

## <a name="create-linked-services"></a>Skapa länkade tjänster
Du kan skapa länkade tjänster i en datafabrik för att länka ditt datalager och beräkna datafabrik-tjänster. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). 

Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.  

AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken. Använd det lagringskonto i vilket du skapade en behållare och laddade upp data under [förberedelsestegen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken. Data som kopieras från blob-lagringen sparas i den här databasen. Du har skapat den tomma tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Skapa en länkad tjänst för ett Azure-lagringskonto
I det här steget länkar du ditt Azure-lagringskonto till datafabriken.

1. Skapa en JSON-fil med namnet **AzureStorageLinkedService.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll: (skapa mappen ADFGetStartedPSH om den inte redan finns.)

    > [!IMPORTANT]
    > Ersätt &lt;accountname&gt; och &lt;accountkey&gt; med namnet och nyckeln för ditt Azure-lagringskonto innan du sparar filen. 

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
2. I **Azure PowerShell** växlar du till appen **ADFGetStartedPSH**.
4. Kör cmdleten **New-AzureRmDataFactoryLinkedService** för att skapa den länkade tjänsten: **AzureStorageLinkedService**. Med den här cmdleten och andra Data Factory-cmdlets som du använder i den här självstudien måste du ange värden för parametrarna **ResourceGroupName** och **DataFactoryName**. Du kan också skicka DataFactory-objektet som returnerades av cmdlet:en New-AzureRmDataFactory utan att ange ResourceGroupName och DataFactoryName varje gång du kör en cmdlet. 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    Här är exempel på utdata:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    Andra sätt att skapa den här länkade tjänsten är att ange resursgruppens namn och datafabriksnamnet istället för att ange DataFactory-objektet.  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Skapa en länkad tjänst för en Azure SQL Database
I det här steget länkar du Azure SQL-databasen till din datafabrik.

1. Skapa en JSON-fil med namnet AzureSqlLinkedService.json i mappen C:\ADFGetStartedPSH med följande innehåll:

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
2. Kör följande kommando för att skapa en länkad tjänst:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    Här är exempel på utdata:

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   Bekräfta att inställningen **Tillåt åtkomst till Azure-tjänster** är aktiverad för SQL-databasservern. Gör så här för att kontrollera och aktivera den:

    1. Logga in på [Azure-portalen](https://portal.azure.com)
    2. Klicka på **fler tjänster >** till vänster och klicka på **SQL-servrar** i kategorin **DATABASER**.
    3. Markera din server i listan över SQL-servrar.
    4. Klicka på länken **Visa brandväggsinställningar** på SQL server-bladet.
    5. På bladet **Brandväggsinställningar** klickar du på **På** för **Tillåt åtkomst till Azure-tjänster**.
    6. Klicka på **Spara** i verktygsfältet. 

## <a name="create-datasets"></a>Skapa datauppsättningar
I det föregående steget skapade du kopplade tjänster för att länka ett Azure-lagringskonto och en Azure SQL-databas till datafabriken. I det här steget definierar du två datauppsättningar – InputDataset och OutputDataset – som visar in- och utdata som lagras i de datalager som refereras till av AzureStorageLinkedService och AzureSqlLinkedService.

Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto. Och en indatauppsättning anger vilken blobbehållare och mapp som innehåller indata.  

Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas. Och utdatauppsättningen (OutputDataset) för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen. 

### <a name="create-an-input-dataset"></a>Skapa en indatauppsättning
I det här steget skapar du en datauppsättning med namnet InputDataset som pekar på en blobfil (emp.ext) i rotmappen i en blobbehållare (adftutorial) i Azure Storage som representeras av den länkade tjänsten AzureStorageLinkedService. Om du inte anger ett värde för filnamnet (eller hoppar över det), kommer data från alla blobbar i indatamappen att kopieras till målet. I den här kursen anger du ett värde för filnamnet.  

1. Skapa en JSON-fil med namnet **InputDataset.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:

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

    Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:

    | Egenskap | Beskrivning |
    |:--- |:--- |
    | typ | Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob-lagringen. |
    | linkedServiceName | Refererar till **AzureStorageLinkedService** som du skapade tidigare. |
    | folderPath | Anger vilken **blobbehållare** och **mapp** som innehåller indatablobbar. I den här självstudiekursen adftutorial är blob-behållaren och -mappen rotmappen. | 
    | fileName | Den här egenskapen är valfri. Om du tar bort egenskapen kommer alla filer från folderPath hämtas. I den här självstudiekursen har angetts **emp.txt** som filnamn så att endast den filen hämtas för bearbetning. |
    | format -> typ |Indatafilen är i textformat, så vi använder **TextFormat**. |
    | columnDelimiter | Kolumner i loggfilerna avgränsas med **kommatecken (`,`)**. |
    | frekvens/intervall | Frekvensen är **timme** och intervallet är **1**, vilket innebär att indatasektorerna är tillgängliga en gång i **timmen**. Det betyder att tjänsten Data Factory söker efter indata varje timme i rotmappen för den angivna blobbehållaren (**adftutorial**). Den söker data i pipelinens start- och sluttider och inte före eller efter dessa tider.  |
    | extern | Den här egenskapen anges som **true** om indatan inte skapades av denna pipeline. Inkommande data i den här självstudien finns i filen emp.txt som genereras av denna pipeline, så vi ställer in den här egenskapen på true. |

    Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).
2. Kör följande kommando för att skapa Data Factory-datauppsättningen.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    Här är exempel på utdata:

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
I den här delen av steget ska du skapa en utdatauppsättning med namnet **OutputDataset**. Den här datauppsättningen pekar på en SQL-tabell i Azure SQL-databasen som representeras av **AzureSqlLinkedService**. 

1. Skapa en JSON-fil med namnet **OutputDataset.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:

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

    Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:

    | Egenskap | Beskrivning |
    |:--- |:--- |
    | typ | Typegenskapen är **AzureSqlTable** eftersom data kopieras till en tabell i en Azure SQL-databas. |
    | linkedServiceName | Refererar till **AzureSqlLinkedService** som du skapade tidigare. |
    | tableName | Ange **tabellen** dit data kopieras. | 
    | frekvens/intervall | Frekvensen är inställd på **timme** och intervallet är **1**, vilket innebär att utdatasegment produceras **varje timme** mellan pipelinens start- och sluttider, inte före eller efter dessa tider.  |

    Det finns tre kolumner – **ID**, **FirstName** och **LastName** – i emp-tabellen i databasen. ID är en identitetskolumn, så du anger bara **FirstName** och **LastName** här.

    Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).
2. Skapa datafabriksdatauppsättningen genom att köra följande kommando.

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    Här är exempel på utdata:

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

Schemat styrs för närvarande av utdatamängd. I den här självstudiekursen är datamängden för utdata konfigurerad för att skapa ett segment en gång i timmen. Pipelinen har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av pipeline. 


1. Skapa en JSON-fil med namnet **ADFTutorialPipeline.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
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
    Observera följande punkter:
   
    - I avsnittet Aktiviteter finns det bara en aktivitet vars **typ** anges till **Kopia**. Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten. I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).
    - Indata för aktiviteten är inställd på **InputDataset** och utdata för aktiviteten är inställd på **OutputDataset**. 
    - I avsnittet för **typeProperties** har **BlobSource** angetts som källtyp och **SqlSink** har angetts som mottagartyp. En fullständig lista över datakällor som stöds av kopieringsaktiviteten som källor och mottagare finns i [Datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Klicka på länken i tabellen om du vill veta hur du använder ett visst datalager som stöds som källa/mottagare.  
     
    Ersätt värdet i **start**egenskapen med den aktuella dagen och **slut**värdet med nästa dag. Du kan ange endast datumdelen och hoppa över tidsvärdet. Till exempel ”2016-02-03” som motsvarar ”2016-02-03T00:00:00Z”
     
    Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2016-10-14T16:32:41Z. **Sluttiden** är valfri, men vi använder den i den här självstudiekursen. 
     
    Om du inte anger värdet för **slut**egenskapen, beräknas det som ”**start + 48 timmar**”. Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.
     
    I det föregående exemplet finns det 24 datasektorer eftersom varje datasektor skapas varje timme.

    Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md). Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md). Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md). Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).
2. Kör följande kommando för att skapa datafabrikstabellen.

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    Här är exempel på utdata: 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

**Grattis!** Du har skapat en Azure-datafabrik med en pipeline för att kopiera data från en Azure blob-lagring till en Azure SQL-databas. 

## <a name="monitor-the-pipeline"></a>Övervaka pipeline
I det här steget använder du Azure PowerShell till att övervaka vad som händer i en Azure Data Factory.

1. Ersätt &lt;DataFactoryName&gt; med namnet på din datafabrik och kör **Get-AzureRmDataFactory**. Tilldela en utdatan till en $df-variabel.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    Till exempel:
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    Skriv sedan ut innehållet i $df för att se följande utdata: 
    
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
2. Kör **Get-AzureRmDataFactorySlice** att få information om alla sektorer av **OutputDataset**, vilket är utdatan för pipelinen.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   Den här inställningen måste matcha **Start**-värdet i pipelinens JSON. Du bör se 24 sektorer, en för varje timme från kl. 12:00 den aktuella dagen till 12:00 nästa dag.

   Här följer tre exempelsegment från utdata: 

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
3. Kör **Get-AzureRmDataFactoryRun** för att hämta information om aktiviteten som körs för en **viss** sektor. Kopiera datum-/tid-värde från utdata från det föregående kommandot för att ange värdet för parametern StartDateTime. 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   Här är exempel på utdata: 

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
I den här självstudien har du skapat en Azure-datafabrik som kopierar data från en Azure-blobb till en Azure SQL-databas. Du använde PowerShell till att skapa datafabriken, länkade tjänster, datauppsättningar och en pipeline. Här är de avancerade steg som du utförde i självstudien:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade **länkade tjänster**:

   a. En länkad **Azure Storage**-tjänst som länkar Azure-lagringskontot som innehåller indata.     
   b. En länkad **Azure SQL**-tjänst som länkar den SQL-databas som innehåller utdata.
3. Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.
4. Du skapade en **pipeline** med **Kopiera aktivitet**, med **BlobSource** som källa och **SqlSink** som mottagare.

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. Följande tabell innehåller en lista över datalager som stöds som källor och mål av kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

För mer information om hur du kopierar data till/från ett datalager klickar du på länken för datalagret i tabellen. 

