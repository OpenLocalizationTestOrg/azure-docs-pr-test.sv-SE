---
title: Anslut till Azure SQL Data Warehouse - SSMS | Microsoft Docs
description: "Använda SQL Server Management Studio (SSMS) för att ansluta till och fråga Azure SQL Data Warehouse."
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
ms.openlocfilehash: 207fb9fd861c66039fbde89681aed3df3a2f4021
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="598c3-103">Anslut till SQL Data Warehouse med SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="598c3-103">Connect to SQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="598c3-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="598c3-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="598c3-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="598c3-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="598c3-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="598c3-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="598c3-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="598c3-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="598c3-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="598c3-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="598c3-109">Använda SQL Server Management Studio (SSMS) för att ansluta till och fråga Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="598c3-109">Use SQL Server Management Studio (SSMS) to connect to and query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="598c3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="598c3-110">Prerequisites</span></span>
<span data-ttu-id="598c3-111">Du behöver följande för att använda de här självstudierna:</span><span class="sxs-lookup"><span data-stu-id="598c3-111">To use this tutorial, you need:</span></span>

* <span data-ttu-id="598c3-112">Ett befintligt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="598c3-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="598c3-113">Se [Skapa ett SQL Data Warehouse][Create a SQL Data Warehouse] för att se hur man skapar det.</span><span class="sxs-lookup"><span data-stu-id="598c3-113">To create one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="598c3-114">SQL Server Management Studio (SSMS) installerat.</span><span class="sxs-lookup"><span data-stu-id="598c3-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="598c3-115">[Installera SSMS] [ Install SSMS] kostnadsfritt om det inte redan har.</span><span class="sxs-lookup"><span data-stu-id="598c3-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="598c3-116">Det fullständigt kvalificerade servernamnet.</span><span class="sxs-lookup"><span data-stu-id="598c3-116">The fully qualified SQL server name.</span></span> <span data-ttu-id="598c3-117">[Anslut till SQL Data Warehouse][Connect to SQL Data Warehouse] för att hitta detta.</span><span class="sxs-lookup"><span data-stu-id="598c3-117">To find this, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

## <a name="1-connect-to-your-sql-data-warehouse"></a><span data-ttu-id="598c3-118">1. Anslut till din SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="598c3-118">1. Connect to your SQL Data Warehouse</span></span>
1. <span data-ttu-id="598c3-119">Öppna SSMS.</span><span class="sxs-lookup"><span data-stu-id="598c3-119">Open SSMS.</span></span>
2. <span data-ttu-id="598c3-120">Öppna Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="598c3-120">Open Object Explorer.</span></span> <span data-ttu-id="598c3-121">Om du vill göra det, Välj **filen** > **ansluta Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="598c3-121">To do this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="598c3-123">Fyll i fälten i fönstret anslut till server.</span><span class="sxs-lookup"><span data-stu-id="598c3-123">Fill in the fields in the Connect to Server window.</span></span>
   
    ![Anslut till server][2]
   
   * <span data-ttu-id="598c3-125">**Servernamn**.</span><span class="sxs-lookup"><span data-stu-id="598c3-125">**Server name**.</span></span> <span data-ttu-id="598c3-126">Ange det **servernamn** som du identifierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="598c3-126">Enter the **server name** previously identified.</span></span>
   * <span data-ttu-id="598c3-127">**Autentisering**.</span><span class="sxs-lookup"><span data-stu-id="598c3-127">**Authentication**.</span></span> <span data-ttu-id="598c3-128">Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.</span><span class="sxs-lookup"><span data-stu-id="598c3-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="598c3-129">**Användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="598c3-129">**User Name** and **Password**.</span></span> <span data-ttu-id="598c3-130">Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.</span><span class="sxs-lookup"><span data-stu-id="598c3-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="598c3-131">Klicka på **anslut**.</span><span class="sxs-lookup"><span data-stu-id="598c3-131">Click **Connect**.</span></span>
4. <span data-ttu-id="598c3-132">Expandera din Azure SQL-server för att utforska.</span><span class="sxs-lookup"><span data-stu-id="598c3-132">To explore, expand your Azure SQL server.</span></span> <span data-ttu-id="598c3-133">Du kan se de databaser som är associerade med servern.</span><span class="sxs-lookup"><span data-stu-id="598c3-133">You can view the databases associated with the server.</span></span> <span data-ttu-id="598c3-134">Expandera AdventureWorksDW för att se tabellerna i din exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="598c3-134">Expand AdventureWorksDW to see the tables in your sample database.</span></span>
   
    ![Utforska AdventureWorksDW][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="598c3-136">2. Kör en exempelfråga</span><span class="sxs-lookup"><span data-stu-id="598c3-136">2. Run a sample query</span></span>
<span data-ttu-id="598c3-137">När du nu etablerat en anslutning till din databas, är det dags att skriva en fråga.</span><span class="sxs-lookup"><span data-stu-id="598c3-137">Now that a connection has been established to your database, let's write a query.</span></span>

1. <span data-ttu-id="598c3-138">Högerklicka på din databas i SQL Server Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="598c3-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="598c3-139">Välj **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="598c3-139">Select **New Query**.</span></span> <span data-ttu-id="598c3-140">Ett nytt frågefönster öppnas.</span><span class="sxs-lookup"><span data-stu-id="598c3-140">A new query window opens.</span></span>
   
    ![Ny fråga][4]
3. <span data-ttu-id="598c3-142">Kopiera den här TSQL-frågan till frågefönstret:</span><span class="sxs-lookup"><span data-stu-id="598c3-142">Copy this TSQL query into the query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="598c3-143">Kör frågan.</span><span class="sxs-lookup"><span data-stu-id="598c3-143">Run the query.</span></span> <span data-ttu-id="598c3-144">Gör detta genom att klicka på `Execute` eller använder följande genväg: `F5`.</span><span class="sxs-lookup"><span data-stu-id="598c3-144">To do this, click `Execute` or use the following shortcut: `F5`.</span></span>
   
    ![Kör frågan][5]
5. <span data-ttu-id="598c3-146">Titta på frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="598c3-146">Look at the query results.</span></span> <span data-ttu-id="598c3-147">I det här exemplet har tabellen FactInternetSales 60398 rader.</span><span class="sxs-lookup"><span data-stu-id="598c3-147">In this example, the FactInternetSales table has 60398 rows.</span></span>
   
    ![Frågeresultat][6]

## <a name="next-steps"></a><span data-ttu-id="598c3-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="598c3-149">Next steps</span></span>
<span data-ttu-id="598c3-150">Nu när du anslutit och frågat, kan du [visualisera dina data med PowerBI][visualizing the data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="598c3-150">Now that you can connect and query, try [visualizing the data with PowerBI][visualizing the data with PowerBI].</span></span>

<span data-ttu-id="598c3-151">För att konfigurera din miljö för Azure Active Directory-autentisering, se [Autentisera till SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="598c3-151">To configure your environment for Azure Active Directory authentication, see [Authenticate to SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

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
