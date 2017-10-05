---
title: "Kom igång med flera databaser frågor (vertikal partitionering) | Microsoft Docs"
description: "hur du använder elastisk databasfrågan med lodrätt partitionerade databaser"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 17158c4960e9ba9251524659c90af9aec1316774
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a><span data-ttu-id="56fff-103">Kom igång med flera databaser frågor (vertikal partitionering) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="56fff-103">Get started with cross-database queries (vertical partitioning) (preview)</span></span>
<span data-ttu-id="56fff-104">Elastisk databasfrågan (förhandsversion) för Azure SQL Database låter dig köra T-SQL-frågor som sträcker sig över flera databaser med hjälp av en enda anslutningspunkt.</span><span class="sxs-lookup"><span data-stu-id="56fff-104">Elastic database query (preview) for Azure SQL Database allows you to run T-SQL queries that span multiple databases using a single connection point.</span></span> <span data-ttu-id="56fff-105">Det här avsnittet gäller [lodrätt partitionerad databaser](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="56fff-105">This topic applies to [vertically partitioned databases](sql-database-elastic-query-vertical-partitioning.md).</span></span>  

<span data-ttu-id="56fff-106">När du är klar, kommer: Lär dig att konfigurera och köra frågor som sträcker sig över flera relaterade databaser med en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56fff-106">When completed, you will: learn how to configure and use an Azure SQL Database to perform queries that span multiple related databases.</span></span> 

<span data-ttu-id="56fff-107">Mer information om funktionen för elastisk databas frågan finns [översikt över Azure SQL Database elastisk databas frågan](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="56fff-107">For more information about the elastic database query feature, please see  [Azure SQL Database elastic database query overview](sql-database-elastic-query-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="56fff-108">Krav</span><span class="sxs-lookup"><span data-stu-id="56fff-108">Prerequisites</span></span>

<span data-ttu-id="56fff-109">Du måste ha behörigheten ALTER ANY extern DATAKÄLLA.</span><span class="sxs-lookup"><span data-stu-id="56fff-109">You must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="56fff-110">Den här behörigheten har behörigheten ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="56fff-110">This permission is included with the ALTER DATABASE permission.</span></span> <span data-ttu-id="56fff-111">ALTER ANY extern DATAKÄLLA behörighet att referera till den underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="56fff-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed to refer to the underlying data source.</span></span>

## <a name="create-the-sample-databases"></a><span data-ttu-id="56fff-112">Skapa exempeldatabasen</span><span class="sxs-lookup"><span data-stu-id="56fff-112">Create the sample databases</span></span>
<span data-ttu-id="56fff-113">Börja med, måste vi skapa två databaserna, **kunder** och **order**, antingen i samma eller andra logiska servrar.</span><span class="sxs-lookup"><span data-stu-id="56fff-113">To start with, we need to create two databases, **Customers** and **Orders**, either in the same or different logical servers.</span></span>   

<span data-ttu-id="56fff-114">Kör följande frågor på den **order** databas för att skapa den **OrderInformation** tabell och ange exempeldata.</span><span class="sxs-lookup"><span data-stu-id="56fff-114">Execute the following queries on the **Orders** database to create the **OrderInformation** table and input the sample data.</span></span> 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

<span data-ttu-id="56fff-115">Nu kan köra följande fråga på den **kunder** databas för att skapa den **CustomerInformation** tabell och ange exempeldata.</span><span class="sxs-lookup"><span data-stu-id="56fff-115">Now, execute following query on the **Customers** database to create the **CustomerInformation** table and input the sample data.</span></span> 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a><span data-ttu-id="56fff-116">Skapa databasobjekt</span><span class="sxs-lookup"><span data-stu-id="56fff-116">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="56fff-117">Databasbegränsade huvudnyckel och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="56fff-117">Database scoped master key and credentials</span></span>
1. <span data-ttu-id="56fff-118">Öppna SQL Server Management Studio eller SQL Server Data Tools i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56fff-118">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="56fff-119">Ansluta till order-databasen och kör följande T-SQL-kommandon:</span><span class="sxs-lookup"><span data-stu-id="56fff-119">Connect to the Orders database and execute the following T-SQL commands:</span></span>
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    <span data-ttu-id="56fff-120">”Användarnamn” och ”password” ska vara användarnamn och lösenord som används för att logga in i databasen kunder.</span><span class="sxs-lookup"><span data-stu-id="56fff-120">The "username" and "password" should be the username and password used to login into the Customers database.</span></span>
    <span data-ttu-id="56fff-121">Autentisering med hjälp av Azure Active Directory med elastisk frågor stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="56fff-121">Authentication using Azure Active Directory with elastic queries is not currently supported.</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="56fff-122">Externa datakällor</span><span class="sxs-lookup"><span data-stu-id="56fff-122">External data sources</span></span>
<span data-ttu-id="56fff-123">Om du vill skapa en extern datakälla, kör du följande kommando i order-databasen:</span><span class="sxs-lookup"><span data-stu-id="56fff-123">To create an external data source, execute the following command on the Orders database:</span></span> 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a><span data-ttu-id="56fff-124">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="56fff-124">External tables</span></span>
<span data-ttu-id="56fff-125">Skapa en extern tabell på order-databasen, som matchar definitionen av tabellen CustomerInformation:</span><span class="sxs-lookup"><span data-stu-id="56fff-125">Create an external table on the Orders database, which matches the definition of the CustomerInformation table:</span></span>

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="56fff-126">Köra en exempelfråga för elastisk databas T-SQL</span><span class="sxs-lookup"><span data-stu-id="56fff-126">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="56fff-127">När du har definierat den externa datakällan och externa tabeller kan du nu använda T-SQL fråga din externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="56fff-127">Once you have defined your external data source and your external tables you can now use T-SQL to query your external tables.</span></span> <span data-ttu-id="56fff-128">Kör frågan på order-databasen:</span><span class="sxs-lookup"><span data-stu-id="56fff-128">Execute this query on the Orders database:</span></span> 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a><span data-ttu-id="56fff-129">Kostnad</span><span class="sxs-lookup"><span data-stu-id="56fff-129">Cost</span></span>
<span data-ttu-id="56fff-130">Funktionen elastisk databas frågan är för närvarande inkluderas i kostnaden för Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="56fff-130">Currently, the elastic database query feature is included into the cost of your Azure SQL Database.</span></span>  

<span data-ttu-id="56fff-131">Mer information om priser finns [priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database).</span><span class="sxs-lookup"><span data-stu-id="56fff-131">For pricing information see [SQL Database Pricing](https://azure.microsoft.com/pricing/details/sql-database).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="56fff-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56fff-132">Next steps</span></span>

* <span data-ttu-id="56fff-133">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="56fff-133">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="56fff-134">Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="56fff-134">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="56fff-135">Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="56fff-135">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="56fff-136">Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="56fff-136">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="56fff-137">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="56fff-137">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>