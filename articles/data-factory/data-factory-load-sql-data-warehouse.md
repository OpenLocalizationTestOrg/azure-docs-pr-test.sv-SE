---
title: aaaLoad terabyte data till SQL Data Warehouse | Microsoft Docs
description: "Visar hur 1 TB data kan läsas in i Azure SQL Data Warehouse med Azure Data Factory under 15 minuter"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e63f60461b082b0e3979004cb631dbf4a6710de3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>Läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Data Factory
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) är en molnbaserad skalbar databas kan bearbeta massiva mängder data, relationell såväl som icke-relationell.  Bygger på arkitektur med massivt parallell bearbetning (MPP) och är SQL Data Warehouse optimerad för arbetsbelastningar på företag data warehouse.  Den erbjuder molnet elasticitet med hello flexibilitet tooscale lagring och beräkna oberoende av varandra.

Komma igång med Azure SQL Data Warehouse är nu enklare än någonsin med **Azure Data Factory**.  Azure Data Factory är en helt hanterad molnbaserade integration datatjänst, vilket kan vara används toopopulate ett SQL Data Warehouse med hello data från ditt befintliga system och sparar värdefull tid när du utvärderar SQL Data Warehouse och skapa din analytics lösningar. Här följer hello viktiga fördelar med att läsa in data i Azure SQL Data Warehouse med Azure Data Factory:

* **Enkelt tooset in**: 5 intuitiva guiden med ingen skript som krävs.
* **Omfattande stöd för datalager**: inbyggt stöd för ett stort utbud av lokala och molnbaserade dataarkiv.
* **Skyddade och kompatibla**: data överförs över HTTPS eller ExpressRoute och tjänsten för global närvaro garanterar att dina data lämnar aldrig hello geografisk gräns
* **Enastående prestanda genom att använda PolyBase** – med Polybase är hello mest effektiva sättet toomove data till Azure SQL Data Warehouse. Du kan uppnå hög belastning hastigheter från alla typer av datalager utöver Azure Blob storage som hello Polybase stöder som standard använder hello mellanlagring blob-funktionen.

Den här artikeln beskrivs hur du toouse Data Factory kopiera guiden tooload 1 TB data från Azure Blobblagring till Azure SQL Data Warehouse i under 15 minuter vid över 1,2 Gbit/s genomströmning.

Den här artikeln innehåller stegvisa instruktioner för att flytta data till Azure SQL Data Warehouse med hjälp av hello guiden Kopiera.

> [!NOTE]
>  Allmän information om funktionerna i Data Factory för att flytta data till/från Azure SQL Data Warehouse finns [flytta data tooand från Azure SQL Data Warehouse med Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) artikel.
>
> Du kan också skapa pipelines med hjälp av Azure portal, Visual Studio, PowerShell, osv. Se [Självstudier: kopiera data från Azure Blob tooAzure SQL-databas](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för en snabb genomgång med stegvisa instruktioner för att använda hello Kopieringsaktiviteten i Azure Data Factory.  
>
>

## <a name="prerequisites"></a>Krav
* Azure Blob Storage: experimentet använder Azure Blob Storage (GRS) för att lagra TPC-H tester dataset.  Lär dig mer om du inte har ett Azure storage-konto [hur toocreate ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* [TPC-H](http://www.tpc.org/tpch/) data: vi toouse TPC-H som hello tester dataset.  toodo att du behöver toouse `dbgen` från TPC-H toolkit som hjälper dig att generera hello dataset.  Du kan antingen ladda ned källkoden för `dbgen` från [TPC verktyg](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) och kompilera den själv eller hämta hello kompileras binära från [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Kör dbgen.exe med hello följande kommandon toogenerate 1 TB flat fil för `lineitem` tabell sprids över 10 filer:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Kopiera hello genererat filer tooAzure Blob.  Se för[flytta data tooand från ett lokalt filsystem med hjälp av Azure Data Factory](data-factory-onprem-file-system-connector.md) för hur toodo som med ADF kopia.    
* Azure SQL Data Warehouse: experimentet läser in data i Azure SQL Data Warehouse som skapats med 6 000 dwu: er

    Se för[skapar ett Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) detaljerade anvisningar om hur toocreate en SQL Data Warehouse-databas.  tooget hello bästa möjliga belastningen prestanda i SQL Data Warehouse med Polybase vi väljer maxantalet Informationslagerenheter (dwu: er) tillåts i hello prestanda inställning, som är 6 000 dwu: er.

  > [!NOTE]
  > Vid inläsning från Azure Blob är hello prestanda för datainläsning proportionell toohello antalet dwu: er som du konfigurerar på hello SQL Data Warehouse:
  >
  > Läsa in 1 TB i 1 000 tar DWU SQL Data Warehouse 87 minuter (~ 200 Mbit/s genomströmning) lästes in 1 TB till 2 000 DWU-informationslager tar 46 minuter (~ 380 Mbit/s genomströmning) lästes in 1 TB till 6 000 DWU SQL Data Warehouse tar 14 minuter (~1.2 Gbit/s genomströmning)
  >
  >

    toocreate ett SQL Data Warehouse med 6 000 dwu: er skjutreglaget hello prestanda alla hello sätt toohello höger:

    ![Skjutreglaget för prestanda](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    För en befintlig databas som inte är konfigurerad med 6 000 dwu: er kan skala upp med hjälp av Azure portal.  Navigera toohello databas i Azure-portalen och det finns en **skala** knapp i hello **översikt** panelen visas i följande bild hello:

    ![Skala knappen](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klicka på hello **skala** knappen tooopen hello följande panelen, flytta skjutreglaget hello toohello maxvärdet och på **spara** knappen.

    ![Dialogruta](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Experimentet läser in data i Azure SQL Data Warehouse med hjälp av `xlargerc` resursklassen.

    tooachieve bästa möjliga genomströmningen, kopiera måste toobe utförs med hjälp av en SQL Data Warehouse användare för`xlargerc` resursklassen.  Lär dig hur toodo som genom att följa [ändra ett exempel på användaren resurs klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).  
* Skapa mål tabellschemat i Azure SQL Data Warehouse-databas genom att köra följande DDL-instruktion hello:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
Med hello nödvändiga steg slutförts, är vi nu redo tooconfigure hello kopieringsaktiviteten med hello guiden Kopiera.

## <a name="launch-copy-wizard"></a>Använda guiden Kopiera
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Intelligence + analys**, och klicka på **Data Factory**.
3. I hello **nya data factory** bladet:

   1. Ange **LoadIntoSQLDWDataFactory** för hello **namn**.
       hello namn i hello Azure data factory måste vara globalt unika. Om felmeddelandet hello: **datafabriksnamnet ”LoadIntoSQLDWDataFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel yournameLoadIntoSQLDWDataFactory) och försök att skapa igen. Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.  
   2. Välj din Azure-**prenumeration**.
   3. För resursgruppen, gör du något av följande hello:
      1. Välj **använda befintliga** tooselect en befintlig resursgrupp.
      2. Välj **Skapa nytt** tooenter ett namn för en resursgrupp.
   4. Välj en **plats** för hello data factory.
   5. Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.  
   6. Klicka på **Skapa**.
4. När hello har skapats visas hello **Data Factory** bladet som visas i följande bild hello:

   ![Datafabrikens startsida](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. Klicka på startsidan för hello Data Factory hello **kopiera data** panelen toolaunch **guiden Kopiera**.

   > [!NOTE]
   > Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställning (eller) se till att den är aktiverad och skapa ett undantag för **login.microsoftonline.com**och försök sedan starta hello guiden igen.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>Steg 1: Konfigurera schemat för datainläsning
hello första steget är tooconfigure hello datainläsning schema.  

I hello **egenskaper** sidan:

1. Ange **CopyFromBlobToAzureSqlDataWarehouse** för **aktivitet**
2. Välj **kör nu en gång** alternativet.   
3. Klicka på **Nästa**.  

    ![Guiden Kopiera - egenskapssidan](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Steg 2: Konfigurera källan
Det här avsnittet visar du hello steg tooconfigure hello källa: Azure Blob som innehåller hello 1 TB TPC-H radobjekt filer.

1. Välj hello **Azure Blob Storage** som hello data lagras och klicka på **nästa**.

    ![Kopiera - guidesidan Välj källa](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Fyll i hello anslutningsinformationen för hello Azure Blob storage-konto och klicka på **nästa**.

    ![Guiden Kopiera - anslutningsinformationen för datakällan](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Välj hello **mappen** som innehåller hello TPC-H rad filer och klickar på **nästa**.

    ![Guiden Kopiera – Välj inkommande mapp](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. När du klickar på **nästa**, hello formatinställningar identifieras automatiskt.  Kontrollera toomake till en kolumnavgränsaren för är ' | 'i stället för hello standard kommatecken','.  Klicka på **nästa** när du har förhandsgranskas hello data.

    ![Guiden Kopiera - inställningar för format](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Steg 3: Konfigurera mål
Det här avsnittet visas hur tooconfigure hello mål: `lineitem` tabell i hello Azure SQL Data Warehouse-databas.

1. Välj **Azure SQL Data Warehouse** som mål för hello lagra och klickar på **nästa**.

    ![Guiden Kopiera - Välj målserver datalager](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Fyll i hello anslutningsinformationen för Azure SQL Data Warehouse.  Kontrollera att du anger hello-användare som är medlem i rollen hello `xlargerc` (se hello **krav** avsnittet detaljerade anvisningar), och klicka på **nästa**.

    ![Guiden Kopiera - anslutningsinformation för mål](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Välj hello måltabellen och på **nästa**.

    ![Kopiera guidesidan - tabellen mappning](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. Schemat mappning på sidan låter ”tillämpa kolumnmappningen” alternativet är avmarkerat och klickar på **nästa**.

## <a name="step-4-performance-settings"></a>Steg 4: Prestandainställningar

**Tillåt polybase** är markerad som standard.  Klicka på **Nästa**.

![Kopiera guidesidan - schemat mappning](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Steg 5: Distribuera och övervaka belastningen resultat
1. Klicka på **Slutför** knappen toodeploy.

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. När hello distributionen är klar klickar du på `Click here toomonitor copy pipeline` toomonitor hello kopiera kör pågår. Välj hello kopiera pipeline som du skapade i hello **aktivitet Windows** lista.

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Du kan visa hello kopiera kör information i hello **aktivitet fönstret Explorer** i hello högra panelen, inklusive hello datavolym läses från källan och skrivs till målet, varaktighet och hello genomsnittlig genomströmning för hello kör.

    Som du kan se hello följande skärmdump, kopierar 1 TB från Azure Blob Storage till SQL Data Warehouse tog 14 minuter, ett effektivt sätt att uppnå 1,22 Gbit/s genomströmning!

    ![Guiden Kopiera - lyckades dialogrutan](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Bästa praxis
Här följer några Metodtips för att köra din Azure SQL Data Warehouse-databas:

* Använd en större resursklassen när inläsning till ett GRUPPERAT COLUMNSTORE-INDEX.
* Överväg att använda hash-distribution genom att markera kolumn i stället för default round robin distribution för effektivare kopplingar.
* Överväg att använda heap för tillfälliga data för högre belastning hastighet.
* Skapa statistik när du är klar läser in Azure SQL Data Warehouse.

Se [Metodtips för Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) mer information.

## <a name="next-steps"></a>Nästa steg
* [Guiden för data Factory kopiera](data-factory-copy-wizard.md) -den här artikeln innehåller information om hello guiden Kopiera.
* [Kopiera aktivitet prestanda och prestandajustering guide](data-factory-copy-activity-performance.md) -den här artikeln innehåller prestandajustering guide och hello referens prestandamått.
