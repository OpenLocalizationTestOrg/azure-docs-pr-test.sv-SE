---
title: aaaUse Redgate tooload data tooyour Azure data warehouse | Microsoft Docs
description: "Lär dig hur toouse Redgate Data Platform Studio för informationslagerscenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a>Läs in data med Redgates Data Platform Studio
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

De här självstudierna visar hur toouse [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DP) toomove data från en lokal SQL Server-tooAzure SQL Data Warehouse. Data Platform Studio gäller hello lämpligaste kompatibilitetsproblem och optimeringar, vilket ger snabbast hello tooget sätt igång med SQL Data Warehouse.

> [!NOTE]
> [Redgate](http://www.red-gate.com) tillhandahåller olika SQL Server-verktyg, och är sedan länge Microsoft-partner. Den här funktionen i Data Platform Studio har gjorts tillgängligt kostnadsfritt för såväl kommersiell och icke-kommersiell användning.
> 
> 

## <a name="before-you-begin"></a>Innan du börjar
### <a name="create-or-identify-resources"></a>Skapa eller identifiera resurser
Innan du påbörjar den här kursen behöver du toohave:

* **lokal SQL Server-databas**: hello data som du vill tooimport tooSQL datalagret måste toocome från en lokal SQL Server (version 2008R2 eller senare). Data Platform Studio kan inte importera data direkt från en Azure SQL Database eller från textfiler.
* **Azure Storage-konto**: Data Platform Studio skapar etapper hello data i Azure Blob Storage innan du läser in den i SQL Data Warehouse. hello storage-konto måste använda hello ”Resource Manager”-modellen (hello standard) i stället för hello ”klassisk” distributionsmodell. Om du inte har ett lagringskonto kan du lära dig hur tooCreate ett lagringskonto. 
* **SQL Data Warehouse**: självstudierna flyttas hello data från lokala SQL Server-tooSQL datalagret, så du måste toohave ett data warehouse online. Om du inte redan har ett data warehouse, lär du dig hur tooCreate ett Azure SQL Data Warehouse.

> [!NOTE]
> Bättre prestanda om hello storage-konto och hello datalagret skapas i hello samma region.
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a>Steg 1: Logga in tooData plattform Studio med ditt Azure-konto
Öppna webbläsaren och gå toohello [Data Platform Studio](https://www.dataplatformstudio.com/) webbplats. Logga in med hello samma Azure-konto som du använt toocreate hello storage-konto och för datalagret. Om din e-postadress är associerad med ett arbets eller skolkonto och ett Microsoft-konto kan vara att toochoose hello konto som har åtkomst till tooyour resurser.

> [!NOTE]
> Om det här är första gången du använder Data Platform Studio är och toogrant hello program behörighet toomanage Azure-resurser.
> 
> 

## <a name="step-2-start-hello-import-wizard"></a>Steg 2: Starta hello guiden Importera
Välj hello importera tooAzure SQL Data Warehouse länken toostart hello guiden Importera hello DP huvudskärmen.

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a>Steg 3: Installera hello Data Platform Studio Gateway
tooconnect tooyour lokala SQL Server-databas måste tooinstall hello DP-Gateway. hello-gateway är en klientagent som ger åtkomst tooyour lokala miljö, extraherar hello data och överför det tooyour storage-konto. Dina data passerar aldrig Redgates servrar. tooinstall hello Gateway:

1. Klicka på hello **skapa Gateway** länk
2. Hämta och installera hello en Gateway med hjälp av hello angivna installer

![][2]

> [!NOTE]
> hello Gateway kan installeras på en dator med network access toohello källa SQL Server-databas. Den kommer åt hello SQL Server-databas med hjälp av Windows-autentisering med hello autentiseringsuppgifterna för hello aktuella användaren.
> 
> 

När du har installerat hello Gateway status ändringar tooConnected och du kan välja Nästa.

## <a name="step-4-identify-hello-source-database"></a>Steg 4: Identifiera hello källdatabasen
I hello *ange servernamnet* textruta anger hello namnet på hello-server som är värd för databasen och välj **nästa**. Välj hello-databasen som du vill tooimport data från hello nedrullningsbara menyn.

![][3]

DP kontrollerar hello valda databasen för tabeller tooimport. Som standard importerar DP alla hello tabeller i hello-databasen. Du kan markera eller avmarkera tabeller genom att expandera hello länka alla tabeller. Välj hello nästa knapp toomove framåt.

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a>Steg 5: Välj en storage-konto toostage hello-data
DP uppmanas du att en plats toostage hello data. Välj ett befintligt lagringskonto från din prenumeration och välj **Nästa**.

> [!NOTE]
> DP skapar en ny blobbbehållare i hello valt storage-konto och använda en distinkta mapp för varje import.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Steg 6: Välj ett informationslager
Sedan väljer du ett online [Azure SQL Data Warehouse](http://aka.ms/sqldw) tooimport hello data till databasen. När du har valt databasen måste tooenter hello autentiseringsuppgifter tooconnect toohello databasen och välj **nästa**.

![][5]

> [!NOTE]
> DP sammanfogas hello källtabellerna data till hello-datalagret. DP varnar dig om kräver hello tabellnamn toooverwrite befintliga tabeller i hello-datalagret. Du kan eventuellt ta bort alla befintliga objekt i datalager hello genom markeringar ta bort alla befintliga objekt före import.
> 
> 

## <a name="step-7-import-hello-data"></a>Steg 7: Importera hello data
DP bekräftar att du vill ha tooimport hello data. Klicka bara på hello Start importera knappen toobegin hello import av data.

![][6]

DP visar en visualisering som visar hello förloppet extrahera och överföra hello data från hello lokala SQL Server och hello fortskrider hello import till SQL Data Warehouse.

![][7]

När hello importen är klar visar DP en sammanfattning av hello dataimporten och en rapport om hello kompatibilitet korrigeringar som har utförts.

![][8]

## <a name="next-steps"></a>Nästa steg
tooexplore data inom SQL Data Warehouse starta genom att visa:

* [Fråga Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Visualisera data med Power BI][Visualize data with Power BI]

Mer om Redgate's Data Platform Studio toolearn:

* [Besök hello DP-webbsida](http://www.dataplatformstudio.com/)
* [Titta på en demonstration av DPS på Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

En översikt över andra sätt toomigrate och Läs in finns data i SQL Data Warehouse:

* [Migrera din lösning tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse]
* [Läs in data till Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md)

För fler utvecklingstips, se hello [översikt över SQL Data Warehouse-utveckling](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
