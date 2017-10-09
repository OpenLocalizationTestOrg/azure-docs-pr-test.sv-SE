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
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a><span data-ttu-id="291f3-103">Anslut tooSQL Data Warehouse med Visual Studio och SSDT</span><span class="sxs-lookup"><span data-stu-id="291f3-103">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="291f3-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="291f3-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="291f3-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="291f3-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="291f3-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="291f3-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="291f3-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="291f3-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="291f3-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="291f3-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="291f3-109">Använd Visual Studio tooquery Azure SQL Data Warehouse på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="291f3-109">Use Visual Studio tooquery Azure SQL Data Warehouse in just a few minutes.</span></span> <span data-ttu-id="291f3-110">Den här metoden använder hello SQL Server Data Tools (SSDT)-tillägget i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="291f3-110">This method uses hello SQL Server Data Tools (SSDT) extension in Visual Studio.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="291f3-111">Krav</span><span class="sxs-lookup"><span data-stu-id="291f3-111">Prerequisites</span></span>
<span data-ttu-id="291f3-112">toouse den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="291f3-112">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="291f3-113">Ett befintligt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="291f3-113">An existing SQL data warehouse.</span></span> <span data-ttu-id="291f3-114">toocreate, se [skapa ett SQL Data Warehouse][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="291f3-114">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="291f3-115">SSDT för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="291f3-115">SSDT for Visual Studio.</span></span> <span data-ttu-id="291f3-116">Om du har Visual Studio har du förmodligen redan det här.</span><span class="sxs-lookup"><span data-stu-id="291f3-116">If you have Visual Studio, you probably already have this.</span></span> <span data-ttu-id="291f3-117">Installationsinstruktioner och alternativ finns i [Installera Visual Studio och SSDT][Installing Visual Studio and SSDT].</span><span class="sxs-lookup"><span data-stu-id="291f3-117">For installation instructions and options, see [Installing Visual Studio and SSDT][Installing Visual Studio and SSDT].</span></span>
* <span data-ttu-id="291f3-118">hello fullständigt kvalificerade namn för SQL server.</span><span class="sxs-lookup"><span data-stu-id="291f3-118">hello fully qualified SQL server name.</span></span> <span data-ttu-id="291f3-119">toofind detta, se [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="291f3-119">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="291f3-120">1. Ansluta tooyour SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="291f3-120">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="291f3-121">Öppna Visual Studio 2013 eller 2015.</span><span class="sxs-lookup"><span data-stu-id="291f3-121">Open Visual Studio 2013 or 2015.</span></span>
2. <span data-ttu-id="291f3-122">Öppna SQL Server Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="291f3-122">Open SQL Server Object Explorer.</span></span> <span data-ttu-id="291f3-123">toodo detta, Välj **visa** > **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="291f3-123">toodo this, select **View** > **SQL Server Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="291f3-125">Klicka på hello **Lägg till SQL Server** ikon.</span><span class="sxs-lookup"><span data-stu-id="291f3-125">Click hello **Add SQL Server** icon.</span></span>
   
    ![Lägg till SQL Server][2]
4. <span data-ttu-id="291f3-127">Fyll i hello i hello Anslut tooServer fönster.</span><span class="sxs-lookup"><span data-stu-id="291f3-127">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Ansluta tooServer][3]
   
   * <span data-ttu-id="291f3-129">**Servernamn**.</span><span class="sxs-lookup"><span data-stu-id="291f3-129">**Server name**.</span></span> <span data-ttu-id="291f3-130">Ange hello **servernamn** konstaterats tidigare.</span><span class="sxs-lookup"><span data-stu-id="291f3-130">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="291f3-131">**Autentisering**.</span><span class="sxs-lookup"><span data-stu-id="291f3-131">**Authentication**.</span></span> <span data-ttu-id="291f3-132">Välj **SQL Server-autentisering** eller **Active Directory-integrerad autentisering**.</span><span class="sxs-lookup"><span data-stu-id="291f3-132">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="291f3-133">**Användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="291f3-133">**User Name** and **Password**.</span></span> <span data-ttu-id="291f3-134">Ange användarnamn och lösenord om du valde SQL Server-autentisering ovan.</span><span class="sxs-lookup"><span data-stu-id="291f3-134">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="291f3-135">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="291f3-135">Click **Connect**.</span></span>
5. <span data-ttu-id="291f3-136">tooexplore, expandera din Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="291f3-136">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="291f3-137">Du kan visa hello databaser som är associerade med hello-servern.</span><span class="sxs-lookup"><span data-stu-id="291f3-137">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="291f3-138">Expandera AdventureWorksDW toosee hello tabeller i din exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="291f3-138">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Utforska AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="291f3-140">2. Kör en exempelfråga</span><span class="sxs-lookup"><span data-stu-id="291f3-140">2. Run a sample query</span></span>
<span data-ttu-id="291f3-141">Nu när en anslutning har etablerat tooyour databasen, dags att skriva en fråga.</span><span class="sxs-lookup"><span data-stu-id="291f3-141">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="291f3-142">Högerklicka på din databas i SQL Server Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="291f3-142">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="291f3-143">Välj **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="291f3-143">Select **New Query**.</span></span> <span data-ttu-id="291f3-144">Ett nytt frågefönster öppnas.</span><span class="sxs-lookup"><span data-stu-id="291f3-144">A new query window opens.</span></span>
   
    ![Ny fråga][5]
3. <span data-ttu-id="291f3-146">Kopiera den här TSQL-frågan till frågefönstret hello:</span><span class="sxs-lookup"><span data-stu-id="291f3-146">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="291f3-147">Kör hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="291f3-147">Run hello query.</span></span> <span data-ttu-id="291f3-148">toodo, klicka på hello gröna pilen eller använder följande genväg hello: `CTRL` + `SHIFT` + `E`.</span><span class="sxs-lookup"><span data-stu-id="291f3-148">toodo this, click hello green arrow or use hello following shortcut: `CTRL`+`SHIFT`+`E`.</span></span>
   
    ![Kör frågan][6]
5. <span data-ttu-id="291f3-150">Titta på hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="291f3-150">Look at hello query results.</span></span> <span data-ttu-id="291f3-151">I det här exemplet har hello tabellen FactInternetSales 60398 rader.</span><span class="sxs-lookup"><span data-stu-id="291f3-151">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Frågeresultat][7]

## <a name="next-steps"></a><span data-ttu-id="291f3-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="291f3-153">Next steps</span></span>
<span data-ttu-id="291f3-154">Nu när du kan ansluta och fråga, försök [visualisera hello data med PowerBI][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="291f3-154">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="291f3-155">tooconfigure din miljö för Azure Active Directory-autentisering, se [autentisera tooSQL datalagret][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="291f3-155">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

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
