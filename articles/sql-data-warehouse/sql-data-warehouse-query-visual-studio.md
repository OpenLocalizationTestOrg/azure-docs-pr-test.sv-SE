---
title: "Ansluta till Azure SQL Data Warehouse – VSTS | Microsoft Docs"
description: "Fråga SQL Data Warehouse med Visual Studio."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 1e44c6c3c47034a892753c69c5ef22a5eac18c0d
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="connect-to-sql-data-warehouse-with-visual-studio-and-ssdt"></a>Ansluta till SQL Data Warehouse med Visual Studio och SSDT
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Använd Visual Studio för att ställa frågor till Azure SQL Data Warehouse på bara några minuter. Den här metoden använder tillägget SQL Server Data Tools (SSDT) i Visual Studio. 

## <a name="prerequisites"></a>Krav
Du behöver följande för att använda de här självstudierna:

* Ett befintligt SQL Data Warehouse. Se [Skapa ett SQL Data Warehouse][Create a SQL Data Warehouse] för att se hur man skapar det.
* SSDT för Visual Studio. Om du har Visual Studio har du förmodligen redan det här. Installationsinstruktioner och alternativ finns i [Installera Visual Studio och SSDT][Installing Visual Studio and SSDT].
* Det fullständigt kvalificerade servernamnet. [Anslut till SQL Data Warehouse][Connect to SQL Data Warehouse] för att hitta detta.

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Anslut till din SQL Data Warehouse
1. Öppna Visual Studio 2013 eller 2015.
2. Öppna SQL Server Object Explorer. Gör det genom att markera **Visa** > **SQL Server Object Explorer**.
   
    ![SQL Server Object Explorer][1]
3. Klicka på ikonen **lägg till SQL Server**.
   
    ![Lägg till SQL Server][2]
4. Fyll i fälten i fönstret anslut till server.
   
    ![Anslut till server][3]
   
   * **Servernamn**. Ange det **servernamn** som du identifierade tidigare.
   * **Autentisering**. Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.
   * **Användarnamn** och **lösenord**. Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.
   * Klicka på **anslut**.
5. Expandera din Azure SQL-server för att utforska. Du kan se de databaser som är associerade med servern. Expandera AdventureWorksDW för att se tabellerna i din exempeldatabas.
   
    ![Utforska AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. Kör en exempelfråga
När du nu etablerat en anslutning till din databas, är det dags att skriva en fråga.

1. Högerklicka på din databas i SQL Server Object Explorer.
2. Välj **ny fråga**. Ett nytt frågefönster öppnas.
   
    ![Ny fråga][5]
3. Kopiera den här TSQL-frågan till frågefönstret:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Kör frågan. För att göra det, klickar du på den gröna pilen eller använder följande genväg: `CTRL`+`SHIFT`+`E`.
   
    ![Kör frågan][6]
5. Titta på frågeresultaten. I det här exemplet har tabellen FactInternetSales 60398 rader.
   
    ![Frågeresultat][7]

## <a name="next-steps"></a>Nästa steg
Nu när du anslutit och frågat, kan du [visualisera dina data med PowerBI][visualizing the data with PowerBI].

För att konfigurera din miljö för Azure Active Directory-autentisering, se [Autentisera till SQL Data Warehouse][Authenticate to SQL Data Warehouse].

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
