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
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Kom igång med flera databaser frågor (vertikal partitionering) (förhandsgranskning)
Elastisk databasfrågan (förhandsversion) för Azure SQL Database kan toorun T-SQL-frågor som sträcker sig över flera databaser med hjälp av en enda anslutningspunkt. Det här avsnittet gäller för[lodrätt partitionerad databaser](sql-database-elastic-query-vertical-partitioning.md).  

När du är klar, kommer: Lär dig hur tooconfigure och använda en Azure SQL Database-tooperform frågar den span flera relaterade databaser. 

Mer information om hello elastisk databas fråga funktionen finns [översikt över Azure SQL Database elastisk databas frågan](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Krav

Du måste ha behörigheten ALTER ANY extern DATAKÄLLA. Den här behörigheten ingår i hello ALTER DATABASE-behörighet. ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.

## <a name="create-hello-sample-databases"></a>Skapa hello exempeldatabas
toostart med vi behöver toocreate två databaserna, **kunder** och **order**, antingen i hello samma eller andra logiska servrar.   

Kör följande frågor på hello hello **order** databasen toocreate hello **OrderInformation** tabell och indata hello exempeldata. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nu kan köra följande fråga på hello **kunder** databasen toocreate hello **CustomerInformation** tabell och indata hello exempeldata. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Skapa databasobjekt
### <a name="database-scoped-master-key-and-credentials"></a>Databasbegränsade huvudnyckel och autentiseringsuppgifter
1. Öppna SQL Server Management Studio eller SQL Server Data Tools i Visual Studio.
2. Ansluta toohello order databas och kör följande T-SQL-kommandon hello:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    hello ”användarnamn” och ”password” måste vara hello användarnamnet och lösenordet som används för toologin till hello kunder databas.
    Autentisering med hjälp av Azure Active Directory med elastisk frågor stöds inte för närvarande.

### <a name="external-data-sources"></a>Externa datakällor
toocreate en extern datakälla, kör följande kommando på hello order databasen hello: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externa tabeller
Skapa en extern tabell på hello order databasen som matchar hello definition av hello CustomerInformation tabell:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Köra en exempelfråga för elastisk databas T-SQL
När du har definierat den externa datakällan och externa tabeller kan du nu använda T-SQL tooquery externa tabeller. Kör frågan på hello order databasen: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kostnad
För närvarande ingår hello elastisk databas frågan i hello kostnaden för din Azure SQL-databas.  

Mer information om priser finns [priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Nästa steg

* En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
* Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)
* Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).
* Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)
* Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.