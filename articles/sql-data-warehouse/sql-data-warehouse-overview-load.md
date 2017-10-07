---
title: aaaLoad data till Azure SQL Data Warehouse | Microsoft Docs
description: "Lär dig hello vanliga scenarier för datainläsning till SQL Data Warehouse. Dessa inkluderar med PolyBase, Azure blob storage, flat-filer och disk leverans. Du kan också använda verktyg från tredje part."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: d1a5063f484e9bd95f854e27a4baed512148aad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Läs in data till Azure SQL Data Warehouse
En sammanfattning av hello scenariot alternativ och rekommendationer för att läsa in data i SQL Data Warehouse.

hello svåraste del inläsning av data vanligtvis förbereder hello data för hello belastningen. Azure förenklar inläsning genom att använda Azure blob-lagring som en gemensam datalager för många hello tjänster och använda Azure Data Factory tooorchestrate kommunikations- och flytt mellan hello Azure-tjänster. De här processerna är integrerade med PolyBase-teknik som använder massivt parallell bearbetning (MPP) tooload data parallellt från Azure blob storage till SQL Data Warehouse. 

Självstudier som läser in exempeldatabaserna finns [ladda exempeldatabaserna][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Läsa in från Azure blob storage
hello snabbaste sättet tooimport data till SQL Data Warehouse är toouse PolyBase tooload data från Azure blob storage. PolyBase använder SQL Data Warehouses databearbetning massivt parallell (MPP) design tooload parallellt från Azure blob storage. toouse PolyBase som du kan använda T-SQL-kommandon eller ett Azure Data Factory-pipelinen.

### <a name="1-use-polybase-and-t-sql"></a>1. Använd PolyBase och T-SQL
Översikt över processen för inläsning:

1. Flytta dina data tooAzure blob storage eller Azure Data Lake Store och lagra den i textfiler.
2. Konfigurera externa objekt i SQL Data Warehouse toodefine hello plats och format för hello data
3. Kör en T-SQL-kommandot tooload hello data parallellt i en ny databastabell.

<!-- 5. Schedule and run a loading job. --> 

En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Använda Azure Data Factory
För ett enklare sätt toouse PolyBase, kan du skapa ett Azure Data Factory-pipelinen som använder PolyBase tooload data från Azure blob storage till SQL Data Warehouse. Detta är snabb tooconfigure eftersom du inte behöver toodefine hello T-SQL-objekt. Om du behöver tooquery hello externa data utan att importera den kan använda T-SQL. 

Översikt över processen för inläsning:

1. Flytta dina data tooAzure blob storage och lagrar den i textfiler. Azure Data Factory stöder för närvarande inte ADLS-anslutning med PolyBase).
2. Skapa ett Azure Data Factory pipeline tooingest hello data. Använd hello PolyBase-alternativet.
4. Schemalägga och köra hello pipeline.

En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Läs in från SQLServer
tooload data från SQL Server-tooSQL datalagret som du kan använda Integration Services (SSIS), överför flat-filer eller leverera diskar tooMicrosoft. Läs vidare toosee en sammanfattning av hello olika inläsning tootutorials processer och länkar.

tooplan en fullständig datamigrering från SQL Server-tooSQL datalagret finns hello [migreringsöversikt][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Använda Integration Services (SSIS)
Om du redan använder Integration Services (SSIS) paket tooload i SQL Server, kan du uppdatera ditt paket toouse SQL Server som hello käll- och SQL Data Warehouse som hello mål. Det går snabbt och enkelt toodo och är ett bra alternativ om du inte försöker toomigrate din inläsning bearbeta toouse data redan i hello molnet. hello förhållandet är hello belastningen blir långsammare än att använda PolyBase eftersom den här SSIS inte utför hello belastningen parallellt.

Översikt över processen för inläsning:

1. Ändra Integration Services paketet toopoint toohello SQL Server-instans för hello källa och hello SQL Data Warehouse-databas för hello mål.
2. Migrera din schemat tooSQL datalagret, om du inte redan har gjort det.
3. Ändra hello mappning i dina paket Använd endast hello-datatyper som stöds av SQL Data Warehouse.
4. Schemalägga och köra hello-paketet.

En självstudiekurs finns [läsa in data från SQL Server-tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Använda AZCopy (rekommenderas för < 10 TB data)
Om dina data är < 10 TB kan du exportera hello data från SQL Server tooflat filer, kopiera hello filer tooAzure blob-lagring och sedan använda PolyBase tooload hello data till SQL Data Warehouse

Översikt över processen för inläsning:

1. Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer.
2. Använda AZCopy hello kommandoradsverktyget toocopy data från flata filer tooAzure blob storage.
3. Använd PolyBase tooload till SQL Data Warehouse.

En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Använd bcp
Om du har en liten mängd data kan du använda bcp tooload direkt i Azure SQL Data Warehouse.

Översikt över processen för inläsning:

1. Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer.
2. Använd bcp tooload data från 2D filer direkt tooSQL Data Warehouse.

En självstudiekurs finns [läsa in data från SQL Server-tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Använd Import/Export (rekommenderas för > 10 TB data)
Om dina data är > 10 TB och du vill toomove den tooAzure, rekommenderar vi att du använder våra disk som levererar tjänsten [Import/Export][Import/Export]. 

Översikt över processen för inläsning

1. Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer på överföringsbar diskar.
2. Leverera hello diskar tooMicrosoft.
3. Hello data hämtas från Microsoft till SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Läsa in från HDInsight
SQL Data Warehouse stöder läsning av data från HDInsight via PolyBase. hello-processen är hello samma som läser in data från Azure Blob Storage - med PolyBase tooconnect tooHDInsight tooload data. 

### <a name="1-use-polybase-and-t-sql"></a>1. Använd PolyBase och T-SQL
Översikt över processen för inläsning:

1. Flytta dina data tooHDInsight och lagra den i textfiler, ORC eller parkettgolv format.
2. Konfigurera externa objekt i SQL Data Warehouse toodefine hello plats och format för hello data.
3. Kör en T-SQL-kommandot tooload hello data parallellt i en ny databastabell.

En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Rekommendationer
Många av våra samarbetspartners har inläsning av lösningar. toofind mer information, se en lista över våra [lösningspartners][solution partners]. 

Om dina data kommer från en icke-relationella källa och du vill tooload till SQL Data Warehouse du behöver tootransform till rader och kolumner innan du läser in. hello omvandlas data behöver inte toobe som lagras i en databas, den kan lagras i textfiler.

Skapa statistik på nyinlästa data. SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.  Det är viktigt toocreate statistik på alla kolumner i alla tabeller efter hello först läsa in eller vid betydande dataändringar i hello data i ordning tooget hello bästa prestanda från dina frågor.  Mer information finns i [statistik][Statistics].

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se hello [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server tooAzure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server tooAzure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
