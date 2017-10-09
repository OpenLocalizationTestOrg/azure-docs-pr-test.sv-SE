---
title: aaaConnect tooAzure SQL Data Warehouse - SSMS | Microsoft Docs
description: "Använd SQL Server Management Studio (SSMS) tooconnect tooand fråga Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: bcbaf7139d2e5183b388b8d58c015cf5ad726722
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a>Ansluta tooSQL Data Warehouse med SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Använd SQL Server Management Studio (SSMS) tooconnect tooand fråga Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Krav
toouse den här kursen behöver du:

* Ett befintligt SQL Data Warehouse. toocreate, se [skapa ett SQL Data Warehouse][Create a SQL Data Warehouse].
* SQL Server Management Studio (SSMS) installerat. [Installera SSMS] [ Install SSMS] kostnadsfritt om det inte redan har.
* hello fullständigt kvalificerade namn för SQL server. toofind detta, se [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Ansluta tooyour SQL Data Warehouse
1. Öppna SSMS.
2. Öppna Object Explorer. toodo detta, Välj **filen** > **ansluta Object Explorer**.
   
    ![SQL Server Object Explorer][1]
3. Fyll i hello i hello Anslut tooServer fönster.
   
    ![Ansluta tooServer][2]
   
   * **Servernamn**. Ange hello **servernamn** konstaterats tidigare.
   * **Autentisering**. Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.
   * **Användarnamn** och **lösenord**. Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.
   * Klicka på **Anslut**.
4. tooexplore, expandera din Azure SQL-server. Du kan visa hello databaser som är associerade med hello-servern. Expandera AdventureWorksDW toosee hello tabeller i din exempeldatabas.
   
    ![Utforska AdventureWorksDW][3]

## <a name="2-run-a-sample-query"></a>2. Kör en exempelfråga
Nu när en anslutning har etablerat tooyour databasen, dags att skriva en fråga.

1. Högerklicka på din databas i SQL Server Object Explorer.
2. Välj **ny fråga**. Ett nytt frågefönster öppnas.
   
    ![Ny fråga][4]
3. Kopiera den här TSQL-frågan till frågefönstret hello:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Kör hello-fråga. toodo, genom att välja `Execute` eller Använd hello följande genväg: `F5`.
   
    ![Kör frågan][5]
5. Titta på hello frågeresultat. I det här exemplet har hello tabellen FactInternetSales 60398 rader.
   
    ![Frågeresultat][6]

## <a name="next-steps"></a>Nästa steg
Nu när du kan ansluta och fråga, försök [visualisera hello data med PowerBI][visualizing hello data with PowerBI].

tooconfigure din miljö för Azure Active Directory-autentisering, se [autentisera tooSQL datalagret][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
