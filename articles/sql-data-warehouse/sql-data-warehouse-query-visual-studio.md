---
title: aaaConnect tooAzure SQL Data Warehouse - VSTS | Microsoft Docs
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
ms.openlocfilehash: 55eef4dff3e0647be5a735295bc89b43eb456079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a>Anslut tooSQL Data Warehouse med Visual Studio och SSDT
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Använd Visual Studio tooquery Azure SQL Data Warehouse på bara några minuter. Den här metoden använder hello SQL Server Data Tools (SSDT)-tillägget i Visual Studio. 

## <a name="prerequisites"></a>Krav
toouse den här kursen behöver du:

* Ett befintligt SQL Data Warehouse. toocreate, se [skapa ett SQL Data Warehouse][Create a SQL Data Warehouse].
* SSDT för Visual Studio. Om du har Visual Studio har du förmodligen redan det här. Installationsinstruktioner och alternativ finns i [Installera Visual Studio och SSDT][Installing Visual Studio and SSDT].
* hello fullständigt kvalificerade namn för SQL server. toofind detta, se [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Ansluta tooyour SQL Data Warehouse
1. Öppna Visual Studio 2013 eller 2015.
2. Öppna SQL Server Object Explorer. toodo detta, Välj **visa** > **SQL Server Object Explorer**.
   
    ![SQL Server Object Explorer][1]
3. Klicka på hello **Lägg till SQL Server** ikon.
   
    ![Lägg till SQL Server][2]
4. Fyll i hello i hello Anslut tooServer fönster.
   
    ![Ansluta tooServer][3]
   
   * **Servernamn**. Ange hello **servernamn** konstaterats tidigare.
   * **Autentisering**. Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.
   * **Användarnamn** och **lösenord**. Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.
   * Klicka på **Anslut**.
5. tooexplore, expandera din Azure SQL-server. Du kan visa hello databaser som är associerade med hello-servern. Expandera AdventureWorksDW toosee hello tabeller i din exempeldatabas.
   
    ![Utforska AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. Kör en exempelfråga
Nu när en anslutning har etablerat tooyour databasen, dags att skriva en fråga.

1. Högerklicka på din databas i SQL Server Object Explorer.
2. Välj **ny fråga**. Ett nytt frågefönster öppnas.
   
    ![Ny fråga][5]
3. Kopiera den här TSQL-frågan till frågefönstret hello:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Kör hello-fråga. toodo, klicka på hello gröna pilen eller använder följande genväg hello: `CTRL` + `SHIFT` + `E`.
   
    ![Kör frågan][6]
5. Titta på hello frågeresultat. I det här exemplet har hello tabellen FactInternetSales 60398 rader.
   
    ![Frågeresultat][7]

## <a name="next-steps"></a>Nästa steg
Nu när du kan ansluta och fråga, försök [visualisera hello data med PowerBI][visualizing hello data with PowerBI].

tooconfigure din miljö för Azure Active Directory-autentisering, se [autentisera tooSQL datalagret][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

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
