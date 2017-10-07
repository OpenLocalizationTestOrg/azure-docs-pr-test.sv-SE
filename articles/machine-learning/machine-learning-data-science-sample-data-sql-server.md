---
title: "aaaSample Data i SQL Server på Azure | Microsoft Docs"
description: "Exempeldata i SQLServer på Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Exempeldata i SQL Server på Azure
Det här dokumentet beskrivs hur toosample data lagras i SQL Server på Azure med hjälp av SQL eller hello Python-programmeringsspråket. Den visar även hur toomove exempeldata i Azure Machine Learning genom att spara den tooa filen, överföra den tooan Azure blob och läs sedan i Azure Machine Learning Studio.

hello Python provtagning använder hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC-biblioteket tooconnect tooSQL Server på Azure och hello [Pandas](http://pandas.pydata.org/) biblioteket toodo hello provtagning.

> [!NOTE]
> hello SQL exempelkoden i det här dokumentet förutsätter att hello data är i en SQL Server på Azure. Om det inte finns för[flytta data tooSQL Server på Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) avsnittet för instruktioner om hur toomove dina data tooSQL Server på Azure.
> 
> 

hello följande **menyn** länkar tootopics som beskriver hur toosample data från olika miljöer för lagring. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Varför exempel dina data?**
Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek. Detta underlättar data förstå undersökning och funktionen tekniker. Roll i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.

Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>Med SQL
Det här avsnittet beskrivs flera metoder som använder SQL tooperform obundet slumpmässigt urval mot hello data i hello-databasen. Välj en metod baserat på datastorleken på din och dess distributionsplatser.

hello två objekt nedan visar hur toouse newid i SQL Server-tooperform hello provtagning. hello metod du väljer beror på hur slumpmässiga du vill hello exempel toobe (pk_id i hello exempelkoden nedan antas toobe en automatiskt genererad primärnyckel).

1. Mindre strikta slumpmässigt prov
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Fler slumpmässigt prov 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample kan vara används för provtagning som visas nedan. Detta kan vara en bättre metod om dina data är stor (förutsatt att data på olika sidor inte korreleras) och för hello frågan toocomplete i rimlig tid.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Du kan utforska och generera funktioner från den här samplade data genom att lagra det i en ny tabell
> 
> 

### <a name="sql-aml"></a>Ansluta tooAzure Machine Learning
Du kan använda hello exempelfrågor ovan direkt i hello Azure Machine Learning [importera Data] [ import-data] modulen toodown sampel hello data på hello föra och sätta den i ett Azure Machine Learning-experiment. En skärmbild av med hello reader modulen tooread hello provtagning data visas nedan:

![läsaren sql][1]

## <a name="python"></a>Med hjälp av programmeringsspråk för hello Python
Det här avsnittet visas hur du använder hello [pyodbc biblioteket](https://code.google.com/p/pyodbc/) tooestablish ODBC ansluta tooa SQL server-databas i Python. hello databasanslutningssträng är följande: (Ersätt servername, dbname, användarnamn och lösenord med din konfiguration):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hej [Pandas](http://pandas.pydata.org/) i Python-bibliotek innehåller ett stort utbud av datastrukturer och verktyg för analys av data för datamanipulering för Python-programmering. hello koden nedan läser ett 0,1% exempel hello data från en tabell i Azure SQL-databas i en Pandas data:

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nu kan du arbeta med hello provtagning data i hello Pandas data ram. 

### <a name="python-aml"></a>Ansluta tooAzure Machine Learning
Du kan använda följande kod toosave hello ned sampla tooa exempeldatafil hello och överföra den tooan Azure blob. hello data i blob hello kan direkt läsas in i ett Azure Machine Learning-Experiment med hello [importera Data] [ import-data] modul. hello stegen är följande: 

1. Skriva hello pandas ram tooa lokala datafilen
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Ladda upp en lokal fil tooAzure blob
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Läsa data från Azure blob med hjälp av Azure Machine Learning [importera Data] [ import-data] modulen som visas i fälten för hello-skärmen nedan:

![läsaren blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a>hello Team av vetenskapliga data i åtgärden exempel
En slutpunkt till slutpunkt genomgången exempel på hello Team av vetenskapliga data med en offentlig dataset finns [Team vetenskap av data i praktiken: med hjälp av SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
