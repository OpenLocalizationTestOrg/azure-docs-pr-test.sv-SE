---
title: "Läs in data från CSV-fil till Azure SQL Database (bcp) | Microsoft Docs"
description: "För mindre datastorlekar används bcp för att importera data till Azure SQL Database."
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
ms.openlocfilehash: 84bebab7763bb21f73880a6c8b367a62b0c137d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a><span data-ttu-id="793c1-103">Läsa in data från CSV till Azure SQL Database (flat-filer)</span><span class="sxs-lookup"><span data-stu-id="793c1-103">Load data from CSV into Azure SQL Database (flat files)</span></span>
<span data-ttu-id="793c1-104">Du kan använda kommandoradsverktyget bcp för att importera data från en CSV-fil till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="793c1-104">You can use the bcp command-line utility to import data from a CSV file into Azure SQL Database.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="793c1-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="793c1-105">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="793c1-106">Krav</span><span class="sxs-lookup"><span data-stu-id="793c1-106">Prerequisites</span></span>
<span data-ttu-id="793c1-107">Du behöver följande för att slutföra stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="793c1-107">To complete the steps in this article, you need:</span></span>

* <span data-ttu-id="793c1-108">Skapa en logisk Azure SQL Database-server och -databas</span><span class="sxs-lookup"><span data-stu-id="793c1-108">An Azure SQL Database logical server and database</span></span>
* <span data-ttu-id="793c1-109">Kommandoradsverktyget bcp installerat</span><span class="sxs-lookup"><span data-stu-id="793c1-109">The bcp command-line utility installed</span></span>
* <span data-ttu-id="793c1-110">Kommandoradsverktyget sqlcmd installerat</span><span class="sxs-lookup"><span data-stu-id="793c1-110">The sqlcmd command-line utility installed</span></span>

<span data-ttu-id="793c1-111">Du kan hämta verktygen bcp och sqlcmd från [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="793c1-111">You can download the bcp and sqlcmd utilities from the [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="793c1-112">Data i ASCII- eller UTF-16-format</span><span class="sxs-lookup"><span data-stu-id="793c1-112">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="793c1-113">Om du använder egna data i självstudierna, måste de använda sig av ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8.</span><span class="sxs-lookup"><span data-stu-id="793c1-113">If you are trying this tutorial with your own data, your data needs to use the ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="793c1-114">1. Skapa en måltabell</span><span class="sxs-lookup"><span data-stu-id="793c1-114">1. Create a destination table</span></span>
<span data-ttu-id="793c1-115">Definiera en tabell i SQL Database som måltabellen.</span><span class="sxs-lookup"><span data-stu-id="793c1-115">Define a table in SQL Database as the destination table.</span></span> <span data-ttu-id="793c1-116">Kolumnerna i tabellen måste motsvara data i varje rad i din datafil.</span><span class="sxs-lookup"><span data-stu-id="793c1-116">The columns in the table must correspond to the data in each row of your data file.</span></span>

<span data-ttu-id="793c1-117">För att skapa en tabell, öppnar du en kommandotolk och använder sqlcmd.exe för att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="793c1-117">To create a table, open a command prompt and use sqlcmd.exe to run the following command:</span></span>

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


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="793c1-118">2. Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="793c1-118">2. Create a source data file</span></span>
<span data-ttu-id="793c1-119">Öppna Anteckningar och kopiera följande datarader till en ny textfil. Spara sedan filen till din lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="793c1-119">Open Notepad and copy the following lines of data into a new text file and then save this file to your local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="793c1-120">Den här datan är i ASCII-format.</span><span class="sxs-lookup"><span data-stu-id="793c1-120">This data is in ASCII format.</span></span>

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

<span data-ttu-id="793c1-121">(Valfritt) Om du vill exportera dina egna data från en SQL Server-databas, öppnar du en kommandotolk och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="793c1-121">(Optional) To export your own data from a SQL Server database, open a command prompt and run the following command.</span></span> <span data-ttu-id="793c1-122">Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.</span><span class="sxs-lookup"><span data-stu-id="793c1-122">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-the-data"></a><span data-ttu-id="793c1-123">3. Läs in data</span><span class="sxs-lookup"><span data-stu-id="793c1-123">3. Load the data</span></span>
<span data-ttu-id="793c1-124">För att läsa in data, öppnar du en kommandotolk och kör följande kommando, där du ersätter värdena för servernamn, databasnamn, användarnamn och lösenord med din egen information.</span><span class="sxs-lookup"><span data-stu-id="793c1-124">To load the data, open a command prompt and run the following command, replacing the values for Server Name, Database name, Username, and Password with your own information.</span></span>

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

<span data-ttu-id="793c1-125">Använd det här kommandot för att verifiera att data har lästs in korrekt</span><span class="sxs-lookup"><span data-stu-id="793c1-125">Use this command to verify the data was loaded properly</span></span>

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="793c1-126">Resultatet borde se ut så här:</span><span class="sxs-lookup"><span data-stu-id="793c1-126">The results should look like this:</span></span>

| <span data-ttu-id="793c1-127">DateId</span><span class="sxs-lookup"><span data-stu-id="793c1-127">DateId</span></span> | <span data-ttu-id="793c1-128">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="793c1-128">CalendarQuarter</span></span> | <span data-ttu-id="793c1-129">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="793c1-129">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="793c1-130">20150101</span><span class="sxs-lookup"><span data-stu-id="793c1-130">20150101</span></span> |<span data-ttu-id="793c1-131">1</span><span class="sxs-lookup"><span data-stu-id="793c1-131">1</span></span> |<span data-ttu-id="793c1-132">3</span><span class="sxs-lookup"><span data-stu-id="793c1-132">3</span></span> |
| <span data-ttu-id="793c1-133">20150201</span><span class="sxs-lookup"><span data-stu-id="793c1-133">20150201</span></span> |<span data-ttu-id="793c1-134">1</span><span class="sxs-lookup"><span data-stu-id="793c1-134">1</span></span> |<span data-ttu-id="793c1-135">3</span><span class="sxs-lookup"><span data-stu-id="793c1-135">3</span></span> |
| <span data-ttu-id="793c1-136">20150301</span><span class="sxs-lookup"><span data-stu-id="793c1-136">20150301</span></span> |<span data-ttu-id="793c1-137">1</span><span class="sxs-lookup"><span data-stu-id="793c1-137">1</span></span> |<span data-ttu-id="793c1-138">3</span><span class="sxs-lookup"><span data-stu-id="793c1-138">3</span></span> |
| <span data-ttu-id="793c1-139">20150401</span><span class="sxs-lookup"><span data-stu-id="793c1-139">20150401</span></span> |<span data-ttu-id="793c1-140">2</span><span class="sxs-lookup"><span data-stu-id="793c1-140">2</span></span> |<span data-ttu-id="793c1-141">4</span><span class="sxs-lookup"><span data-stu-id="793c1-141">4</span></span> |
| <span data-ttu-id="793c1-142">20150501</span><span class="sxs-lookup"><span data-stu-id="793c1-142">20150501</span></span> |<span data-ttu-id="793c1-143">2</span><span class="sxs-lookup"><span data-stu-id="793c1-143">2</span></span> |<span data-ttu-id="793c1-144">4</span><span class="sxs-lookup"><span data-stu-id="793c1-144">4</span></span> |
| <span data-ttu-id="793c1-145">20150601</span><span class="sxs-lookup"><span data-stu-id="793c1-145">20150601</span></span> |<span data-ttu-id="793c1-146">2</span><span class="sxs-lookup"><span data-stu-id="793c1-146">2</span></span> |<span data-ttu-id="793c1-147">4</span><span class="sxs-lookup"><span data-stu-id="793c1-147">4</span></span> |
| <span data-ttu-id="793c1-148">20150701</span><span class="sxs-lookup"><span data-stu-id="793c1-148">20150701</span></span> |<span data-ttu-id="793c1-149">3</span><span class="sxs-lookup"><span data-stu-id="793c1-149">3</span></span> |<span data-ttu-id="793c1-150">1</span><span class="sxs-lookup"><span data-stu-id="793c1-150">1</span></span> |
| <span data-ttu-id="793c1-151">20150801</span><span class="sxs-lookup"><span data-stu-id="793c1-151">20150801</span></span> |<span data-ttu-id="793c1-152">3</span><span class="sxs-lookup"><span data-stu-id="793c1-152">3</span></span> |<span data-ttu-id="793c1-153">1</span><span class="sxs-lookup"><span data-stu-id="793c1-153">1</span></span> |
| <span data-ttu-id="793c1-154">20150801</span><span class="sxs-lookup"><span data-stu-id="793c1-154">20150801</span></span> |<span data-ttu-id="793c1-155">3</span><span class="sxs-lookup"><span data-stu-id="793c1-155">3</span></span> |<span data-ttu-id="793c1-156">1</span><span class="sxs-lookup"><span data-stu-id="793c1-156">1</span></span> |
| <span data-ttu-id="793c1-157">20151001</span><span class="sxs-lookup"><span data-stu-id="793c1-157">20151001</span></span> |<span data-ttu-id="793c1-158">4</span><span class="sxs-lookup"><span data-stu-id="793c1-158">4</span></span> |<span data-ttu-id="793c1-159">2</span><span class="sxs-lookup"><span data-stu-id="793c1-159">2</span></span> |
| <span data-ttu-id="793c1-160">20151101</span><span class="sxs-lookup"><span data-stu-id="793c1-160">20151101</span></span> |<span data-ttu-id="793c1-161">4</span><span class="sxs-lookup"><span data-stu-id="793c1-161">4</span></span> |<span data-ttu-id="793c1-162">2</span><span class="sxs-lookup"><span data-stu-id="793c1-162">2</span></span> |
| <span data-ttu-id="793c1-163">20151201</span><span class="sxs-lookup"><span data-stu-id="793c1-163">20151201</span></span> |<span data-ttu-id="793c1-164">4</span><span class="sxs-lookup"><span data-stu-id="793c1-164">4</span></span> |<span data-ttu-id="793c1-165">2</span><span class="sxs-lookup"><span data-stu-id="793c1-165">2</span></span> |

## <a name="next-steps"></a><span data-ttu-id="793c1-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="793c1-166">Next steps</span></span>
<span data-ttu-id="793c1-167">Om du vill migrera en SQL Server-databas hittar du mer information i [SQL Server-databasmigrering](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="793c1-167">To migrate a SQL Server database, see [SQL Server database migration](sql-database-cloud-migrate.md).</span></span>

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
