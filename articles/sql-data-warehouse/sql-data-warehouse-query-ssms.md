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
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="1d54c-103">Ansluta tooSQL Data Warehouse med SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="1d54c-103">Connect tooSQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d54c-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="1d54c-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="1d54c-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1d54c-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="1d54c-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d54c-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="1d54c-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="1d54c-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="1d54c-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="1d54c-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="1d54c-109">Använd SQL Server Management Studio (SSMS) tooconnect tooand fråga Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1d54c-109">Use SQL Server Management Studio (SSMS) tooconnect tooand query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1d54c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1d54c-110">Prerequisites</span></span>
<span data-ttu-id="1d54c-111">toouse den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="1d54c-111">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="1d54c-112">Ett befintligt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="1d54c-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="1d54c-113">toocreate, se [skapa ett SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="1d54c-113">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="1d54c-114">SQL Server Management Studio (SSMS) installerat.</span><span class="sxs-lookup"><span data-stu-id="1d54c-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="1d54c-115">[Installera SSMS] [ Install SSMS] kostnadsfritt om det inte redan har.</span><span class="sxs-lookup"><span data-stu-id="1d54c-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="1d54c-116">hello fullständigt kvalificerade namn för SQL server.</span><span class="sxs-lookup"><span data-stu-id="1d54c-116">hello fully qualified SQL server name.</span></span> <span data-ttu-id="1d54c-117">toofind detta, se [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="1d54c-117">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="1d54c-118">1. Ansluta tooyour SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1d54c-118">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="1d54c-119">Öppna SSMS.</span><span class="sxs-lookup"><span data-stu-id="1d54c-119">Open SSMS.</span></span>
2. <span data-ttu-id="1d54c-120">Öppna Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="1d54c-120">Open Object Explorer.</span></span> <span data-ttu-id="1d54c-121">toodo detta, Välj **filen** > **ansluta Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-121">toodo this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="1d54c-123">Fyll i hello i hello Anslut tooServer fönster.</span><span class="sxs-lookup"><span data-stu-id="1d54c-123">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Ansluta tooServer][2]
   
   * <span data-ttu-id="1d54c-125">**Servernamn**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-125">**Server name**.</span></span> <span data-ttu-id="1d54c-126">Ange hello **servernamn** konstaterats tidigare.</span><span class="sxs-lookup"><span data-stu-id="1d54c-126">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="1d54c-127">**Autentisering**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-127">**Authentication**.</span></span> <span data-ttu-id="1d54c-128">Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="1d54c-129">**Användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-129">**User Name** and **Password**.</span></span> <span data-ttu-id="1d54c-130">Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.</span><span class="sxs-lookup"><span data-stu-id="1d54c-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="1d54c-131">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-131">Click **Connect**.</span></span>
4. <span data-ttu-id="1d54c-132">tooexplore, expandera din Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="1d54c-132">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="1d54c-133">Du kan visa hello databaser som är associerade med hello-servern.</span><span class="sxs-lookup"><span data-stu-id="1d54c-133">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="1d54c-134">Expandera AdventureWorksDW toosee hello tabeller i din exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="1d54c-134">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Utforska AdventureWorksDW][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="1d54c-136">2. Kör en exempelfråga</span><span class="sxs-lookup"><span data-stu-id="1d54c-136">2. Run a sample query</span></span>
<span data-ttu-id="1d54c-137">Nu när en anslutning har etablerat tooyour databasen, dags att skriva en fråga.</span><span class="sxs-lookup"><span data-stu-id="1d54c-137">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="1d54c-138">Högerklicka på din databas i SQL Server Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="1d54c-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="1d54c-139">Välj **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="1d54c-139">Select **New Query**.</span></span> <span data-ttu-id="1d54c-140">Ett nytt frågefönster öppnas.</span><span class="sxs-lookup"><span data-stu-id="1d54c-140">A new query window opens.</span></span>
   
    ![Ny fråga][4]
3. <span data-ttu-id="1d54c-142">Kopiera den här TSQL-frågan till frågefönstret hello:</span><span class="sxs-lookup"><span data-stu-id="1d54c-142">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="1d54c-143">Kör hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="1d54c-143">Run hello query.</span></span> <span data-ttu-id="1d54c-144">toodo, genom att välja `Execute` eller Använd hello följande genväg: `F5`.</span><span class="sxs-lookup"><span data-stu-id="1d54c-144">toodo this, click `Execute` or use hello following shortcut: `F5`.</span></span>
   
    ![Kör frågan][5]
5. <span data-ttu-id="1d54c-146">Titta på hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="1d54c-146">Look at hello query results.</span></span> <span data-ttu-id="1d54c-147">I det här exemplet har hello tabellen FactInternetSales 60398 rader.</span><span class="sxs-lookup"><span data-stu-id="1d54c-147">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Frågeresultat][6]

## <a name="next-steps"></a><span data-ttu-id="1d54c-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d54c-149">Next steps</span></span>
<span data-ttu-id="1d54c-150">Nu när du kan ansluta och fråga, försök [visualisera hello data med PowerBI][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="1d54c-150">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="1d54c-151">tooconfigure din miljö för Azure Active Directory-autentisering, se [autentisera tooSQL datalagret][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="1d54c-151">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

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
