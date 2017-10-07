---
title: "aaaLoad data till Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "Den här kursen läser in data i Azure SQL Data Warehouse med hjälp av Azure Data Factory och använder en SQL Server-databas som hello datakälla."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Läs in data till SQL Data Warehouse med Data Factory

Du kan använda Azure Data Factory tooload data till Azure SQL Data Warehouse från någon av hello [stöds källa datalager](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats). T.ex, du kan läsa in data från en Azure SQL-databas eller en Oracle-databas till en SQL data warehouse med hjälp av Data Factory. Kursen i den här artikeln visar hur tooload data från en lokal SQL Server-databas till en SQL data warehouse.

**Tid uppskattning**: den här kursen tar 10 – 15 minuter toocomplete när hello krav är uppfyllda.

## <a name="prerequisites"></a>Krav

- Du behöver en **SQL Server-databas** med tabeller som innehåller hello data kopieras toobe via toohello SQL data warehouse.  

- Du behöver ett online **SQL Data Warehouse**. Om du inte redan har ett data warehouse lär du dig hur för[skapar ett Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).

- Du behöver ett **Azure Storage-konto**. Om du inte redan har ett lagringskonto, lär du dig hur för[skapa ett lagringskonto](../storage/common/storage-create-storage-account.md). Leta upp hello storage-konto för bästa prestanda och hello datalagret i hello samma Azure-region.

## <a name="configure-a-data-factory"></a>Konfigurera en datafabrik
1. Logga in toohello [Azure-portalen][].
2. Leta upp ditt data warehouse och på tooopen den.
3. I hello huvudblad, klickar du på **Läs in Data** > **Azure Data Factory**.

    ![Starta guiden Läs in Data](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Om du inte har en datafabrik i din Azure-prenumeration visas en **nya Data Factory** dialogrutan i en separat flik i webbläsaren hello. Fyll i hello information som efterfrågas och klicka på **skapa**. När du har skapat hello data factory hello **nya Data Factory** dialogrutan stängs och du ser hello **Välj Data Factory** dialogrutan.

    Om du har en eller flera datafabriker redan i hello Azure-prenumeration kan du se hello **Välj Data Factory** dialogrutan. I den här dialogrutan kan du antingen välja en befintlig datafabrik eller klicka på **skapa nya data factory** toocreate en ny.

    ![Konfigurera Data Factory](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. I hello **Välj Data Factory** dialogrutan, hello **läsa in data** alternativet är markerat som standard. Klicka på **nästa** toostart att skapa en aktivitet för datainläsning.

## <a name="configure-hello-data-factory-properties"></a>Konfigurera egenskaper för hello data factory
Nu när du har skapat en datafabrik är hello nästa steg tooconfigure hello datainläsning schema.

1. För **aktivitet**, ange **DWLoadData fromSQLServer**.
2. Använd hello standard **kör nu en gång** klickar du på **nästa**.

    ![Konfigurera belastningen schema](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a>Konfigurera datalager för hello källa och gateway
Nu kan du se Data Factory om hello lokala SQL Server-databas som du vill tooload data.

1. Välj **SQL Server** från hello stöds källdata lagra katalog och klicka på **nästa**.

    ![Välj källa för SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. En **ange hello lokala SQL Server-databas** visas. Hej först **anslutningsnamn** är fältet fylls i automatiskt. hello andra fältet Begär hello namnet på hello **Gateway**. Om du använder en befintlig datafabrik som redan har en gateway kan du återanvända hello gateway genom att välja den hello listrutan. Klicka på hello **skapa Gateway** länka toocreate en Data Management Gateway.  

    > [!NOTE]
    > Om hello källdata lagrar lokalt eller i en virtuell Azure IaaS-dator, en Data Management Gateway måste anges. En gateway har en 1-1-relation med en datafabrik. Det går inte att använda från en annan data factory, men den kan användas av flera uppgifter med i hello för datainläsning samma data factory. En gateway kan vara används tooconnect toomultiple datalager när du kör uppgifter för datainläsning.
    >
    > Detaljerad information om hello gateway finns [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) artikel.

3. En **skapa Gateway** dialogrutan visas. Namn, ange **GatewayForDWLoading**, och klicka på **skapa**.

4. En **konfigurera Gateway** dialogrutan visas. Klicka på **starta Expressinstallationen på den här datorn** tooautomatically hämta, installera och registrera Data Management Gateway på den aktuella datorn. hello förloppet visas i ett popup-fönster. Om hello datorn inte kan ansluta toohello datalagret, kan du manuellt [ladda ned och installera hello gateway](https://www.microsoft.com/download/details.aspx?id=39717) lagrar på en dator som kan ansluta toohello data och sedan använder hello viktiga tooregister.
    > [!NOTE]
    > hello Expressinstallationen fungerar internt med Microsoft Edge och Internet Explorer. Om du använder Google Chrome, måste du först installera hello ClickOnce-tillägget från Chrome web store.

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. Vänta tills hello gateway installationsprogrammet toocomplete. När hello gatewayen har registrerats och är online, hello popup-fönstret stängs och hello ny gateway visas i fältet för hello-gateway. Sedan fyller du i hello rest obligatoriska fält enligt följande och klicka sedan på **nästa**.
    - **Servernamnet**: namnet på hello lokala SQL Server.
    - **Databasnamnet**: SQL Server-databas.
    - **Autentiseringsuppgifter kryptering**: Använd hello standard ”av webbläsaren”.
    - **Autentiseringstypen**: Välj hello typ av autentisering som du använder.
    - **Användarnamnet** och **lösenord**: Ange hello användarnamn och lösenord för en användare som har behörigheten toocopy hello data.

    ![Starta installationen av Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. hello nästa steg är toochoose hello tabeller från vilka toocopy hello data. Du kan filtrera hello tabeller med hjälp av nyckelord. Och du kan förhandsgranska hello data och tabellen schemat i hello nedre panelen. När du slutför ditt val klickar du på **nästa**.

    ![Välj tabeller](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a>Konfigurera hello mål, ditt SQL Data Warehouse

Nu kan du se Data Factory om hello målinformation.

1. SQL Data Warehouse anslutningsinformationen fylls i automatiskt. Ange hello lösenord för hello användarnamn. och klicka på **nästa**.

    ![Konfigurera mål](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. Mappningen för en intelligent tabellen visas som mappar toodestination källtabellerna baserat på namn. Om hello tabellen inte finns i hello målet, som standard ADF skapar en med hello samma namn (Detta gäller tooSQL Server eller Azure SQL Database som källa). Du kan också välja toomap tooan befintlig tabell. Granska och klicka på **nästa**.

    ![Mappa tabeller](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. Granska hello schemamappning och leta efter fel eller varningsmeddelanden. Intelligent mappningen baserat på kolumnnamn. Om det finns en typkonvertering för data som inte stöds mellan hello käll- och kolumnen, visas ett felmeddelande om du tillsammans med motsvarande hello-tabellen. Om du väljer toolet Data Factory automatiskt skapa hello tabeller, rätt datatypkonvertering kan inträffa om det behövs toofix hello inkompatibilitet mellan käll- och Arkiv.

    ![Mappa schema](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. Klicka på **Nästa**.

## <a name="configure-hello-performance-settings"></a>Konfigurera hello prestandainställningar
Hello prestandakonfigurationer kan du konfigurera ett Azure storage-konto som används för mellanlagring hello data innan den läses in i SQL Data Warehouse performantly med [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly). Därefter kopiera hello rensas hello mellanliggande data i lagring automatiskt.

Välj ett befintligt Azure Storage-konto och klicka på **nästa**.

![Konfigurera fristående blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a>Granska sammanfattningsinformation och distribuera hello pipeline

Granska hello konfigurationen och klicka på **Slutför** knappen toodeploy hello pipeline.

![Distribuera data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Övervaka förloppet för datainläsning

Du kan se hello förlopp och resultat i hello **distribution** sidan.

1. När hello distributionen är klar klickar du på hello länken **Klicka här toomonitor kopiera pipeline** toomonitor datainläsning pågår.

    ![Övervaka pipeline](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. hello nyskapad **DWLoadData fromSQLServer** pipeline för inläsning av data är automatisk från hello vänstra **Resursläsaren**.

    ![Visa pipeline](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. Klicka till hello pipeline hello mitten panelen toosee hello detaljerad status för varje tabell som mappar tooan aktivitet.

    ![Visa tabellen aktivitet](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. Ytterligare Klicka i en aktivitet och du ser hello datainläsning information i hello högra panelen, inklusive datastorleken, rader, genomflöde och så vidare.

    ![Visa tabellen Aktivitetsinformation](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. toolaunch denna övervakning visa senare, gå tooyour SQL Data Warehouse, klicka på **Läs in Data > Azure Data Factory**, Välj din factory och välj **övervaka befintliga läser in aktiviteter**.

## <a name="next-steps"></a>Nästa steg

toomigrate din databas tooSQL datalagret, se [migreringsöversikt](sql-data-warehouse-overview-migrate.md).

toolearn mer om Azure Data Factory och dess funktioner för flytt av data, se hello följande artiklar:

- [Introduktion tooAzure Data Factory](../data-factory/data-factory-introduction.md)
- [Flytta data med hjälp av Kopieringsaktiviteten](../data-factory/data-factory-data-movement-activities.md)
- [Flytta data tooand från Azure SQL Data Warehouse med Azure Data Factory](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

tooexplore dina data i SQL Data Warehouse finns hello följande artiklar:

- [Anslut tooSQL Data Warehouse med Visual Studio och SSDT](sql-data-warehouse-query-visual-studio.md)
- [Visuella data med Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[Azure-portalen]: https://portal.azure.com
