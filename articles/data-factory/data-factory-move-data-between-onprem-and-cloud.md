---
title: aaaMove - data i Data Management Gateway | Microsoft Docs
description: "Ställ in en data gateway toomove data mellan lokala och hello molnet. Använd Data Management Gateway i Azure Data Factory toomove dina data."
keywords: datagatewayen dataintegrering, flytta data, gateway-autentiseringsuppgifter
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 314341c142d5260c785b7e82081774f044450e81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-between-on-premises-sources-and-hello-cloud-with-data-management-gateway"></a>Flytta data mellan lokala källor och hello moln med Data Management Gateway
Den här artikeln innehåller en översikt över dataintegration mellan lokala datalager och datalager i molnet med hjälp av Data Factory. Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln och andra data factory grundläggande begrepp artiklar: [datauppsättningar](data-factory-create-datasets.md) och [pipelines](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Gateway för datahantering
Data Management Gateway måste du installera på din lokala dator tooenable flytta data till och från ett lokalt datalager. hello gateway kan installeras på samma dator som hello datalager eller på en annan dator så länge hello gateway kan ansluta toohello datalagret hello.

> [!IMPORTANT]
> Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information om Data Management Gateway. 

hello följande genomgången visar hur toocreate en datafabrik med en rörledning som flyttar data från en lokal **SQL Server** tooan Azure blob storage-databas. Som en del av hello genomgången kan du installera och konfigurera hello Data Management Gateway på din dator.

## <a name="walkthrough-copy-on-premises-data-toocloud"></a>Genomgång: kopiera lokala data toocloud
I den här genomgången du hello följande steg: 

1. Skapa en datafabrik.
2. Skapa en data management gateway. 
3. Skapa länkade tjänster för källa och mottagare datalager.
4. Skapa datauppsättningar toorepresent indata och utdata.
5. Skapa en pipeline med kopiera aktiviteten toomove hello data.

## <a name="prerequisites-for-hello-tutorial"></a>Förutsättningar för självstudiekursen hello
Innan du börjar den här genomgången måste du ha hello följande krav:

* **Azure-prenumeration**.  Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter. Se hello [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.
* **Azure Storage-konto**. Du använder hello blob storage som en **mål-sink** datalager i den här självstudiekursen. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.
* **SQL Server**. Du använder en lokal SQL Server-databas som **källdata** i den här självstudien. 

## <a name="create-data-factory"></a>Skapa en datafabrik
I det här steget kan du använda hello Azure portal toocreate en Azure Data Factory-instans med namnet **ADFTutorialOnPremDF**.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **+ ny**, klickar du på **Intelligence + analys**, och klicka på **Data Factory**.

   ![Nytt->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. I hello **nya datafabriken** anger **ADFTutorialOnPremDF** för hello namn.

    ![Lägg till tooStartboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: **datafabriksnamnet ”ADFTutorialOnPremDF” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel yournameADFTutorialOnPremDF) och försök att skapa igen. Använd det här namnet i stället för ADFTutorialOnPremDF när du utför stegen i den här självstudiekursen.
   >
   > hello namn i hello data factory får registreras som en **DNS** namn i hello framtiden och därför bli synligt offentligt.
   >
   >
4. Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.
5. Välj befintlig **resursgrupp** eller skapa en resursgrupp. Hello självstudiekurs skapar du en resursgrupp med namnet: **ADFTutorialResourceGroup**.
6. Klicka på **skapa** på hello **nya data factory** sidan.

   > [!IMPORTANT]
   > toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.
   >
   >
7. När du har skapats visas hello **Data Factory** sidan som visas i följande bild hello:

   ![Data Factory-startsida](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Skapa en gateway
1. I hello **Datafabriken** klickar du på **författare och distribuera** panelen toolaunch hello **Editor** för hello data factory.

    ![Ikonen Författare och distribution](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. Hello Data Factory-redigeraren, klicka på **... Flera** hello verktygsfält och klicka sedan på **nya datagateway**. Du kan också högerklicka på **Datagatewayar** i hello trädvyn och klicka på **nya datagateway**.

   ![Nya datagateway i verktygsfältet](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. I hello **skapa** anger **adftutorialgateway** för hello **namn**, och klicka på **OK**.     

    ![Skapa sida för Gateway](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > I den här genomgången skapa hello logiska gateway med endast en nod (lokala Windows-dator). Du kan skala ut en data management gateway genom att associera flera lokala datorer med hello gateway. Du kan skala upp genom att öka antalet data movement jobb som kan köras samtidigt på en nod. Den här funktionen finns också en logisk gateway med en enda nod. Se [skalning data management gateway i Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) artikeln för information.  
4. I hello **konfigurera** klickar du på **installera direkt på den här datorn**. Den här åtgärden hämtar hello installationspaket för hello gateway, installerar, konfigurerar och registrerar hello gateway på hello-dator.  

   > [!NOTE]
   > Använd Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.
   >
   > Om du använder Chrome, gå toohello [Chrome web store](https://chrome.google.com/webstore/), söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.
   >
   > Hello samma för Firefox (installera tillägget). Klicka på **öppna menyn** hello i verktygsfältet (**tre horisontella raderna** hello längst upp till höger), klicka på **tillägg**, söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.    
   >
   >

    ![Gateway - konfigurera](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Det här sättet är hello enklaste sättet (enkelklickning) toodownload, installera, konfigurera och registrera hello gateway i ett enda steg. Du kan se hello **Microsoft Data Management Gateway Configuration Manager** programmet är installerat på datorn. Du kan också hitta hello körbara **ConfigManager.exe** i hello mapp: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    Du kan också hämta och installera gateway manuellt med hjälp av hello länkar i den här sidan och registrera den med hjälp av hello nyckeln visas i hello **ny nyckel** textruta.

    Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel för alla hello information om hello gateway.

   > [!NOTE]
   > Du måste vara administratör på hello lokal dator tooinstall och konfigurera hello Data Management Gateway har. Du kan lägga till fler användare toohello **Data Management Gateway-användare** lokala Windows-gruppen. hello medlemmar i gruppen kan använda hello Data Management Gateway Configuration Manager-verktyget tooconfigure hello gateway.
   >
   >
5. Vänta några minuter eller vänta tills du ser hello följande meddelande:

    ![Gatewayinstallationen lyckades](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Starta **Data Management Gateway Configuration Manager** på datorn. I hello **Sök** fönster, Skriv **Data Management Gateway** tooaccess det här verktyget. Du kan också hitta hello körbara **ConfigManager.exe** i hello mapp: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Bekräfta att du ser `adftutorialgateway is connected toohello cloud service` meddelande. Hej statusfältet hello nedre visar **anslutna toohello Molntjänsten** tillsammans med en **grön bock**.

    På hello **Start** och du kan också göra hello följande åtgärder:

   * **Registrera** en gateway med en nyckel från hello Azure-portalen med hjälp av hello Register-knappen.
   * **Stoppa** hello Data Management Gateway-värdtjänsten körs på gateway-datorn.
   * **Schemalägga uppdateringar** toobe som installerats på en specifik tidpunkt hello.
   * Visa när hello gateway var **senast uppdaterad**.
   * Ange tiden då en uppdatering toohello gateway kan installeras.
8. Växla toohello **inställningar** fliken hello certifikatet som anges i hello **certifikat** avsnittet är används tooencrypt/dekryptera autentiseringsuppgifterna för hello lokalt datalager som du anger på hello-portalen. (valfritt) Klicka på **ändra** toouse ditt eget certifikat i stället. Som standard använder hello gateway hello-certifikat som genereras automatiskt av hello Data Factory-tjänsten.

    ![Gateway-konfiguration för certifikat](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Du kan också göra hello följande åtgärder på hello **inställningar** fliken:

   * Visa eller exportera hello certifikatet som används av hello gateway.
   * Ändra hello HTTPS-slutpunkt som används av hello gateway.    
   * Ange en HTTP-proxy toobe som används av hello gateway.     
9. (valfritt) Växla toohello **diagnostik** kontrollerar hello **aktivera utförlig loggning** om du vill tooenable utförlig loggning som du kan använda tootroubleshoot eventuella problem med hello-gateway. hello loggningsinformation finns i **Loggboken** under **program- och tjänstloggar** -> **Data Management Gateway** nod.

    ![Fliken diagnostik](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Du kan också utföra följande åtgärder i hello hello **diagnostik** fliken:

   * Använd **Testanslutningen** avsnittet tooan lokal datakälla med hjälp av hello gateway.
   * Klicka på **visa loggfiler** toosee hello Data Management Gateway logga in ett fönster i Loggboken.
   * Klicka på **skicka loggar** tooupload en zip-fil med loggar av senaste sju dagarna tooMicrosoft toofacilitate felsökning av ditt problem.
10. På hello **diagnostik** på fliken hello **Testanslutningen** väljer **SqlServer** för hello hello datatyp lagra, ange hello namnet på hello databasserver, namnet på hello databas, ange autentiseringstypen, ange användarnamn och lösenord och klicka på **Test** tootest om hello gateway kan ansluta toohello databas.
11. Växeln toohello webbläsare och i hello **Azure-portalen**, klickar du på **OK** på hello **konfigurera** sidan och klicka sedan på hello **nya datagateway** sidan.
12. Du bör se **adftutorialgateway** under **Datagatewayar** i hello trädvyn hello vänster.  Om du klickar på den, bör du se hello associerade JSON.

## <a name="create-linked-services"></a>Skapa länkade tjänster
I det här steget skapar du två länkade tjänster: **AzureStorageLinkedService** och **SqlServerLinkedService**. Hej **SqlServerLinkedService** länkar en lokal SQL Server-databas och hello **AzureStorageLinkedService** länkade tjänsten länkar ett Azure blob store toohello data factory. Du kan skapa en pipeline för senare i den här genomgången som kopierar data från hello lokala SQL Server-databasen toohello Azure blobstore.

#### <a name="add-a-linked-service-tooan-on-premises-sql-server-database"></a>Lägga till en länkad tjänst tooan lokala SQL Server-databas
1. I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** på hello verktygsfältet och välj **SQL Server**.

   ![Ny SQL Server länkad tjänst](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. I hello **JSON-redigerare** på rätt hello, hello gör du följande steg:

   1. För hello **gatewayName**, ange **adftutorialgateway**.    
   2. I hello **connectionString**, hello följande steg:    

      1. För **servername**, anger hello namnet på hello-server som är värd för hello SQL Server-databas.
      2. För **databasename**, ange hello namnet på hello-databasen.
      3. Klicka på **kryptera** hello i verktygsfältet. Du ser hello Autentiseringshanteraren program.

         ![Autentiseringsuppgifter Manager-program](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. I hello **ställa in autentiseringsuppgifter** dialogrutan Ange autentiseringstypen, användarnamn och lösenord och klicka på **OK**. Om hello anslutningen är klar, hello krypterade autentiseringsuppgifter lagras i hello JSON och hello dialogrutan stängs.
      5. Stäng hello tom webbläsarflik som startas hello dialogruta om den inte är stängd automatiskt och få tillbaka toohello flik med hello Azure-portalen.

         Hello gateway-datorn är autentiseringsuppgifterna **krypterade** genom att använda ett certifikat som hello Data Factory tjänsten äger. Om du vill ha toouse hello certifikat som är associerad med hello Data Management Gateway i stället Se [ange autentiseringsuppgifterna på ett säkert sätt](#set-credentials-and-security).    
   3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello tjänsten SQL Server som är länkad. Du bör se hello länkad tjänst i hello trädvyn.

      ![SQL Server som är länkad tjänst i hello trädvy](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Lägg till en länkad tjänst för ett Azure storage-konto
1. I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** hello kommandofältet och klicka på **Azure storage**.
2. Anger hello namnet på ditt Azure storage-konto för hello **kontonamn**.
3. Ange hello nyckel för Azure storage-konto för hello **kontonyckel**.
4. Klicka på **distribuera** toodeploy hello **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Skapa datauppsättningar
I det här steget kan du skapa indata och utdata datauppsättningar som representerar inkommande och utgående data för hello kopieringen (lokal SQL Server-databas = > Azure blob storage). Innan du skapar datauppsättningar hello följande (detaljerade anvisningar följer hello lista):

* Skapa en tabell med namnet **tomma** i hello SQL Server-databas som du lagt till som en länkad tjänst toohello data factory och infoga några exempel poster i hello-tabellen.
* Skapa en blobbbehållare med namnet **adftutorial** i hello Azure blob storage-konto som du lagt till som en länkad tjänst toohello data factory.

### <a name="prepare-on-premises-sql-server-for-hello-tutorial"></a>Förbered på lokal SQL Server hello genomgång
1. Länkade tjänsten i hello-databasen som du angav för hello lokala SQL Server (**SqlServerLinkedService**), Använd följande SQL-skript toocreate hello hello **tomma** tabellen i hello-databasen.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Infoga vissa exempel i hello tabell:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Skapa indatauppsättning

1. I hello **Data Factory-redigeraren**, klickar du på **... Flera**, klickar du på **ny datamängd** på hello kommandofältet och klicka på **SQL Server-tabellen**.
2. Ersätt hello JSON i hello högra med hello följande text:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   Observera följande punkter hello:

   * **typen** har angetts för**SqlServerTable**.
   * **tableName** har angetts för**tomma**.
   * **linkedServiceName** har angetts för**SqlServerLinkedService** (du har skapat den här länkade tjänsten tidigare i den här genomgången.).
   * För en inkommande dataset som genereras av en annan pipeline i Azure Data Factory, måste du ange **externa** för**SANT**. Den anger hello indata är producerade externa toohello Azure Data Factory-tjänsten. Du kan också ange policyer externa data med hjälp av hello **externalData** element i hello **princip** avsnitt.    

   Se [flytta data till/från SQL Server](data-factory-sqlserver-connector.md) för ytterligare information om JSON-egenskaper.
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello dataset.  

### <a name="create-output-dataset"></a>Skapa datauppsättning för utdata

1. I hello **Data Factory-redigeraren**, klickar du på **ny datamängd** på hello kommandofältet och klicka på **Azure Blob storage**.
2. Ersätt hello JSON i hello högra med hello följande text:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Observera följande punkter hello:

   * **typen** har angetts för**AzureBlob**.
   * **linkedServiceName** har angetts för**AzureStorageLinkedService** (du har skapat den här länkade tjänsten i steg 2).
   * **folderPath** har angetts för**adftutorial/outfromonpremdf** där outfromonpremdf är hello mapp i hello adftutorial behållaren. Skapa hello **adftutorial** behållaren om den inte redan finns.
   * Hej **tillgänglighet** har angetts för**varje timme** (**frekvens** ställa in också**timme** och **intervall** ställa in också **1**).  hello Data Factory-tjänsten som genererar en utdata datasektorn varje timme i hello **tomma** tabellen i hello Azure SQL Database.

   Om du inte anger en **fileName** för en **utdatatabell**, hello genererade filer i hello **folderPath** namnges i hello följande format: Data.<Guid>. txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   tooset **folderPath** och **fileName** dynamiskt utifrån hello **SliceStart** tid, använda hello partitionedBy egenskap. I följande exempel hello, folderPath använder år, månad och dag från hello SliceStart (starttiden för hello sektorn bearbetas) och fileName använder timme från hello SliceStart. Om exempelvis ett segment skapas för 2014-10-20T08:00:00, hello mappnamn toowikidatagateway/wikisampledataout/2014/10/20 och hello filnamn har angetts too08.csv.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    Se [flytta data till/från Azure Blob Storage](data-factory-azure-blob-connector.md) mer information om JSON-egenskaper.
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello dataset. Bekräfta att du ser både hello datauppsättningar i hello trädvyn.  

## <a name="create-pipeline"></a>Skapa pipeline
I det här steget skapar du en **pipeline** med en **Kopieringsaktiviteten** som använder **EmpOnPremSQLTable** som indata och **OutputBlobTable** som utdata.

1. I Data Factory-redigeraren, klicka på **... More (Mer)** och sedan på **Ny pipeline**.
2. Ersätt hello JSON i hello högra med hello följande text:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL tooAzure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server tooblob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
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
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag.
   >
   >

   Observera följande punkter hello:

   * Under hello aktiviteter är bara aktivitet vars **typen** har angetts för**kopiera**.
   * **Indata** för hello aktiviteten är inställd för**EmpOnPremSQLTable** och **utdata** för hello aktiviteten är inställd för**OutputBlobTable**.
   * I hello **typeProperties** avsnittet **SqlSource** har angetts som hello **typ av datakälla** och ** BlobSink ** har angetts som hello **sink typen**.
   * SQL-frågan `select * from emp` har angetts för hello **sqlReaderQuery** -egenskapen för **SqlSource**.

   Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601). Exempel: 2014-10-14T16:32:41Z. Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.

   Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”. toorun hello pipeline på obestämd tid, ange **9999-9-9** som hello värde hello **end** egenskapen.

   Du definierar hello varaktighet i vilka hello datasektorer bearbetas baserat på hello **tillgänglighet** egenskaper som definierats för varje Azure Data Factory-datauppsättningen.

   I exemplet hello finns 24 datasektorer som varje datasektorn produceras per timma.        
3. Klicka på **distribuera** på hello i kommandofältet toodeploy hello datauppsättningen (tabellen är en rektangulär dataset). Bekräfta att hello pipeline visas i hello trädvyn under **Pipelines** nod.  
4. Klicka på **X** två gånger tooclose hello sidan tooget tillbaka toohello **Datafabriken** för hello **ADFTutorialOnPremDF**.

**Grattis!** Du har skapat ett Azure data factory, länkade tjänster, datauppsättningar, och en pipeline och schemalagda hello pipeline.

#### <a name="view-hello-data-factory-in-a-diagram-view"></a>Visa hello data factory i en diagramvy
1. I hello **Azure-portalen**, klickar du på **Diagram** panelen på hello startsidan för hello **ADFTutorialOnPremDF** datafabriken. :

    ![Diagram-länk](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Du bör se hello diagram liknande toohello följande bild:

    ![Diagramvy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Du kan zooma in, Zooma in, Zooma too100%, Zooma toofit, automatiskt position pipelines och datauppsättningar och visa härkomst information (markerar överordnade och underordnade objekt av valda objekt).  Du kan dubbelklicka på ett objekt (i/o-datauppsättningen eller pipeline) toosee egenskaper för den.

## <a name="monitor-pipeline"></a>Övervaka pipeline
I det här steget använder du hello Azure portal toomonitor vad som händer i ett Azure data factory. Du kan också använda PowerShell-cmdlets toomonitor datamängd och pipelinor. Mer information om övervakning finns [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md).

1. Dubbelklicka i hello diagram **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Observera att alla hello datasektorer in finns i **klar** tillstånd eftersom hello pipeline varaktighet (tid tooend starttid) hello tidigare. Det är också eftersom du har infogat hello data i hello SQL Server-databas och det är där alla hello tid. Bekräfta att ingen segment visas i hello **problemet segment** avsnittet längst ned hello. tooview alla hello segment, klicka på **finns mer** längst hello hello lista över segment.
3. Nu i hello **datauppsättningar** klickar du på **OutputBlobTable**.

    ![OputputBlobTable segment](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Klicka på någon datasektorn hello listan och du bör se hello **Datasektorn** sidan. Du kan se aktiviteten körs för hello segment. Endast en aktivitet körs vanligtvis visas.  

    ![Data sektorn bladet](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Om hello segment inte hello **klar** tillstånd, som du kan se hello överordnade sektorer som inte är redo och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista.
5. Klicka på hello **aktiviteten kör** hello listan vid hello nedre toosee **aktivitet köras information**.

   ![Aktiviteten kör informationssidan](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Visas information som dataflöde, varaktighet och hello gateway används tootransfer hello data.
6. Klicka på **X** tooclose alla hello sidor förrän du
7. Hämta toohello startsidan för hello **ADFTutorialOnPremDF**.
8. (valfritt) Klicka på **Pipelines**, klickar du på **ADFTutorialOnPremDF**, och detaljvisning indatatabeller (**förbrukad**) eller utdata datauppsättningar (**framställda**).
9. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) tooverify att en blob/fil skapas för varje timme.

   ![Azure Lagringsutforskaren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Nästa steg
* Se [Data Management Gateway](data-factory-data-management-gateway.md) artikel för alla hello information om hello Data Management Gateway.
* Se [kopiera data från Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) toolearn om hur toouse Kopieringsaktiviteten toomove data från en källdata lagrar tooa sink-datalagret.
