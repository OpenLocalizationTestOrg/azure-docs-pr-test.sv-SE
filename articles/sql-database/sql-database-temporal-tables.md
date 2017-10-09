---
title: "aaaGetting igång med Temporala tabeller i Azure SQL Database | Microsoft Docs"
description: "Lär dig hur tooget igång med att använda Temporala tabeller i Azure SQL Database."
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: c8c0f232-0751-4a7f-a36e-67a0b29fa1b8
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 01/10/2017
ms.author: bonova
ms.openlocfilehash: 54f394b51df07aa2f9bb299f207e692171d23479
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a><span data-ttu-id="5d328-103">Komma igång med Temporala tabeller i Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5d328-103">Getting Started with Temporal Tables in Azure SQL Database</span></span>
<span data-ttu-id="5d328-104">Temporala tabeller är en ny funktion för programmering i Azure SQL Database som gör att du tootrack och analysera hello hela historiken för ändringar i dina data utan att hello behöver anpassade kodning.</span><span class="sxs-lookup"><span data-stu-id="5d328-104">Temporal Tables are a new programmability feature of Azure SQL Database that allows you tootrack and analyze hello full history of changes in your data, without hello need for custom coding.</span></span> <span data-ttu-id="5d328-105">Temporala tabeller behålla nära relaterade tootime datakontexten så att lagrade fakta kan tolkas som giltig endast inom hello specifik period.</span><span class="sxs-lookup"><span data-stu-id="5d328-105">Temporal Tables keep data closely related tootime context so that stored facts can be interpreted as valid only within hello specific period.</span></span> <span data-ttu-id="5d328-106">Den här egenskapen för Temporala tabeller möjliggör effektiv tidsbaserade analys och hämtning insikter från utvecklingen av data.</span><span class="sxs-lookup"><span data-stu-id="5d328-106">This property of Temporal Tables allows for efficient time-based analysis and getting insights from data evolution.</span></span>

## <a name="temporal-scenario"></a><span data-ttu-id="5d328-107">Tidsbestämd Scenario</span><span class="sxs-lookup"><span data-stu-id="5d328-107">Temporal Scenario</span></span>
<span data-ttu-id="5d328-108">Den här artikeln visar hello steg tooutilize Temporala tabeller i ett scenario för programmet.</span><span class="sxs-lookup"><span data-stu-id="5d328-108">This article illustrates hello steps tooutilize Temporal Tables in an application scenario.</span></span> <span data-ttu-id="5d328-109">Anta att du vill visa tootrack användaraktivitet på en ny webbplats som utvecklas från grunden eller på en befintlig webbplats som du vill tooextend med aktiviteten analyser.</span><span class="sxs-lookup"><span data-stu-id="5d328-109">Suppose that you want tootrack user activity on a new website that is being developed from scratch or on an existing website that you want tooextend with user activity analytics.</span></span> <span data-ttu-id="5d328-110">I detta exempel antar vi att hello antal besökta webbsidor under en tidsperiod är en symbol som behöver toobe fångas och övervakas i hello webbplats databas som finns på Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5d328-110">In this simplified example, we assume that hello number of visited web pages during a period of time is an indicator that needs toobe captured and monitored in hello website database that is hosted on Azure SQL Database.</span></span> <span data-ttu-id="5d328-111">hello målet med hello historisk analys av användaraktivitet är tooget indata tooredesign webbplats och ge bättre upplevelse för hello besökare.</span><span class="sxs-lookup"><span data-stu-id="5d328-111">hello goal of hello historical analysis of user activity is tooget inputs tooredesign website and provide better experience for hello visitors.</span></span>

<span data-ttu-id="5d328-112">hello databasmodell för det här scenariot är väldigt enkelt - användaren aktivitet mått representeras med ett enda heltalsfält **PageVisited**, och avbildas tillsammans med grundläggande information om hello användarprofil.</span><span class="sxs-lookup"><span data-stu-id="5d328-112">hello database model for this scenario is very simple - user activity metric is represented with a single integer field, **PageVisited**, and is captured along with basic information on hello user profile.</span></span> <span data-ttu-id="5d328-113">Dessutom för klockslag analys skulle du ha en serie med rader för varje användare, där varje rad representerar hello antalet sidor som en viss användare besöker inom en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="5d328-113">Additionally, for time based analysis, you would keep a series of rows for each user, where every row represents hello number of pages a particular user visited within a specific period of time.</span></span>

![Schemat](./media/sql-database-temporal-tables/AzureTemporal1.png)

<span data-ttu-id="5d328-115">Som tur är kan du behöver inte tooput alla arbete i din app toomaintain informationen aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5d328-115">Fortunately, you do not need tooput any effort in your app toomaintain this activity information.</span></span> <span data-ttu-id="5d328-116">Med Temporala tabeller är den här processen automatiskt - så att du fullständig flexibilitet under webbdesign och mer tid toofocus på hello dataanalys sig själv.</span><span class="sxs-lookup"><span data-stu-id="5d328-116">With Temporal Tables, this process is automated - giving you full flexibility during website design and more time toofocus on hello data analysis itself.</span></span> <span data-ttu-id="5d328-117">Hej endast du har toodo är tooensure som **WebSiteInfo** tabellen har konfigurerats som [temporala systemversionstabeller](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="5d328-117">hello only thing you have toodo is tooensure that **WebSiteInfo** table is configured as [temporal system-versioned](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span></span> <span data-ttu-id="5d328-118">Hej hur tooutilize Temporala tabeller i det här scenariot beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="5d328-118">hello exact steps tooutilize Temporal Tables in this scenario are described below.</span></span>

## <a name="step-1-configure-tables-as-temporal"></a><span data-ttu-id="5d328-119">Steg 1: Konfigurera tabeller som temporala</span><span class="sxs-lookup"><span data-stu-id="5d328-119">Step 1: Configure tables as temporal</span></span>
<span data-ttu-id="5d328-120">Beroende på om du startar nyutveckling eller uppgradera befintliga program, ska du skapa temporala tabeller eller ändra befintliga genom att lägga till temporala attribut.</span><span class="sxs-lookup"><span data-stu-id="5d328-120">Depending on whether you are starting new development or upgrading existing application, you will either create temporal tables or modify existing ones by adding temporal attributes.</span></span> <span data-ttu-id="5d328-121">I allmänhet ärende, ditt scenario kan vara en blandning av de här två alternativen.</span><span class="sxs-lookup"><span data-stu-id="5d328-121">In general case, your scenario can be a mix of these two options.</span></span> <span data-ttu-id="5d328-122">Utför dessa åtgärder med hjälp av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) eller andra Transact-SQL-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="5d328-122">Perform these action using [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) or any other Transact-SQL development tool.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d328-123">Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5d328-123">It is recommended that you always use hello latest version of Management Studio tooremain synchronized with updates tooMicrosoft Azure and SQL Database.</span></span> <span data-ttu-id="5d328-124">[Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d328-124">[Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span></span>
> 
> 

### <a name="create-new-table"></a><span data-ttu-id="5d328-125">Skapa ny tabell</span><span class="sxs-lookup"><span data-stu-id="5d328-125">Create new table</span></span>
<span data-ttu-id="5d328-126">Använda kontexten menyalternativet ”nya Systemversionstabellen” i Object Explorer i SSMS tooopen hello frågeredigeraren med ett skript för temporala tabellen mallar och sedan ”ange värden för mallparametrar” (Ctrl + Skift + M) toopopulate hello mallen:</span><span class="sxs-lookup"><span data-stu-id="5d328-126">Use context menu item “New System-Versioned Table” in SSMS Object Explorer tooopen hello query editor with a temporal table template script and then use “Specify Values for Template Parameters” (Ctrl+Shift+M) toopopulate hello template:</span></span>

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

<span data-ttu-id="5d328-128">I SSDT, väljer du ”Temporala tabellen (Systemversionstabeller)” mallen när du lägger till nya objekt toohello databasprojektet.</span><span class="sxs-lookup"><span data-stu-id="5d328-128">In SSDT, chose “Temporal Table (System-Versioned)” template when adding new items toohello database project.</span></span> <span data-ttu-id="5d328-129">Att kommer öppna tabelldesignern och aktivera du tooeasily ange hello tabellayout:</span><span class="sxs-lookup"><span data-stu-id="5d328-129">That will open table designer and enable you tooeasily specify hello table layout:</span></span>

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

<span data-ttu-id="5d328-131">Du kan också en temporal tabell skapa genom att ange hello Transact-SQL-instruktioner direkt, enligt hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="5d328-131">You can also a create temporal table by specifying hello Transact-SQL statements directly, as shown in hello example below.</span></span> <span data-ttu-id="5d328-132">Observera att hello obligatoriska elementen i varje temporal tabell är hello PERIOD-definitionen och hello SYSTEM_VERSIONING-satsen med en referens tooanother tabell som lagrar historisk raden versioner:</span><span class="sxs-lookup"><span data-stu-id="5d328-132">Note that hello mandatory elements of every temporal table are hello PERIOD definition and hello SYSTEM_VERSIONING clause with a reference tooanother user table that will store historical row versions:</span></span>

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

<span data-ttu-id="5d328-133">När du skapar en temporal systemversionstabell kan skapas automatiskt hello tillhörande historiktabellen med hello standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="5d328-133">When you create system-versioned temporal table, hello accompanying history table with hello default configuration is automatically created.</span></span> <span data-ttu-id="5d328-134">hello standard historiktabellen innehåller ett grupperat index B-trädet på hello periodkolumner (end start) med sidan komprimering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="5d328-134">hello default history table contains a clustered B-tree index on hello period columns (end, start) with page compression enabled.</span></span> <span data-ttu-id="5d328-135">Den här konfigurationen är optimal för hello merparten av scenarier där temporala tabeller används, särskilt för [data granskning](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="5d328-135">This configuration is optimal for hello majority of scenarios in which temporal tables are used, especially for [data auditing](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span></span> 

<span data-ttu-id="5d328-136">I det här fallet mål tooperform tidsbaserade trendanalys över en längre data historik och större datauppsättningar så hello lagring valet för hello historiktabellen är ett grupperat columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="5d328-136">In this particular case, we aim tooperform time-based trend analysis over a longer data history and with bigger data sets, so hello storage choice for hello history table is a clustered columnstore index.</span></span> <span data-ttu-id="5d328-137">Ett grupperat columnstore ger mycket bra komprimering och prestanda för analytiska frågor.</span><span class="sxs-lookup"><span data-stu-id="5d328-137">A clustered columnstore provides very good compression and performance for analytical queries.</span></span> <span data-ttu-id="5d328-138">Temporala tabeller ger du hello flexibilitet tooconfigure index för hello aktuella och temporala tabeller helt oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="5d328-138">Temporal Tables give you hello flexibility tooconfigure indexes on hello current and temporal tables completely independently.</span></span> 

> [!NOTE]
> <span data-ttu-id="5d328-139">Columnstore-index är bara tillgängliga i hello premiumnivån.</span><span class="sxs-lookup"><span data-stu-id="5d328-139">Columnstore indexes are only available in hello premium service tier.</span></span>
>

<span data-ttu-id="5d328-140">hello följande skript visar hur Standardindex för historiktabellen kan vara ändrade toohello grupperade columnstore:</span><span class="sxs-lookup"><span data-stu-id="5d328-140">hello following script shows how default index on history table can be changed toohello clustered columnstore:</span></span>

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

<span data-ttu-id="5d328-141">Temporala tabeller representeras i hello Object Explorer med hello specifika ikon för att lättare identifiera när dess historiktabellen visas som en underordnad nod.</span><span class="sxs-lookup"><span data-stu-id="5d328-141">Temporal Tables are represented in hello Object Explorer with hello specific icon for easier identification, while its history table is displayed as a child node.</span></span>

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a><span data-ttu-id="5d328-143">Ändra den befintliga tabellen tootemporal</span><span class="sxs-lookup"><span data-stu-id="5d328-143">Alter existing table tootemporal</span></span>
<span data-ttu-id="5d328-144">Vi ska omfatta hello alternativa scenariot i vilka hello WebsiteUserInfo tabellen redan finns, men var inte utformad tookeep en historik över ändringarna.</span><span class="sxs-lookup"><span data-stu-id="5d328-144">Let’s cover hello alternative scenario in which hello WebsiteUserInfo table already exists, but was not designed tookeep a history of changes.</span></span> <span data-ttu-id="5d328-145">I det här fallet kan du helt enkelt utöka hello befintliga tabell toobecome temporala som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="5d328-145">In this case, you can simply extend hello existing table toobecome temporal, as shown in hello following example:</span></span>

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

## <a name="step-2-run-your-workload-regularly"></a><span data-ttu-id="5d328-146">Steg 2: Kör din arbetsbelastning regelbundet</span><span class="sxs-lookup"><span data-stu-id="5d328-146">Step 2: Run your workload regularly</span></span>
<span data-ttu-id="5d328-147">hello största fördelen med Temporala tabeller är att du inte behöver toochange eller justera webbplatsen i alla sätt tooperform ändringsspårning.</span><span class="sxs-lookup"><span data-stu-id="5d328-147">hello main advantage of Temporal Tables is that you do not need toochange or adjust your website in any way tooperform change tracking.</span></span> <span data-ttu-id="5d328-148">När du skapat kvarstår Temporala tabeller transparent tidigare versioner av raden varje gång du genomför ändringar på dina data.</span><span class="sxs-lookup"><span data-stu-id="5d328-148">Once created, Temporal Tables transparently persist previous row versions every time you perform modifications on your data.</span></span> 

<span data-ttu-id="5d328-149">I ordning tooleverage automatisk ändringsspårning för det här scenariot, vi bara uppdatera kolumnen **PagesVisited** varje gång när användaren slutar göra åtaganden session på hello webbplats:</span><span class="sxs-lookup"><span data-stu-id="5d328-149">In order tooleverage automatic change tracking for this particular scenario, let’s just update column **PagesVisited** every time when user ends her/his session on hello website:</span></span>

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

<span data-ttu-id="5d328-150">Det är viktigt toonotice som hello update-fråga inte behöver tooknow hello exakt tid när hello faktiska åtgärden uppstod eller hur historiska data sparas för framtida analys.</span><span class="sxs-lookup"><span data-stu-id="5d328-150">It is important toonotice that hello update query doesn’t need tooknow hello exact time when hello actual operation occurred nor how historical data will be preserved for future analysis.</span></span> <span data-ttu-id="5d328-151">Båda aspekter hanteras automatiskt av hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5d328-151">Both aspects are automatically handled by hello Azure SQL Database.</span></span> <span data-ttu-id="5d328-152">hello följande diagram illustrerar hur historikdata genereras på varje uppdatering.</span><span class="sxs-lookup"><span data-stu-id="5d328-152">hello following diagram illustrates how history data is being generated on every update.</span></span>

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a><span data-ttu-id="5d328-154">Steg 3: Analysera historiska data</span><span class="sxs-lookup"><span data-stu-id="5d328-154">Step 3: Perform historical data analysis</span></span>
<span data-ttu-id="5d328-155">Nu när temporala systemversionshanteringen är aktiverat, historiska dataanalys är bara en fråga från dig.</span><span class="sxs-lookup"><span data-stu-id="5d328-155">Now when temporal system-versioning is enabled, historical data analysis is just one query away from you.</span></span> <span data-ttu-id="5d328-156">I den här artikeln ska vi förse några exempel som löser vanliga scenarier för analys - toolearn alla uppgifter, utforska olika alternativ som introducerades med hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) satsen.</span><span class="sxs-lookup"><span data-stu-id="5d328-156">In this article, we will provide a few examples that address common analysis scenarios - toolearn all details, explore various options introduced with hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) clause.</span></span>

<span data-ttu-id="5d328-157">toosee hello översta 10 användare sorterade efter hello antal besökta webbsidor från och med en timme sedan, köra den här frågan:</span><span class="sxs-lookup"><span data-stu-id="5d328-157">toosee hello top 10 users ordered by hello number of visited web pages as of an hour ago, run this query:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

<span data-ttu-id="5d328-158">Du kan enkelt ändra den här frågan besöker tooanalyze hello platsen från och med en dag sedan, för en månad sedan eller när som helst hello senaste du önskar.</span><span class="sxs-lookup"><span data-stu-id="5d328-158">You can easily modify this query tooanalyze hello site visits as of a day ago, a month ago or at any point in hello past you wish.</span></span>

<span data-ttu-id="5d328-159">tooperform grundläggande statistisk analys för hello föregående dag, Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="5d328-159">tooperform basic statistical analysis for hello previous day, use hello following example:</span></span>

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

<span data-ttu-id="5d328-160">toosearch för aktiviteter för en specifik användare inom en viss tidsperiod, Använd hello INNESLUTNA IN-sats:</span><span class="sxs-lookup"><span data-stu-id="5d328-160">toosearch for activities of a specific user, within a period of time, use hello CONTAINED IN clause:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

<span data-ttu-id="5d328-161">Grafisk visualiseringen är särskilt användbart för temporala frågor som du kan visa trender och användningsmönster i ett intuitivt sätt enkelt:</span><span class="sxs-lookup"><span data-stu-id="5d328-161">Graphic visualization is especially convenient for temporal queries as you can show trends and usage patterns in an intuitive way very easily:</span></span>

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a><span data-ttu-id="5d328-163">Utvecklingen av tabellens schema</span><span class="sxs-lookup"><span data-stu-id="5d328-163">Evolving table schema</span></span>
<span data-ttu-id="5d328-164">Vanligtvis behöver du toochange hello temporal tabellschemat medan du arbetar med app-utveckling.</span><span class="sxs-lookup"><span data-stu-id="5d328-164">Typically, you will need toochange hello temporal table schema while you are doing app development.</span></span> <span data-ttu-id="5d328-165">För att bara köra regelbundna ALTER TABLE-instruktioner och Azure SQL Database på lämpligt sätt kommer att sprida ändringarna toohello historiktabellen.</span><span class="sxs-lookup"><span data-stu-id="5d328-165">For that, simply run regular ALTER TABLE statements and Azure SQL Database will appropriately propagate changes toohello history table.</span></span> <span data-ttu-id="5d328-166">hello följande skript visar hur du kan lägga till ytterligare attribut för spårning:</span><span class="sxs-lookup"><span data-stu-id="5d328-166">hello following script shows how you can add additional attribute for tracking:</span></span>

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

<span data-ttu-id="5d328-167">Du kan ändra kolumndefinitionen när din arbetsbelastning är aktiv:</span><span class="sxs-lookup"><span data-stu-id="5d328-167">Similarly, you can change column definition while your workload is active:</span></span>

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

<span data-ttu-id="5d328-168">Slutligen kan du ta bort en kolumn som du inte behöver längre.</span><span class="sxs-lookup"><span data-stu-id="5d328-168">Finally, you can remove a column that you do not need anymore.</span></span>

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

<span data-ttu-id="5d328-169">Du kan också använda senaste [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange temporal tabell schema när du är ansluten toohello databasen (onlineläge) eller som en del av hello databasprojektet (offlineläge).</span><span class="sxs-lookup"><span data-stu-id="5d328-169">Alternatively, use latest [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange temporal table schema while you are connected toohello database (online mode) or as part of hello database project (offline mode).</span></span>

## <a name="controlling-retention-of-historical-data"></a><span data-ttu-id="5d328-170">Kontrollera kvarhållning av historisk data</span><span class="sxs-lookup"><span data-stu-id="5d328-170">Controlling retention of historical data</span></span>
<span data-ttu-id="5d328-171">Med temporala systemversionstabeller öka hello historiktabellen hello databasstorleken mer än vanliga tabeller.</span><span class="sxs-lookup"><span data-stu-id="5d328-171">With system-versioned temporal tables, hello history table may increase hello database size more than regular tables.</span></span> <span data-ttu-id="5d328-172">En stor och ständigt växande historiktabellen kan bli ett problem som både förfallodatum toopure lagringskostnader som införs en prestanda skatt på temporala frågor.</span><span class="sxs-lookup"><span data-stu-id="5d328-172">A large and ever-growing history table can become an issue both due toopure storage costs as well as imposing a performance tax on temporal querying.</span></span> <span data-ttu-id="5d328-173">Därför kan är utveckla en databevarandeprincip för att hantera data i hello historiktabellen en viktig del av planering och hantering av hello livscykeln för alla temporala tabeller.</span><span class="sxs-lookup"><span data-stu-id="5d328-173">Hence, developing a data retention policy for managing data in hello history table is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="5d328-174">Med Azure SQL Database har hello följande metoder för att hantera historiska data i hello temporal tabell:</span><span class="sxs-lookup"><span data-stu-id="5d328-174">With Azure SQL Database, you have hello following approaches for managing historical data in hello temporal table:</span></span>

* [<span data-ttu-id="5d328-175">Tabellpartitionering</span><span class="sxs-lookup"><span data-stu-id="5d328-175">Table Partitioning</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [<span data-ttu-id="5d328-176">Rensning av anpassade skript</span><span class="sxs-lookup"><span data-stu-id="5d328-176">Custom Cleanup Script</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a><span data-ttu-id="5d328-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d328-177">Next steps</span></span>
<span data-ttu-id="5d328-178">Mer detaljerad information om Temporala tabeller kolla [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d328-178">For detailed information on Temporal Tables, check out [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>
<span data-ttu-id="5d328-179">Besök Channel 9 toohear en [verkliga kunden temporal implemenation fungerat](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) och titta på en [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="5d328-179">Visit Channel 9 toohear a [real customer temporal implemenation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

