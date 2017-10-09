---
title: aaaVisualize SQL Data Warehouse-data med Power BI Microsoft Azure
description: Visualisera SQL Data Warehouse-data med Power BI
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a>Visualisera data med Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

De här självstudierna visar hur toouse Power BI tooconnect tooSQL Data Warehouse och skapa några grundläggande visualiseringar.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Krav
toostep igenom den här kursen behöver du:

* Ett SQL Data Warehouse förinstallerad med hello AdventureWorksDW-databasen. tooprovision detta, se [skapa ett SQL Data Warehouse] [ Create a SQL Data Warehouse] och välj tooload hello exempeldata. Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].

## <a name="1-connect-tooyour-database"></a>1. Ansluta tooyour databas
tooopen Power BI och ansluta tooyour AdventureWorksDW-databasen:

1. Logga in på hello [Azure-portalen][Azure portal].
2. Klicka på **SQL-databaser** och välj din AdventureWorks SQL Data Warehouse-databas.
   
    ![Hitta din databas][1]
3. Klicka på hello ”öppna i Power BI-knappen.
   
    ![Power BI-knappen][2]
4. Du bör nu se hello SQL Data Warehouse anslutningssidan som visar webbadressen för databasen. Klicka på Nästa.
   
    ![Power BI-anslutning][3]
5. Ange din Azure SQL server-användarnamn och lösenord och du blir fullt ansluten tooyour SQL Data Warehouse-databas.
   
    ![Power BI-inloggning][4]
6. Klicka på hello AdventureWorksDW-datauppsättningen på hello vänstra bladet när du har loggat in på Power BI. Hello databasen öppnas.
   
    ![Power BI öppna AdventureWorksDW][5]

## <a name="2-create-a-report"></a>2. Skapa en rapport
Du är nu redo toouse Power BI tooanalyze AdventureWorksDW-exempeldata. tooperform hello analysen har AdventureWorksDW en vy som heter AggregateSales. Den här vyn innehåller några hello viktiga mått för att analysera hello försäljningen av hello företag.

1. toocreate en karta över försäljningsbelopp enligt toopostal kod i hello högra rutan, klicka på hello AggregateSales visa tooexpand den. Klicka på hello PostalCode och SalesAmount kolumner tooselect dem.
   
    ![Power BI välj AggregateSales][6]
   
    Power BI identifierar automatiskt dessa geografiska data och placerar dem på en karta för dig.
   
    ![Power BI-karta][7]
2. Det här steget skapar ett stapeldiagram som visar försäljningsintäkter efter kund. toocreate denna gå toohello expanderas AggregateSales-vyn. Klicka på hello SalesAmount fält. Dra hello Customer Income fältet toohello vänster och släpp det i axeln.
   
    ![Power BI välj axel][8]
   
    Vi har flyttat hello liggande diagram över hello vänster.
   
    ![Power BI-stapel][9]
3. Det här steget skapar ett linjediagram som visar försäljning per beställningsdatum. toocreate denna gå toohello expanderas AggregateSales-vyn. Klicka på SalesAmount och OrderDate. Klicka i hello visualiseringar kolumnen hello linjediagram-ikonen. Detta är hello första ikonen i hello andra raden under visualiseringar.
   
    ![Power BI välj linjediagram][10]
   
    Nu har du en rapport som visar tre olika datavisualiseringar hello data.
   
    ![Power BI-linje][11]

Du kan spara ditt arbete när som helst genom att klicka på **Arkiv** och välja **Spara**.

## <a name="next-steps"></a>Nästa steg
Nu när vi har gett dig vissa tid toowarm med exempeldata hello, se hur för[utveckla][develop], [ladda][load], eller [ Migrera][migrate]. Eller titta på hello [Power BI-webbplatsen][Power BI website].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
