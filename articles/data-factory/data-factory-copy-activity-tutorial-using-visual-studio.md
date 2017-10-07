---
title: "Självstudier: Skapa en pipeline med en kopieringsaktivitet med hjälp av Visual Studio | Microsoft Docs"
description: "I de här självstudierna skapar du en Azure Data Factory-pipeline med en kopieringsaktivitet genom att använda Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Självstudie: Skapa en pipeline med en kopieringsaktivitet med hjälp av Visual Studio
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

I den här artikeln får du lära dig hur hello toouse Microsoft Visual Studio toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas. Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.   

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE] 
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Krav
1. Läs igenom [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikeln och fullständig hello **nödvändiga** steg.       
2. toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.
3. Du måste ha hello följande installerat på datorn: 
   * Visual Studio 2013 eller Visual Studio 2015
   * Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015. Navigera för[Azure-hämtningssida](https://azure.microsoft.com/downloads/) och på **VS 2013** eller **VS 2015** i hello **.NET** avsnitt.
   * Hämta hello senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Du kan också uppdatera hello plugin-programmet genom att göra följande hello: på menyn hello **verktyg** -> **tillägg och uppdateringar** -> **Online**  ->  **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **uppdatering**.

## <a name="steps"></a>Steg
Här är hello steg du utför som en del av den här kursen:

1. Skapa **länkade tjänster** i hello data factory. I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas. 
    
    Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).     
2. Skapa ingående och utgående **datauppsättningar** i hello data factory.  
    
    hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.  

    På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.
3. Skapa en **pipeline** i hello data factory. I det här steget kan du skapa en pipeline med en kopieringsaktivitet.   
    
    Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas. Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet. I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds. 
4. Skapa en Azure-**datafabrik** när du distribuerar Data Factory-enheter (länkade tjänster, datauppsättningar och tabeller och pipelines). 

## <a name="create-visual-studio-project"></a>Skapa Visual Studio-projekt
1. Starta **Visual Studio 2015**. Klicka på **filen**, peka för**ny**, och klicka på **projekt**. Du bör se hello **nytt projekt** dialogrutan.  
2. I hello **nytt projekt** dialogrutan, Välj hello **DataFactory** mall och klicka på **tomt Data Factory projekt**.  
   
    ![Dialogrutan Nytt projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Ange hello av hello projekt, platsen för hello lösningen och namn på hello lösningen och klicka sedan på **OK**.
   
    ![Solution Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Skapa länkade tjänster
Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory. I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics. Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål). 

Därför kan du skapa två länkade tjänster: AzureStorage och AzureSqlDatabase.  

hello Azure Storage länkade tjänsten länkar till din Azure storage-konto toohello data factory. Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

Azure SQL länkade tjänsten länkar till din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Länkade tjänster länka datalager eller compute services tooan Azure data factory. Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten. Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory. I den här självstudiekursen använder du ingen tjänst för beräkning. 

### <a name="create-hello-azure-storage-linked-service"></a>Skapa hello länkad Azure Storage-tjänst
1. I **Solution Explorer**, högerklicka på **länkade tjänster**, peka för**Lägg till**, och klicka på **nytt objekt**.      
2. I hello **Lägg till nytt objekt** dialogrutan **länkade Azure-lagringstjänsten** hello listan och klickar på **Lägg till**. 
   
    ![Ny länkad tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Ersätt `<accountname>` och `<accountkey>`* med hello namnet på ditt Azure storage-konto och dess nyckel. 
   
    ![Länkad Azure Storage-tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Spara hello **AzureStorageLinkedService1.json** fil.

    Läs mer om JSON-egenskaper i länkade hello tjänstdefinitionen [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel.

### <a name="create-hello-azure-sql-linked-service"></a>Skapa hello Azure SQL-länkade tjänst
1. Högerklicka på **länkade tjänster** nod i hello **Solution Explorer** igen och peka för**Lägg till**, och klicka på **nytt objekt**. 
2. Den här gången väljer du **Länkad Azure SQL-tjänst** och klickar på **Lägg till**. 
3. I hello **AzureSqlLinkedService1.json filen**, Ersätt `<servername>`, `<databasename>`, `<username@servername>`, och `<password>` med namn på din Azure SQL server, databas, användarkonto och lösenord.    
4. Spara hello **AzureSqlLinkedService1.json** fil. 
    
    Mer information om de här JSON-egenskaperna finns i [Anslutningsapp för Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).


## <a name="create-datasets"></a>Skapa datauppsättningar
I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory. I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService1 och AzureSqlLinkedService1 respektive.

hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.  

På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas. Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage. 

### <a name="create-input-dataset"></a>Skapa indatauppsättning
I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService1 länkade tjänsten. Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål. I kursen får ange du ett värde för hello filnamnet. 

Här kan använda du hello termen ”tabeller” i stället för ”datauppsättningar”. En tabell är en rektangulär datamängd och hello enda typen av datauppsättning stöds just nu. 

1. Högerklicka på **tabeller** i hello **Solution Explorer**, peka för**Lägg till**, och klicka på **nytt objekt**.
2. I hello **Lägg till nytt objekt** dialogrutan **Azure Blob**, och klicka på **Lägg till**.   
3. Ersätt hello JSON-text med hello följande text och spara hello **AzureBlobLocation1.json** fil. 

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
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
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

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata
I det här steget ska du skapa en utdatauppsättning med namnet **OutputDataset**. Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService1**. 

1. Högerklicka på **tabeller** i hello **Solution Explorer** igen och peka för**Lägg till**, och klicka på **nytt objekt**.
2. I hello **Lägg till nytt objekt** dialogrutan **Azure SQL**, och klicka på **Lägg till**. 
3. Ersätt hello JSON-text med hello följande JSON och spara hello **AzureSqlTableLocation1.json** fil.

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
       "linkedServiceName": "AzureSqlLinkedService1",
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

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.

Datamängd för utdata är för närvarande vilka enheter hello schema. I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen. hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar. Därför produceras 24 segment för utdatauppsättningen av hello pipeline. 

1. Högerklicka på **Pipelines** i hello **Solution Explorer**, peka för**Lägg till**, och klicka på **nytt objekt**.  
2. Välj **kopiera Data Pipeline** i hello **Lägg till nytt objekt** dialogrutan och klicka på **Lägg till**. 
3. Ersätt hello JSON med hello följande JSON och spara hello **CopyActivity1.json** fil.

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
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**. Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md). I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).
    - Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**. 
    - I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen. En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats). toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.  
     
    Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag. Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid. Till exempel ”2016-02-03”, vilket motsvarar för ”2016-02-03T00:00:00Z”
     
    Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2016-10-14T16:32:41Z. Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen. 
     
    Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”. toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.
     
    I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.

    Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md). Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md). Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md). Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).

## <a name="publishdeploy-data-factory-entities"></a>Publicera/distribuera Data Factory-entiteter
I det här steget publicerar du Data Factory-enheter (länkade tjänster, datauppsättningar och pipeline) som du skapade tidigare. Du kan också ange hello namn för hello nya data factory toobe skapade toohold dessa enheter.  

1. Högerklicka på projektet i hello Solution Explorer och klicka på **publicera**. 
2. Om du ser **logga in tooyour Microsoft-konto** dialogrutan, ange dina autentiseringsuppgifter för hello-konto med Azure-prenumeration och på **logga in**.
3. Du bör se följande dialogrutan hello:
   
   ![Dialogrutan Publicera](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Hello konfigurera data factory på sidan hello gör du följande steg: 
   
   1. välj alternativet **Skapa ny Data Factory**.
   2. Ange **VSTutorialFactory** som **namn**.  
      
      > [!IMPORTANT]
      > hello namn i hello Azure data factory måste vara globalt unika. Om du får ett felmeddelande om hello namnet på data factory vid publicering, ändra hello namn hello data factory (till exempel yournameVSTutorialFactory) och försök att publicera igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.        
      > 
      > 
   3. Välj din Azure-prenumeration för hello **prenumeration** fältet.
      
      > [!IMPORTANT]
      > Om du inte ser någon prenumeration, kontrollera att du har loggat in med ett konto som är en administratör eller medadministratör för hello prenumeration.  
      > 
      > 
   4. Välj hello **resursgruppen** för hello data factory toobe skapas. 
   5. Välj hello **region** för hello data factory. Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.
   6. Klicka på **nästa** tooswitch toohello **publicera objekt** sidan.
      
       ![Konfigurera Data Factory-sida](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. I hello **publicera objekt** , se till att alla hello Datafabriker entiteter är markerade och klickar på **nästa** tooswitch toohello **sammanfattning** sidan.
   
   ![Sidan Publish items (Publicera objekt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Granska hello sammanfattning och klicka på **nästa** toostart hello distribution processen och visa hello **Distributionsstatus**.
   
   ![Sidan Publish summary (Publicera översikt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. I hello **Distributionsstatus** sida, bör du se hello status hello distributionsprocessen. Klicka på Slutför när hello distributionen är klar.
 
   ![Statussida för distribution](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Observera följande punkter hello: 

* Om felmeddelandet hello: ”den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory”, gör du något av följande hello och försök att publicera igen: 
  
  * Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen. Den här åtgärden registrerar automatiskt hello-providern för dig.
* hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.

> [!IMPORTANT]
> toocreate Data Factory instanser måste toobe en admin/medadministratör av hello Azure-prenumeration

## <a name="monitor-pipeline"></a>Övervaka pipeline
Navigera toohello startsidan för din data factory:

1. Logga in för[Azure-portalen](https://portal.azure.com).
2. Klicka på **fler tjänster** på hello vänstra menyn och klickar på **datafabriker**.

    ![Bläddra igenom datafabrikerna](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. Börja skriva hello namnet på din data factory.

    ![Namnet på datafabriken](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. Klicka på din data factory i hello resultat listan toosee hello startsidan för din data factory.

    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. Följ anvisningarna från [övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen. Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines. 

## <a name="summary"></a>Sammanfattning
I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas. Du har använt Visual Studio toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline. Här följer hello anvisningar som du utförde i den här kursen:  

1. Du skapade en Azure **Data Factory**.
2. Du skapade **länkade tjänster**:
   1. En **Azure Storage** länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.     
   2. En **Azure SQL** länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata. 
3. Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.
4. Du skapade en **pipeline** med en **kopieringsaktivitet** med **BlobSource** som källa och **SqlSink** som mottagare. 

hur toouse en HDInsight Hive tootransform aktivitetsdata med hjälp av Azure HDInsight-kluster, se toosee [ Självstudier: skapa din första pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory). 

## <a name="view-all-data-factories-in-server-explorer"></a>Visa datafabriker i Server Explorer
Det här avsnittet beskrivs hur toouse hello Server Explorer i Visual Studio tooview alla hello datafabriker i din Azure-prenumeration och skapa ett Visual Studio-projekt utifrån en befintlig datafabrik. 

1. I **Visual Studio**, klickar du på **visa** på hello menyn och klickar på **Server Explorer**.
2. I hello Server Explorer, expandera **Azure** och expandera **Data Factory**. Om du ser **logga in tooVisual Studio**, ange hello **konto** som är associerade med din Azure-prenumeration och klicka på **Fortsätt**. Ange **lösenordet** och klicka på **Logga in**. Visual Studio försöker tooget information om alla Azure datafabriker i din prenumeration. Du ser hello status för den här åtgärden i hello **Data Factory uppgiftslista** fönster.

    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a>Skapa ett Visual Studio-projekt för en befintlig datafabrik

- Högerklicka på en datafabrik i Server Explorer och välj **exportera Data Factory tooNew projekt** toocreate Visual Studio-projekt utifrån en befintlig datafabrik.

    ![Exportera data factory tooa VS-projektet](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Uppdatera Data Factory-verktyg för Visual Studio
tooupdate Azure Data Factory tools för Visual Studio hello följande steg:

1. Klicka på **verktyg** på hello-menyn och välj **tillägg och uppdateringar**. 
2. Välj **uppdateringar** i hello till vänster och välj sedan **Visual Studio-galleriet**.
3. Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**. Om du inte ser den här posten har du redan hello senaste versionen av hello verktyg. 

## <a name="use-configuration-files"></a>Använda konfigurationsfiler
Du kan använda konfigurationsfiler i Visual Studio tooconfigure egenskaper för länkade tjänster/tabeller/pipelines på olika sätt för varje miljö.

Överväg att hello JSON-definitionen för en länkad Azure Storage-tjänst. toospecify **connectionString** med olika värden för accountname eller accountkey baserat på hello miljö (Dev/Test/produktion) toowhich du distribuerar Data Factory entiteter. Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a>Lägga till en konfigurationsfil
Lägg till en konfigurationsfil för varje miljö genom att utföra hello följande steg:   

1. Högerklicka på hello Data Factory-projekt i Visual Studio-lösning, peka för**Lägg till**, och klicka på **nytt objekt**.
2. Välj **Config** hello listan över installerade mallar hello vänster och välj **konfigurationsfilen**, ange en **namn** för konfiguration av hello fil och klicka på **Lägga till**.

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Lägg till konfigurationsparametrar och deras värden i hello följande format:

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst. Observera att hello syntax för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).   

    Om JSON har en egenskap som innehåller en matris med värden som visas i följande kod hello:  

    ```json
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
    ```

    Konfigurera egenskaperna som visas i hello följande konfigurationsfilen (Använd Nollbaserad indexering):

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a>Egenskapsnamn med blanksteg
Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser som visas i följande exempel (Databasservernamnet) hello:

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a>Distribuera lösningen med en konfiguration
När du publicerar Azure Data Factory-entiteter i VS anger du hello-konfiguration som du vill toouse för publishing operationen.

toopublish entiteter i ett Azure Data Factory-projekt med hjälp av konfigurationsfil:   

1. Högerklicka på Data Factory-projektet och klicka på **publicera** toosee hello **publicera objekt** dialogrutan.
2. Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på hello **konfigurera datafabriken** och klicka på **nästa**.   
3. På hello **publicera objekt** sida: du kan se en lista med tillgängliga konfigurationer för hello **Välj distributionskonfiguration** fältet.

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. Välj hello **konfigurationsfilen** som du skulle t.ex. toouse och på **nästa**.
5. Bekräfta att du ser hello namnet på JSON-fil i hello **sammanfattning** och klickar på **nästa**.
6. Klicka på **Slutför** när hello Distributionsåtgärden har slutförts.

När du distribuerar är hello värden från konfigurationsfilen hello används tooset värden för egenskaper i hello JSON-filer innan hello entiteter som är distribuerade tooAzure Data Factory-tjänsten.   

## <a name="use-azure-key-vault"></a>Använda Azure Key Vault
Det är inte tillrådligt och ofta mot säkerhet princip toocommit känsliga data, till exempel anslutning strängar toohello databasen. Se [ADF Secure publicera](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub toolearn om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory entiteter. hello Secure publicera tillägget för Visual Studio kan hello hemligheter toobe lagras i Nyckelvalvet och endast referenser toothem har angetts i länkade tjänster / distributionskonfigurationer. När du publicerar Data Factory entiteter tooAzure matchas dessa referenser. De här filerna kan sedan vara allokerat toosource databasen utan att exponera alla hemligheter.


## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.
