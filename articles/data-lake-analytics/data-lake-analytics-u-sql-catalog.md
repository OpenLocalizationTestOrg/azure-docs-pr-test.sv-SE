---
title: "Kom igång med hello U-SQL-katalogen | Microsoft Docs"
description: "Lär dig hur toouse hello U-SQL katalogen tooshare kod och data."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: edmaca
ms.openlocfilehash: 559bb7a3879031eb290a3e82946d7bf42ac9f553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-u-sql-catalog"></a><span data-ttu-id="b0e6d-103">Kom igång med hello U-SQL-katalogen</span><span class="sxs-lookup"><span data-stu-id="b0e6d-103">Get started with hello U-SQL Catalog</span></span>

## <a name="create-a-tvf"></a><span data-ttu-id="b0e6d-104">Skapa en Tabellvärdesfunktion</span><span class="sxs-lookup"><span data-stu-id="b0e6d-104">Create a TVF</span></span>

<span data-ttu-id="b0e6d-105">I hello tidigare U-SQL-skript du upprepas hello användning av EXTRAHERA tooread från hello samma källfil.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-105">In hello previous U-SQL script, you repeated hello use of EXTRACT tooread from hello same source file.</span></span> <span data-ttu-id="b0e6d-106">Du kan använda hello U-SQL-Tabellvärdesfunktionen (Tabellvärdesfunktion), för att kapsla in hello data för senare användning.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-106">With hello U-SQL table-valued function (TVF), you can encapsulate hello data for future reuse.</span></span>  

<span data-ttu-id="b0e6d-107">hello följande skript skapar en Tabellvärdesfunktion kallas `Searchlog()` i hello standarddatabasen och -schemat:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-107">hello following script creates a TVF called `Searchlog()` in hello default database and schema:</span></span>

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

<span data-ttu-id="b0e6d-108">hello följande skript visar hur toouse hello Tabellvärdesfunktion som har definierats i föregående hello-skriptet:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-108">hello following script shows you how toouse hello TVF that was defined in hello previous script:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a><span data-ttu-id="b0e6d-109">Skapa vyer</span><span class="sxs-lookup"><span data-stu-id="b0e6d-109">Create views</span></span>

<span data-ttu-id="b0e6d-110">Om du har ett enda frågeuttryck i stället för en Tabellvärdesfunktion kan du använda ett U-SQL-VYN tooencapsulate uttrycket.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-110">If you have a single query expression, instead of a TVF you can use a U-SQL VIEW tooencapsulate that expression.</span></span>

<span data-ttu-id="b0e6d-111">hello följande skript skapar en vy som heter `SearchlogView` i hello standarddatabasen och -schemat:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-111">hello following script creates a view called `SearchlogView` in hello default database and schema:</span></span>

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

<span data-ttu-id="b0e6d-112">hello följande skript visar hello använder hello definierad vy:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-112">hello following script demonstrates hello use of hello defined view:</span></span>

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a><span data-ttu-id="b0e6d-113">Skapa tabeller</span><span class="sxs-lookup"><span data-stu-id="b0e6d-113">Create tables</span></span>
<span data-ttu-id="b0e6d-114">Som med relationsdatabas tabeller med U-SQL kan du skapa en tabell med ett fördefinierat schema eller skapa en tabell som skapar hello schema från hello-fråga som fyller på hello tabellen (även kallat CREATE TABLE AS SELECT eller CTAS).</span><span class="sxs-lookup"><span data-stu-id="b0e6d-114">As with relational database tables, with U-SQL you can create a table with a predefined schema or create a table that infers hello schema from hello query that populates hello table (also known as CREATE TABLE AS SELECT or CTAS).</span></span>

<span data-ttu-id="b0e6d-115">Skapa en databas och två tabeller med hjälp av hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-115">Create a database and two tables by using hello following script:</span></span>

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a><span data-ttu-id="b0e6d-116">Frågetabeller</span><span class="sxs-lookup"><span data-stu-id="b0e6d-116">Query tables</span></span>
<span data-ttu-id="b0e6d-117">Du kan ställa frågor till tabeller, till exempel de som skapats i hello föregående skript i hello samma sätt som du frågar hello datafiler.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-117">You can query tables, such as those created in hello previous script, in hello same way that you query hello data files.</span></span> <span data-ttu-id="b0e6d-118">I stället för att skapa en raduppsättning med EXTRAHERA kan nu du referera toohello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-118">Instead of creating a rowset by using EXTRACT, you now can refer toohello table name.</span></span>

<span data-ttu-id="b0e6d-119">tooread från hello tabeller, ändra hello transformeringen skript som du använde tidigare:</span><span class="sxs-lookup"><span data-stu-id="b0e6d-119">tooread from hello tables, modify hello transform script that you used previously:</span></span>

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    too"/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 ><span data-ttu-id="b0e6d-120">Du kan inte för närvarande köra en väljer på en tabell i samma skript som hello en hello där du skapade hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b0e6d-120">Currently, you cannot run a SELECT on a table in hello same script as hello one where you created hello table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0e6d-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0e6d-121">Next Steps</span></span>
* [<span data-ttu-id="b0e6d-122">Översikt över Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b0e6d-122">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="b0e6d-123">Utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0e6d-123">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="b0e6d-124">Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b0e6d-124">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
