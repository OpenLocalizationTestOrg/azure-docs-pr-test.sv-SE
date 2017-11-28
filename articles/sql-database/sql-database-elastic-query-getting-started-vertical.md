---
title: "aaaGet igång med flera databaser frågor (vertikal partitionering) | Microsoft Docs"
description: "hur toouse elastisk databasfrågan med lodrätt partitionerad databaser"
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
ms.openlocfilehash: 9e6183268e8bf87e3ac28f502711fcc05a7a3f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a><span data-ttu-id="04241-103">Kom igång med flera databaser frågor (vertikal partitionering) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="04241-103">Get started with cross-database queries (vertical partitioning) (preview)</span></span>
<span data-ttu-id="04241-104">Elastisk databasfrågan (förhandsversion) för Azure SQL Database kan toorun T-SQL-frågor som sträcker sig över flera databaser med hjälp av en enda anslutningspunkt.</span><span class="sxs-lookup"><span data-stu-id="04241-104">Elastic database query (preview) for Azure SQL Database allows you toorun T-SQL queries that span multiple databases using a single connection point.</span></span> <span data-ttu-id="04241-105">Det här avsnittet gäller för[lodrätt partitionerad databaser](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="04241-105">This topic applies too[vertically partitioned databases](sql-database-elastic-query-vertical-partitioning.md).</span></span>  

<span data-ttu-id="04241-106">När du är klar, kommer: Lär dig hur tooconfigure och använda en Azure SQL Database-tooperform frågar den span flera relaterade databaser.</span><span class="sxs-lookup"><span data-stu-id="04241-106">When completed, you will: learn how tooconfigure and use an Azure SQL Database tooperform queries that span multiple related databases.</span></span> 

<span data-ttu-id="04241-107">Mer information om hello elastisk databas fråga funktionen finns [översikt över Azure SQL Database elastisk databas frågan](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="04241-107">For more information about hello elastic database query feature, please see  [Azure SQL Database elastic database query overview](sql-database-elastic-query-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="04241-108">Krav</span><span class="sxs-lookup"><span data-stu-id="04241-108">Prerequisites</span></span>

<span data-ttu-id="04241-109">Du måste ha behörigheten ALTER ANY extern DATAKÄLLA.</span><span class="sxs-lookup"><span data-stu-id="04241-109">You must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="04241-110">Den här behörigheten ingår i hello ALTER DATABASE-behörighet.</span><span class="sxs-lookup"><span data-stu-id="04241-110">This permission is included with hello ALTER DATABASE permission.</span></span> <span data-ttu-id="04241-111">ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.</span><span class="sxs-lookup"><span data-stu-id="04241-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="create-hello-sample-databases"></a><span data-ttu-id="04241-112">Skapa hello exempeldatabas</span><span class="sxs-lookup"><span data-stu-id="04241-112">Create hello sample databases</span></span>
<span data-ttu-id="04241-113">toostart med vi behöver toocreate två databaserna, **kunder** och **order**, antingen i hello samma eller andra logiska servrar.</span><span class="sxs-lookup"><span data-stu-id="04241-113">toostart with, we need toocreate two databases, **Customers** and **Orders**, either in hello same or different logical servers.</span></span>   

<span data-ttu-id="04241-114">Kör följande frågor på hello hello **order** databasen toocreate hello **OrderInformation** tabell och indata hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="04241-114">Execute hello following queries on hello **Orders** database toocreate hello **OrderInformation** table and input hello sample data.</span></span> 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

<span data-ttu-id="04241-115">Nu kan köra följande fråga på hello **kunder** databasen toocreate hello **CustomerInformation** tabell och indata hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="04241-115">Now, execute following query on hello **Customers** database toocreate hello **CustomerInformation** table and input hello sample data.</span></span> 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a><span data-ttu-id="04241-116">Skapa databasobjekt</span><span class="sxs-lookup"><span data-stu-id="04241-116">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="04241-117">Databasbegränsade huvudnyckel och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="04241-117">Database scoped master key and credentials</span></span>
1. <span data-ttu-id="04241-118">Öppna SQL Server Management Studio eller SQL Server Data Tools i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04241-118">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="04241-119">Ansluta toohello order databas och kör följande T-SQL-kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="04241-119">Connect toohello Orders database and execute hello following T-SQL commands:</span></span>
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    <span data-ttu-id="04241-120">hello ”användarnamn” och ”password” måste vara hello användarnamnet och lösenordet som används för toologin till hello kunder databas.</span><span class="sxs-lookup"><span data-stu-id="04241-120">hello "username" and "password" should be hello username and password used toologin into hello Customers database.</span></span>
    <span data-ttu-id="04241-121">Autentisering med hjälp av Azure Active Directory med elastisk frågor stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="04241-121">Authentication using Azure Active Directory with elastic queries is not currently supported.</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="04241-122">Externa datakällor</span><span class="sxs-lookup"><span data-stu-id="04241-122">External data sources</span></span>
<span data-ttu-id="04241-123">toocreate en extern datakälla, kör följande kommando på hello order databasen hello:</span><span class="sxs-lookup"><span data-stu-id="04241-123">toocreate an external data source, execute hello following command on hello Orders database:</span></span> 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a><span data-ttu-id="04241-124">Externa tabeller</span><span class="sxs-lookup"><span data-stu-id="04241-124">External tables</span></span>
<span data-ttu-id="04241-125">Skapa en extern tabell på hello order databasen som matchar hello definition av hello CustomerInformation tabell:</span><span class="sxs-lookup"><span data-stu-id="04241-125">Create an external table on hello Orders database, which matches hello definition of hello CustomerInformation table:</span></span>

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="04241-126">Köra en exempelfråga för elastisk databas T-SQL</span><span class="sxs-lookup"><span data-stu-id="04241-126">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="04241-127">När du har definierat den externa datakällan och externa tabeller kan du nu använda T-SQL tooquery externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="04241-127">Once you have defined your external data source and your external tables you can now use T-SQL tooquery your external tables.</span></span> <span data-ttu-id="04241-128">Kör frågan på hello order databasen:</span><span class="sxs-lookup"><span data-stu-id="04241-128">Execute this query on hello Orders database:</span></span> 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a><span data-ttu-id="04241-129">Kostnad</span><span class="sxs-lookup"><span data-stu-id="04241-129">Cost</span></span>
<span data-ttu-id="04241-130">För närvarande ingår hello elastisk databas frågan i hello kostnaden för din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="04241-130">Currently, hello elastic database query feature is included into hello cost of your Azure SQL Database.</span></span>  

<span data-ttu-id="04241-131">Mer information om priser finns [priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database).</span><span class="sxs-lookup"><span data-stu-id="04241-131">For pricing information see [SQL Database Pricing](https://azure.microsoft.com/pricing/details/sql-database).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="04241-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04241-132">Next steps</span></span>

* <span data-ttu-id="04241-133">En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="04241-133">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="04241-134">Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="04241-134">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="04241-135">Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="04241-135">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="04241-136">Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="04241-136">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="04241-137">Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.</span><span class="sxs-lookup"><span data-stu-id="04241-137">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>