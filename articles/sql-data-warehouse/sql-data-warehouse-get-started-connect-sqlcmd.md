---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd | Microsoft Docs
description: "Använd [sqlcmd] [sqlcmd] kommandoradsverktyget tooconnect tooand fråga en Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a><span data-ttu-id="6ea2c-103">Ansluta tooSQL Data Warehouse med sqlcmd</span><span class="sxs-lookup"><span data-stu-id="6ea2c-103">Connect tooSQL Data Warehouse with sqlcmd</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6ea2c-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="6ea2c-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="6ea2c-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6ea2c-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="6ea2c-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ea2c-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="6ea2c-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="6ea2c-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="6ea2c-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="6ea2c-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="6ea2c-109">Använd [sqlcmd] [ sqlcmd] kommandoradsverktyget tooconnect tooand fråga en Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-109">Use [sqlcmd][sqlcmd] command-line utility tooconnect tooand query an Azure SQL Data Warehouse.</span></span>  

## <a name="1-connect"></a><span data-ttu-id="6ea2c-110">1. Anslut</span><span class="sxs-lookup"><span data-stu-id="6ea2c-110">1. Connect</span></span>
<span data-ttu-id="6ea2c-111">tooget igång med [sqlcmd][sqlcmd], öppna hello-kommandotolk och ange **sqlcmd** följt av hello anslutningssträngen för SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-111">tooget started with [sqlcmd][sqlcmd], open hello command prompt and enter **sqlcmd** followed by hello connection string for your SQL Data Warehouse database.</span></span> <span data-ttu-id="6ea2c-112">hello anslutningssträngen kräver hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="6ea2c-112">hello connection string requires hello following parameters:</span></span>

* <span data-ttu-id="6ea2c-113">**Server (-S):** Server i form av hello `<`servernamn`>`. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="6ea2c-113">**Server (-S):** Server in hello form `<`Server Name`>`.database.windows.net</span></span>
* <span data-ttu-id="6ea2c-114">**Databas (-d):** Databasens namn.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-114">**Database (-d):** Database name.</span></span>
* <span data-ttu-id="6ea2c-115">**Aktivera identifierare inom citattecken (-I):** identifierare inom citattecken måste vara aktiverade tooconnect tooa SQL Data Warehouse-instans.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-115">**Enable Quoted Identifiers (-I):** Quoted identifiers must be enabled tooconnect tooa SQL Data Warehouse instance.</span></span>

<span data-ttu-id="6ea2c-116">toouse SQL Server-autentisering, behöver du tooadd hello användarnamn/lösenord parametrar:</span><span class="sxs-lookup"><span data-stu-id="6ea2c-116">toouse SQL Server Authentication, you need tooadd hello username/password parameters:</span></span>

* <span data-ttu-id="6ea2c-117">**Användare (-U):** serveranvändare i formatet hello `<`användare`>`</span><span class="sxs-lookup"><span data-stu-id="6ea2c-117">**User (-U):** Server user in hello form `<`User`>`</span></span>
* <span data-ttu-id="6ea2c-118">**Lösenord (-P):** lösenord som är associerat med hello användare.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-118">**Password (-P):** Password associated with hello user.</span></span>

<span data-ttu-id="6ea2c-119">Anslutningssträngen kan till exempel se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="6ea2c-119">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

<span data-ttu-id="6ea2c-120">toouse Azure Active Directory-integrerad autentisering, behöver du tooadd hello Azure Active Directory parametrar:</span><span class="sxs-lookup"><span data-stu-id="6ea2c-120">toouse Azure Active Directory Integrated authentication, you need tooadd hello Azure Active Directory parameters:</span></span>

* <span data-ttu-id="6ea2c-121">**Azure Active Directory-autentisering (-G):** använder Azure Active Directory för autentisering</span><span class="sxs-lookup"><span data-stu-id="6ea2c-121">**Azure Active Directory Authentication (-G):** use Azure Active Directory for authentication</span></span>

<span data-ttu-id="6ea2c-122">Anslutningssträngen kan till exempel se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="6ea2c-122">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> <span data-ttu-id="6ea2c-123">Du behöver för[aktivera Azure Active Directory Authentication](sql-data-warehouse-authentication.md) tooauthenticate med Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-123">You need too[enable Azure Active Directory Authentication](sql-data-warehouse-authentication.md) tooauthenticate using Active Directory.</span></span>
> 
> 

## <a name="2-query"></a><span data-ttu-id="6ea2c-124">2. Fråga</span><span class="sxs-lookup"><span data-stu-id="6ea2c-124">2. Query</span></span>
<span data-ttu-id="6ea2c-125">Du kan utfärda alla stöds Transact-SQL-instruktioner mot hello instansen efter anslutning.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-125">After connection, you can issue any supported Transact-SQL statements against hello instance.</span></span>  <span data-ttu-id="6ea2c-126">I det här exemplet skickas frågor i interaktivt läge.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-126">In this example, queries are submitted in interactive mode.</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

<span data-ttu-id="6ea2c-127">Dessa nästa exempel visar hur du kan köra dina frågor i batchläge hello -Q alternativet eller skicka SQL-toosqlcmd.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-127">These next examples show how you can run your queries in batch mode using hello -Q option or piping your SQL toosqlcmd.</span></span>

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a><span data-ttu-id="6ea2c-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ea2c-128">Next steps</span></span>
<span data-ttu-id="6ea2c-129">Se [sqlcmd-dokumentationen] [ sqlcmd] mer information om information om hello alternativen i sqlcmd.</span><span class="sxs-lookup"><span data-stu-id="6ea2c-129">See [sqlcmd documentation][sqlcmd] for more about details about hello options available in sqlcmd.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
