---
title: "Självstudier: Skapa ett Azure Data Factory pipeline toocopy data (Azure portal) | Microsoft Docs"
description: "I den här kursen använder du Azure portal toocreate ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten toocopy data från Azure blob storage tooan Azure SQL-databas."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fadd840fe9a15cd8831cdb25dccbd10ac42fa161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-portal-toocreate-a-data-factory-pipeline-toocopy-data"></a>Självstudier: Använda Azure portal toocreate Data Factory pipeline toocopy data 
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

I den här artikeln får du lära dig hur toouse [Azure-portalen](https://portal.azure.com) toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas. Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Krav
Slutföra förutsättningar som anges i hello [förutsättningar för självstudien](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel innan du utför den här kursen.

## <a name="steps"></a>Steg
Här är hello steg du utför som en del av den här kursen:

1. Skapa en Azure-**datafabrik**. I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactory. 
2. Skapa **länkade tjänster** i hello data factory. I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas. 
    
    Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   
3. Skapa ingående och utgående **datauppsättningar** i hello data factory.  
    
    hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.  

    På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.
4. Skapa en **pipeline** i hello data factory. I det här steget kan du skapa en pipeline med en kopieringsaktivitet.   
    
    Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas. Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet. I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds. 
5. Övervakaren hello pipeline. I det här steget du **övervakaren** hello segment av inkommande och utgående datamängder med hjälp av Azure-portalen. 

## <a name="create-data-factory"></a>Skapa en datafabrik
> [!IMPORTANT]
> Fullständig [förutsättningar för självstudiekursen hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan gjort det..   

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata. Låt oss börja med att skapa hello data factory i det här steget.

1. När du loggar in toohello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**. 
   
   ![Nytt->DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. I hello **nya data factory** bladet:
   
   1. Ange **ADFTutorialDataFactory** för hello **namn**. 
      
         ![Bladet Ny datafabrik](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       hello hello Azure data factory måste vara **globalt unika**. Om du får följande fel hello ändra hello namn hello data factory (till exempel yournameADFTutorialDataFactory) och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Datafabriksnamnet är inte tillgängligt](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Välj din Azure **prenumeration** som du vill toocreate hello data factory. 
   3. För hello **resursgruppen**, gör du något av följande hello:
      
      - Välj **använda befintliga**, och välj en befintlig resursgrupp hello nedrullningsbara listan. 
      - Välj **Skapa nytt**, och ange hello namnet på en resursgrupp.   
         
          Några av hello stegen i den här självstudiekursen förutsätts att du använder hello namn: **ADFTutorialResourceGroup** för hello resursgrupp. toolearn om resursgrupper finns [resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-overview.md).  
   4. Välj hello **plats** för hello data factory. Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.
   5. Välj **PIN-kod toodashboard**.     
   6. Klicka på **Skapa**.
      
      > [!IMPORTANT]
      > toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.
      > 
      > hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.                
      > 
      > 
3. Hello instrumentpanelen visas hello följande panelen med status: **distribuera data factory**. 

    ![panelen distribuerar datafabrik](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. När hello har skapats visas hello **Data Factory** bladet enligt hello bild.
   
   ![Datafabrikens startsida](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Skapa länkade tjänster
Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). 

Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.  

Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Skapa en länkad Azure-lagringstjänst
I det här steget kan länka du din Azure storage-konto tooyour data factory. Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet.  

1. I hello **Datafabriken** bladet, klickar du på **författare och distribuera** panelen.
   
   ![Ikonen Författare och distribution](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. Du ser hello **Data Factory-redigeraren** som visas i följande bild hello: 

    ![Data Factory-redigeraren](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. I Redigeraren för hello, klickar du på **Nytt datalager** hello verktygsfältet och välj knappen **Azure storage** hello nedrullningsbara menyn. Du bör se hello JSON-mall för att skapa en länkad Azure storage-tjänst i hello till höger. 
   
    ![Redigerarens knapp Nytt datalager](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Ersätt `<accountname>` och `<accountkey>` med hello konto namn och värden för Azure storage-konto. 
   
    ![JSON för redigerarens blobblagring](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Klicka på **distribuera** hello i verktygsfältet. Du bör se hello distribueras **AzureStorageLinkedService** i trädet hello nu visa. 
   
    ![Distribuera redigerarens blobblagring](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    Läs mer om JSON-egenskaper i länkade hello tjänstdefinitionen [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel.

### <a name="create-a-linked-service-for-hello-azure-sql-database"></a>Skapa en länkad tjänst för hello Azure SQL Database
I det här steget kan länka du din Azure SQL database tooyour data factory. Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet. 

1. I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** hello verktygsfältet och välj knappen **Azure SQL Database** hello nedrullningsbara menyn. Du bör se hello JSON-mall för att skapa hello Azure SQL-länkade tjänsten i hello till höger.
2. Ersätt `<servername>`, `<databasename>`, `<username>@<servername>` och `<password>` med namnen på din Azure SQL-server, databas, ditt användarkonto och lösenord. 
3. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **AzureSqlLinkedService**.
4. Bekräfta att du ser **AzureSqlLinkedService** i hello trädvy under **länkade tjänster**.  

    Mer information om de här JSON-egenskaperna finns i [Anslutningsapp för Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="create-datasets"></a>Skapa datauppsättningar
I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory. I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.

hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.  

På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage. 

### <a name="create-input-dataset"></a>Skapa indatauppsättning
I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten. Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål. I kursen får ange du ett värde för hello filnamnet. 

1. I hello **Editor** hello Data Factory, klickar du på **... Flera**, klickar du på **ny datamängd**, och klicka på **Azure Blob storage** hello nedrullningsbara menyn. 
   
    ![Menyn Ny datauppsättning](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Ersätt JSON i hello högra med hello följande JSON-fragment: 
   
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
3. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset** dataset. Bekräfta att du ser hello **InputDataset** i hello trädvy.

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
hello Azure SQL Database länkade tjänsten anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. hello SQL tabell utdatauppsättningen (OututDataset) du skapar i det här steget anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.

1. I hello **Editor** hello Data Factory, klickar du på **... Flera**, klickar du på **ny datamängd**, och klicka på **Azure SQL** hello nedrullningsbara menyn. 
2. Ersätt JSON i hello högra med hello följande JSON-fragment:

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
3. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **OutputDataset** dataset. Bekräfta att du ser hello **OutputDataset** i hello trädvy under **datauppsättningar**. 

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.

Datamängd för utdata är för närvarande vilka enheter hello schema. I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen. hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av hello pipeline. 

1. I hello **Editor** hello Data Factory, klickar du på **... More (Mer)** och sedan på **Ny pipeline**. Du kan också högerklicka på **Pipelines** i hello trädvyn och klicka på **ny pipeline**.
2. Ersätt JSON i hello högra med hello följande JSON-fragment: 

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
    - Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2016-10-14T16:32:41Z. Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen. Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”. toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.
     
    I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.

    Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md). Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md). Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md). Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).
3. Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **ADFTutorialPipeline**. Bekräfta att du ser hello pipeline i hello trädvyn. 
4. Stäng nu hello **Editor** bladet genom att klicka på **X**. Klicka på **X** igen toosee hello **Datafabriken** startsidan för hello **ADFTutorialDataFactory**.

**Grattis!** Du har skapat ett Azure data factory med en rörledning toocopy data från Azure blob storage tooan Azure SQL-databas. 


## <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget använder du hello Azure portal toomonitor vad som händer i ett Azure data factory.    

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Övervaka pipeline med övervaknings- och hanteringsappen
hello visar följande steg hur toomonitor rörledningar i din data factory med hello övervaka och hantera program: 

1. Klicka på **övervaka och hantera** panelen på hello startsidan för din data factory.
   
    ![Ikonen Övervaka och hantera](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Du bör se programmet **Övervaka och hantera** i en separat flik. 

    > [!NOTE]
    > Om du ser att hello webbläsare har ”auktorisera...”, gör du något av följande hello: Rensa hello **blockerar cookies från tredje part och platsdata** kryssrutan (eller) skapa ett undantag för **login.microsoftonline.com**, och försök sedan tooopen hello appen igen.

    ![Appen Övervaka och hantera](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. Ändra hello **starttid** och **sluttiden** tooinclude start (2017-05-11) och sluttider (2017-05-12) för din pipeline och på **tillämpa**.     
3. Du ser hello **aktivitet windows** som är associerade med varje timme mellan pipeline start och slut gånger i listan för hello hello mellersta rutan. 
4. toosee information om en aktivitetsfönstret väljer hello aktivitetsfönstret i hello **aktivitet Windows** lista. 
    ![Aktivitetsfönsterinformation](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

    Aktiviteten fönstret Utforskaren på hello rätt visas i den hello sektorer toohello aktuella UTC-tid (8:12:00) bearbetas (med grön färg). hello 8 – 9 PM, 9-10 PM, 10-11 PM, 11 PM - 12: 00 segment bearbetas inte ännu.

    Hej **försök** avsnitt i hello högra fönstret innehåller information om hello aktiviteten kör för hello datasektorn. Om ett fel uppstod, innehåller information om hello-fel. Till exempel om hello indata mapp eller behållare inte finns och hello sektor bearbetningen misslyckas, visas ett felmeddelande om behållaren hello eller mappen finns inte.

    ![Aktivitetskörningsförsök](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. Starta **SQL Server Management Studio**, ansluta toohello Azure SQL Database och kontrollera att hello rader infogas i toohello **tomma** tabellen i hello-databasen.
    
    ![sql-frågeresultat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.

### <a name="monitor-pipeline-using-diagram-view"></a>Övervaka pipeline med diagramvyn
Du kan också övervaka data pipelines med hjälp av hello diagramvyn.  

1. I hello **Datafabriken** bladet, klickar du på **Diagram**.
   
    ![Data Factory-bladet – Diagramikon](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Du bör se hello diagram liknande toohello följande bild: 
   
    ![Diagramvy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. Dubbelklicka i hello diagramvyn **InputDataset** toosee segment för hello dataset.  
   
    ![Datauppsättningar med InputDataset valt](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Klicka på **se mer** länka toosee alla hello datasegment. Du ser 24 timsegment mellan pipelinens start- och sluttider. 
   
    ![Alla indatasektorer](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    Alla hello datasektorer toohello aktuella UTC-tid är **klar** eftersom hello **emp.txt** filen finns alla hello-tid i hello blob-behållaren: **adftutorial\input**. hello segment för hello framtida gånger ännu inte i tillståndet ready. Bekräfta att ingen segment visas i hello **nyligen misslyckade datasektorer** avsnittet längst ned hello.
6. Stäng hello blad tills du se hello diagram visa (eller) rulla vänstra toosee hello diagram visar. Dubbelklicka sedan på **OutputDataset**. 
8. Klicka på **se mer** länk på hello **tabell** bladet för **OutputDataset** toosee alla hello segment.

    ![datasektorblad](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. Observera att alla hello segment toohello aktuella UTC-tid flytta från **väntande körning** tillstånd = > **pågår** ==> **klar** tillstånd. Hej från hello senaste segment (före aktuell tid) bearbetas från senaste toooldest som standard. Till exempel bearbetas hello aktuell tid är 8:12:00 UTC, hello segment för 7 PM - 20: 00 i hello 18: 00 - 7 PM sektorn. hello 20: 00 - 21: 00 sektorn behandlas hello slutet av hello tidsintervall som standard som inträffar efter 21: 00.  
10. Klicka på någon datasektorn hello listan och du bör se hello **datasektorn** bladet. En datauppsättning som associeras med ett aktivitetsfönster kallas ett segment. Ett segment kan ha en eller flera filer.  
    
     ![datasektorblad](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Om hello segment inte hello **klar** tillstånd, som du kan se hello överordnade sektorer som inte är redo och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista.
11. I hello **DATASEKTORN** bladet bör du se alla aktiviteter som körs i hello lista längst ned hello. Klicka på en **aktiviteten kör** toosee hello **aktivitet köras information** bladet. 
    
    ![Aktivitetskörningsinformation](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    I det här bladet kommer du se hur lång hello kopieringsåtgärden tog vilka dataflöde, hur många byte data har läs- och skriftliga, kör starttiden körs sluttid osv.  
12. Klicka på **X** tooclose alla hello blad tills du komma toohello hem bladet för hello **ADFTutorialDataFactory**.
13. (valfritt) Klicka på hello **datauppsättningar** panelen eller **Pipelines** panelen tooget hello blad som du har sett hello ovanstående steg. 
14. Starta **SQL Server Management Studio**, ansluta toohello Azure SQL Database och kontrollera att hello rader infogas i toohello **tomma** tabellen i hello-databasen.
    
    ![sql-frågeresultat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a>Sammanfattning
I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas. Du har använt hello Azure portal toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline. Här följer hello anvisningar som du utförde i den här kursen:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade **länkade tjänster**:
   1. En **Azure Storage** länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.     
   2. En **Azure SQL** länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata. 
3. Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.
4. Du skapade en **pipeline** med en **kopieringsaktivitet** med **BlobSource** som källa och **SqlSink** som mottagare.  

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.
