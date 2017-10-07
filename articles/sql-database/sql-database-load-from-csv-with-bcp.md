---
title: "aaaLoad data från CSV-filen till Azure SQL Database (bcp) | Microsoft Docs"
description: "För små datastorlekar, använder du bcp tooimport data till Azure SQL Database."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a><span data-ttu-id="79e13-103">Läsa in data från CSV till Azure SQL Database (flat-filer)</span><span class="sxs-lookup"><span data-stu-id="79e13-103">Load data from CSV into Azure SQL Database (flat files)</span></span>
<span data-ttu-id="79e13-104">Du kan använda hello bcp kommandoradsverktyget tooimport data från en CSV-fil till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="79e13-104">You can use hello bcp command-line utility tooimport data from a CSV file into Azure SQL Database.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="79e13-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="79e13-105">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="79e13-106">Krav</span><span class="sxs-lookup"><span data-stu-id="79e13-106">Prerequisites</span></span>
<span data-ttu-id="79e13-107">toocomplete hello stegen i den här artikeln, måste du:</span><span class="sxs-lookup"><span data-stu-id="79e13-107">toocomplete hello steps in this article, you need:</span></span>

* <span data-ttu-id="79e13-108">Skapa en logisk Azure SQL Database-server och -databas</span><span class="sxs-lookup"><span data-stu-id="79e13-108">An Azure SQL Database logical server and database</span></span>
* <span data-ttu-id="79e13-109">hello kommandoradsverktyget BCP installerat</span><span class="sxs-lookup"><span data-stu-id="79e13-109">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="79e13-110">hello kommandoradsverktyget sqlcmd installerat</span><span class="sxs-lookup"><span data-stu-id="79e13-110">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="79e13-111">Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="79e13-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="79e13-112">Data i ASCII- eller UTF-16-format</span><span class="sxs-lookup"><span data-stu-id="79e13-112">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="79e13-113">Om du provar den här självstudiekursen med dina egna data behöver data toouse hello ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8.</span><span class="sxs-lookup"><span data-stu-id="79e13-113">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="79e13-114">1. Skapa en måltabell</span><span class="sxs-lookup"><span data-stu-id="79e13-114">1. Create a destination table</span></span>
<span data-ttu-id="79e13-115">Definiera en tabell i SQL-databas som hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="79e13-115">Define a table in SQL Database as hello destination table.</span></span> <span data-ttu-id="79e13-116">hello kolumner i tabellen hello måste överensstämma med toohello data i varje rad i datafilen.</span><span class="sxs-lookup"><span data-stu-id="79e13-116">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="79e13-117">toocreate en tabell, öppna en kommandotolk och använder sqlcmd.exe toorun hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="79e13-117">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="79e13-118">2. Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="79e13-118">2. Create a source data file</span></span>
<span data-ttu-id="79e13-119">Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="79e13-119">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="79e13-120">Den här datan är i ASCII-format.</span><span class="sxs-lookup"><span data-stu-id="79e13-120">This data is in ASCII format.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

<span data-ttu-id="79e13-121">(Valfritt) tooexport dina egna data från en SQL Server-databas, öppna en kommandotolk och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="79e13-121">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="79e13-122">Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.</span><span class="sxs-lookup"><span data-stu-id="79e13-122">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a><span data-ttu-id="79e13-123">3. Läs in hello data</span><span class="sxs-lookup"><span data-stu-id="79e13-123">3. Load hello data</span></span>
<span data-ttu-id="79e13-124">tooload hello data, öppna en kommandotolk och kör hello följande kommando, ersätter hello värdena för servernamn, databasen namn, användarnamn och lösenord med din egen information.</span><span class="sxs-lookup"><span data-stu-id="79e13-124">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

<span data-ttu-id="79e13-125">Använd det här kommandot tooverify hello data har lästs in korrekt</span><span class="sxs-lookup"><span data-stu-id="79e13-125">Use this command tooverify hello data was loaded properly</span></span>

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="79e13-126">hello resultat bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="79e13-126">hello results should look like this:</span></span>

| <span data-ttu-id="79e13-127">DateId</span><span class="sxs-lookup"><span data-stu-id="79e13-127">DateId</span></span> | <span data-ttu-id="79e13-128">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="79e13-128">CalendarQuarter</span></span> | <span data-ttu-id="79e13-129">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="79e13-129">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="79e13-130">20150101</span><span class="sxs-lookup"><span data-stu-id="79e13-130">20150101</span></span> |<span data-ttu-id="79e13-131">1</span><span class="sxs-lookup"><span data-stu-id="79e13-131">1</span></span> |<span data-ttu-id="79e13-132">3</span><span class="sxs-lookup"><span data-stu-id="79e13-132">3</span></span> |
| <span data-ttu-id="79e13-133">20150201</span><span class="sxs-lookup"><span data-stu-id="79e13-133">20150201</span></span> |<span data-ttu-id="79e13-134">1</span><span class="sxs-lookup"><span data-stu-id="79e13-134">1</span></span> |<span data-ttu-id="79e13-135">3</span><span class="sxs-lookup"><span data-stu-id="79e13-135">3</span></span> |
| <span data-ttu-id="79e13-136">20150301</span><span class="sxs-lookup"><span data-stu-id="79e13-136">20150301</span></span> |<span data-ttu-id="79e13-137">1</span><span class="sxs-lookup"><span data-stu-id="79e13-137">1</span></span> |<span data-ttu-id="79e13-138">3</span><span class="sxs-lookup"><span data-stu-id="79e13-138">3</span></span> |
| <span data-ttu-id="79e13-139">20150401</span><span class="sxs-lookup"><span data-stu-id="79e13-139">20150401</span></span> |<span data-ttu-id="79e13-140">2</span><span class="sxs-lookup"><span data-stu-id="79e13-140">2</span></span> |<span data-ttu-id="79e13-141">4</span><span class="sxs-lookup"><span data-stu-id="79e13-141">4</span></span> |
| <span data-ttu-id="79e13-142">20150501</span><span class="sxs-lookup"><span data-stu-id="79e13-142">20150501</span></span> |<span data-ttu-id="79e13-143">2</span><span class="sxs-lookup"><span data-stu-id="79e13-143">2</span></span> |<span data-ttu-id="79e13-144">4</span><span class="sxs-lookup"><span data-stu-id="79e13-144">4</span></span> |
| <span data-ttu-id="79e13-145">20150601</span><span class="sxs-lookup"><span data-stu-id="79e13-145">20150601</span></span> |<span data-ttu-id="79e13-146">2</span><span class="sxs-lookup"><span data-stu-id="79e13-146">2</span></span> |<span data-ttu-id="79e13-147">4</span><span class="sxs-lookup"><span data-stu-id="79e13-147">4</span></span> |
| <span data-ttu-id="79e13-148">20150701</span><span class="sxs-lookup"><span data-stu-id="79e13-148">20150701</span></span> |<span data-ttu-id="79e13-149">3</span><span class="sxs-lookup"><span data-stu-id="79e13-149">3</span></span> |<span data-ttu-id="79e13-150">1</span><span class="sxs-lookup"><span data-stu-id="79e13-150">1</span></span> |
| <span data-ttu-id="79e13-151">20150801</span><span class="sxs-lookup"><span data-stu-id="79e13-151">20150801</span></span> |<span data-ttu-id="79e13-152">3</span><span class="sxs-lookup"><span data-stu-id="79e13-152">3</span></span> |<span data-ttu-id="79e13-153">1</span><span class="sxs-lookup"><span data-stu-id="79e13-153">1</span></span> |
| <span data-ttu-id="79e13-154">20150801</span><span class="sxs-lookup"><span data-stu-id="79e13-154">20150801</span></span> |<span data-ttu-id="79e13-155">3</span><span class="sxs-lookup"><span data-stu-id="79e13-155">3</span></span> |<span data-ttu-id="79e13-156">1</span><span class="sxs-lookup"><span data-stu-id="79e13-156">1</span></span> |
| <span data-ttu-id="79e13-157">20151001</span><span class="sxs-lookup"><span data-stu-id="79e13-157">20151001</span></span> |<span data-ttu-id="79e13-158">4</span><span class="sxs-lookup"><span data-stu-id="79e13-158">4</span></span> |<span data-ttu-id="79e13-159">2</span><span class="sxs-lookup"><span data-stu-id="79e13-159">2</span></span> |
| <span data-ttu-id="79e13-160">20151101</span><span class="sxs-lookup"><span data-stu-id="79e13-160">20151101</span></span> |<span data-ttu-id="79e13-161">4</span><span class="sxs-lookup"><span data-stu-id="79e13-161">4</span></span> |<span data-ttu-id="79e13-162">2</span><span class="sxs-lookup"><span data-stu-id="79e13-162">2</span></span> |
| <span data-ttu-id="79e13-163">20151201</span><span class="sxs-lookup"><span data-stu-id="79e13-163">20151201</span></span> |<span data-ttu-id="79e13-164">4</span><span class="sxs-lookup"><span data-stu-id="79e13-164">4</span></span> |<span data-ttu-id="79e13-165">2</span><span class="sxs-lookup"><span data-stu-id="79e13-165">2</span></span> |

## <a name="next-steps"></a><span data-ttu-id="79e13-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79e13-166">Next steps</span></span>
<span data-ttu-id="79e13-167">toomigrate en SQL Server-databas finns [SQL Server-Databasmigrering](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="79e13-167">toomigrate a SQL Server database, see [SQL Server database migration](sql-database-cloud-migrate.md).</span></span>

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
