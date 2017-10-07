---
title: "aaaMove data från en lokal SQL Server-tooSQL Azure med Azure Data Factory | Microsoft Docs"
description: "Ställ in en ADM-pipeline som composes två data migreringsaktiviteter som tillsammans flyttar data dagligen mellan databaser på lokalt och i hello molnet."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a>Flytta data från en lokal SQL server tooSQL Azure med Azure Data Factory
Det här avsnittet visar hur toomove data från en lokal SQL Server-databas tooa SQL Azure Database via Azure Blob Storage med hjälp av hello Azure Data Factory (ADM).

En tabell som sammanfattar olika alternativ för att flytta data tooan Azure SQL Database finns [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

## <a name="intro"></a>Introduktion: Vad är ADF och när ska den vara används toomigrate data?
Azure Data Factory är en helt hanterad molnbaserade integration datatjänst som samordnar och automatiserar hello flytt och transformering av data. hello viktiga begrepp i hello ADF modellen är pipeline. En pipeline är en logisk gruppering av aktiviteter, som definierar hello åtgärder tooperform på hello data som finns i DataSet. Länkade tjänster är används toodefine hello information som behövs för Data Factory tooconnect toohello dataresurser.

Med ADF, kan befintliga databearbetning tjänster sammanställas till pipeline-data som är hög tillgänglighet och hanterad i hello molnet. Pipelines dessa data kan vara schemalagda tooingest, förbereda, transformera, analysera och publicera data och ADF hanterar och samordnar hello komplexa data och beroenden för bearbetning. Lösningar kan hello snabbt inbyggd och distribuerade i molnet, ansluta ett växande antal lokalt och moln-datakällor.

Överväg att använda ADF:

* När data måste toobe migreras kontinuerligt i ett hybridscenario som har åtkomst till både lokalt och molnresurser
* När hello data överförda eller toobe måste ändras eller har affärslogik till tooit när migreras.

ADF tillåter hello schemaläggning och övervakning av jobb med hjälp av enkla JSON-skript som hanterar hello flödet av data regelbundet. ADF har även andra funktioner som stöd för komplex. Mer information om ADF finns hello dokumentationen på [Azure Data Factory (ADM)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>hello Scenario
Vi har skapat en ADM-pipeline som composes två aktiviteter för migrering av data. Tillsammans flytta data dagligen mellan en lokal SQL-databas och en Azure SQL Database i hello molnet. hello två aktiviteter är:

* Kopiera data från en lokal SQL Server-databasen tooan Azure Blob Storage-konto
* Kopiera data från hello Azure Blob Storage-konto tooan Azure SQL Database.

> [!NOTE]
> Hej steg som visas här har anpassats från hello mer detaljerad genomgång som tillhandahålls av hello ADF team: [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) refererar till toohello relevanta avsnitt i avsnittet anges vid behov.
>
>

## <a name="prereqs"></a>Förhandskrav
Den här kursen förutsätter att du har:

* En **Azure-prenumeration**. Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* En **Azure storage-konto**. Du kan använda ett Azure storage-konto för att lagra hello data i den här självstudiekursen. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel. När du har skapat hello storage-konto behöver du tooobtain hello konto nyckel som används för tooaccess hello lagring. Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Åtkomst tooan **Azure SQL Database**. Om du måste konfigurera en Azure SQL Database hello tpoic [komma igång med Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) innehåller information om hur tooprovision en ny instans av en Azure SQL Database.
* Installerat och konfigurerat **Azure PowerShell** lokalt. Instruktioner finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

> [!NOTE]
> Den här proceduren använder hello [Azure-portalen](https://portal.azure.com/).
>
>

## <a name="upload-data"></a>Överför hello data tooyour lokala SQL Server
Vi använder hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello migreringsprocessen. hello NYC Taxi dataset är tillgänglig, enligt beskrivningen i det inlägget på Azure-blobblagring [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/). hello har två filer, hello trip_data.csv filen som innehåller information om resa och hello trip_far.csv filen som innehåller information om hello avgiften betalat för varje resa. Ett exempel och en beskrivning av dessa filer finns i [NYC Taxi resor Dataset beskrivning](machine-learning-data-science-process-sql-walkthrough.md#dataset).

Du kan anpassa hello förfarandet här tooa uppsättning dina egna data eller hello gör enligt med hjälp av hello NYC Taxi dataset. tooupload hello NYC Taxi dataset i dina lokala SQL Server-databas, följ hello proceduren som beskrivs i [Bulk importera Data till SQL Server-databas](machine-learning-data-science-process-sql-walkthrough.md#dbload). Dessa instruktioner är för en SQL Server på en virtuell dator i Azure, men hello proceduren för att ladda upp toohello på lokal SQL Server är hello samma.

## <a name="create-adf"></a>Skapa ett Azure Data Factory
Hej instruktioner för att skapa en ny Azure Data Factory och en resursgrupp i hello [Azure-portalen](https://portal.azure.com/) tillhandahålls [skapa ett Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory). Namnet hello ny ADF instans *adfdsp* och namnet hello resursgrupp skapade *adfdsprg*.

## <a name="install-and-configure-up-hello-data-management-gateway"></a>Installera och konfigurera in hello Data Management Gateway
tooenable din pipelines i ett Azure data factory toowork med en lokal SQL Server behöver du tooadd den som en länkad tjänst toohello data factory. toocreate en länkad tjänst för en lokal SQL Server måste du:

* Hämta och installera Microsoft Data Management Gateway till hello lokala dator.
* Konfigurera hello länkad tjänst för hello lokala datakälla toouse hello gateway.

hello Data Management Gateway Serialiserar och deserializes hello källa och mottagare data på hello dator där det finns.

Ställa in instruktioner och information om Data Management Gateway finns [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)

## <a name="adflinkedservices"></a>Skapa länkade tjänster tooconnect toohello dataresurser
En länkad tjänst definierar hello information som behövs för Azure Data Factory tooconnect tooa Dataresurs. hello stegvisa anvisningar för att skapa länkade tjänster finns i [Skapa länkade tjänster](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).

Vi har tre resurser i det här scenariot som krävs för länkade tjänster.

1. [Länkad tjänst för lokala SQL Server](#adf-linked-service-onprem-sql)
2. [Länkad tjänst för Azure Blob Storage](#adf-linked-service-blob-store)
3. [Länkad tjänst för Azure SQL-databas](#adf-linked-service-azure-sql)

### <a name="adf-linked-service-onprem-sql"></a>Länkad tjänst för lokala SQL Server-databas
toocreate hello länkad tjänst för hello lokala SQL Server:

* Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen
* Välj **SQL** och ange hello *användarnamn* och *lösenord* autentiseringsuppgifter för hello lokala SQL Server. Du behöver tooenter hello servername som en **instansnamn för fullständigt kvalificerade servername omvänt snedstreck (ServerNamn\InstansNamn)**. Namnet hello länkade tjänsten *adfonpremsql*.

### <a name="adf-linked-service-blob-store"></a>Länkad tjänst för Blob
toocreate hello länkad tjänst för hello Azure Blob Storage-konto:

* Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen
* Välj **Azure Storage-konto**
* Ange hello Azure Blob Storage-konto nyckel och behållare. Namnet hello länkade tjänsten *adfds*.

### <a name="adf-linked-service-azure-sql"></a>Länkad tjänst för Azure SQL-databas
toocreate hello länkad tjänst för hello Azure SQL Database:

* Klicka på hello **datalagret** i hello ADF landningssida på den klassiska Azure-portalen
* Välj **Azure SQL** och ange hello *användarnamn* och *lösenord* autentiseringsuppgifter för hello Azure SQL Database. Hej *användarnamn* måste anges som  *user@servername* .   

## <a name="adf-tables"></a>Definiera och skapa tabeller toospecify hur tooaccess hello datauppsättningar
Skapa tabeller som anger hello struktur, plats och tillgängligheten för hello datauppsättningar med hello följa skriptbaserade procedurer. JSON-filer finns används toodefine hello tabeller. Mer information om hello strukturen för de här filerna finns [datauppsättningar](../data-factory/data-factory-create-datasets.md).

> [!NOTE]
> Du bör köra hello `Add-AzureAccount` cmdlet innan du kör hello [ny AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet tooconfirm som hello höger Azure-prenumeration har valts för hello Kommandokörning. Dokumentation för denna cmdlet finns [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

hello JSON-baserade definitioner i hello tabeller Använd hello följande namn:

* Hej **tabellnamn** i hello lokal SQLServer är *nyctaxi_data*
* Hej **behållarnamn** i hello Azure Blob Storage-kontot är *containername*  

Tre tabelldefinitionerna krävs för den här ADF-pipelinen:

1. [Lokal SQL-tabell](#adf-table-onprem-sql)
2. [BLOB-tabell](#adf-table-blob-store)
3. [SQL Azure-tabellen](#adf-table-azure-sql)

> [!NOTE]
> De här procedurerna Använd Azure PowerShell toodefine och skapa hello ADF aktiviteter. Men dessa uppgifter kan också utföras med hjälp av hello Azure-portalen. Mer information finns i [skapa datauppsättningar](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).
>
>

### <a name="adf-table-onprem-sql"></a>Lokal SQL-tabell
hello tabelldefinitionen för hello lokala SQL Server har angetts i hello följande JSON-fil:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

hello kolumnnamn ingick inte. Du kan välja på hello kolumnnamn genom att inkludera dem här underordnad (information finns hello [ADF dokumentationen](../data-factory/data-factory-data-movement-activities.md) avsnittet.

Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *onpremtabledef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\onpremtabledef.json*). Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>BLOB-tabell
Definitionen för hello tabellen för hello utdata blob befinner sig i hello följande (det här mappar hello inhämtas data från lokala tooAzure blob):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *bloboutputtabledef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\bloboutputtabledef.json*). Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sq"></a>SQL Azure-tabellen
Definitionen för hello tabellen för hello SQL Azure utdata har hello följande (det här schemat mappar hello data från blob hello):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Kopiera hello JSON-definitionen av hello tabell till en fil med namnet *AzureSqlTable.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\AzureSqlTable.json*). Skapa hello tabell i ADF med hello följande Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <a name="adf-pipeline"></a>Definiera och skapa hello pipeline
Ange hello aktiviteter som tillhör toohello pipeline och skapa hello pipeline med hello följa skriptbaserade procedurer. En JSON-fil är används toodefine hello pipeline egenskaper.

* hello skript förutsätter att hello **pipeline namnet** är *AMLDSProcessPipeline*.
* Observera också att vi anger hello periodicitet hello pipeline toobe på daglig basis och används hello standard körningstiden för hello jobb (12: 00 UTC).

> [!NOTE]
> hello följande procedurer Använd Azure PowerShell toodefine och skapa hello ADF pipeline. Men den här uppgiften kan också utföras med hjälp av Azure-portalen. Mer information finns i [skapa pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).
>
>

Med hjälp av hello definitioner som tidigare hello pipeline definition för hello ADF anges enligt följande:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data tooSql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopiera den här JSON-definitionen för hello pipeline till en fil med namnet *pipelinedef.json* filen och spara den tooa känd plats (här antas toobe *C:\temp\pipelinedef.json*). Skapa hello pipeline i ADF med hello följande Azure PowerShell-cmdlet:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Bekräfta att du kan se hello-pipeline på hello ADF i hello Azure klassiska Portal visas som följande (när du klickar på hello diagram)

![ADF pipeline](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <a name="adf-pipeline-start"></a>Starta hello Pipeline
Nu kan köra hello pipeline med hello följande kommando:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Hej *startdate* och *enddate* parametervärden måste ersättas med hello verkliga datum som ska hello pipeline toorun toobe.

När hello pipeline utförs, bör du kunna toosee hello data visas i hello-behållaren som valts för hello blob, en fil per dag.

Observera att vi inte utnyttjas hello funktionalitet som tillhandahålls av ADF toopipe data inkrementellt. Mer information om hur toodo detta och andra funktioner som tillhandahålls av ADF, se hello [ADF dokumentationen](https://azure.microsoft.com/services/data-factory/).
