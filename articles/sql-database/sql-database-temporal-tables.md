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
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Komma igång med Temporala tabeller i Azure SQL-databas
Temporala tabeller är en ny funktion för programmering i Azure SQL Database som gör att du tootrack och analysera hello hela historiken för ändringar i dina data utan att hello behöver anpassade kodning. Temporala tabeller behålla nära relaterade tootime datakontexten så att lagrade fakta kan tolkas som giltig endast inom hello specifik period. Den här egenskapen för Temporala tabeller möjliggör effektiv tidsbaserade analys och hämtning insikter från utvecklingen av data.

## <a name="temporal-scenario"></a>Tidsbestämd Scenario
Den här artikeln visar hello steg tooutilize Temporala tabeller i ett scenario för programmet. Anta att du vill visa tootrack användaraktivitet på en ny webbplats som utvecklas från grunden eller på en befintlig webbplats som du vill tooextend med aktiviteten analyser. I detta exempel antar vi att hello antal besökta webbsidor under en tidsperiod är en symbol som behöver toobe fångas och övervakas i hello webbplats databas som finns på Azure SQL Database. hello målet med hello historisk analys av användaraktivitet är tooget indata tooredesign webbplats och ge bättre upplevelse för hello besökare.

hello databasmodell för det här scenariot är väldigt enkelt - användaren aktivitet mått representeras med ett enda heltalsfält **PageVisited**, och avbildas tillsammans med grundläggande information om hello användarprofil. Dessutom för klockslag analys skulle du ha en serie med rader för varje användare, där varje rad representerar hello antalet sidor som en viss användare besöker inom en viss tidsperiod.

![Schemat](./media/sql-database-temporal-tables/AzureTemporal1.png)

Som tur är kan du behöver inte tooput alla arbete i din app toomaintain informationen aktivitet. Med Temporala tabeller är den här processen automatiskt - så att du fullständig flexibilitet under webbdesign och mer tid toofocus på hello dataanalys sig själv. Hej endast du har toodo är tooensure som **WebSiteInfo** tabellen har konfigurerats som [temporala systemversionstabeller](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Hej hur tooutilize Temporala tabeller i det här scenariot beskrivs nedan.

## <a name="step-1-configure-tables-as-temporal"></a>Steg 1: Konfigurera tabeller som temporala
Beroende på om du startar nyutveckling eller uppgradera befintliga program, ska du skapa temporala tabeller eller ändra befintliga genom att lägga till temporala attribut. I allmänhet ärende, ditt scenario kan vara en blandning av de här två alternativen. Utför dessa åtgärder med hjälp av [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) eller andra Transact-SQL-utvecklingsverktyg.

> [!IMPORTANT]
> Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Skapa ny tabell
Använda kontexten menyalternativet ”nya Systemversionstabellen” i Object Explorer i SSMS tooopen hello frågeredigeraren med ett skript för temporala tabellen mallar och sedan ”ange värden för mallparametrar” (Ctrl + Skift + M) toopopulate hello mallen:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

I SSDT, väljer du ”Temporala tabellen (Systemversionstabeller)” mallen när du lägger till nya objekt toohello databasprojektet. Att kommer öppna tabelldesignern och aktivera du tooeasily ange hello tabellayout:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Du kan också en temporal tabell skapa genom att ange hello Transact-SQL-instruktioner direkt, enligt hello exemplet nedan. Observera att hello obligatoriska elementen i varje temporal tabell är hello PERIOD-definitionen och hello SYSTEM_VERSIONING-satsen med en referens tooanother tabell som lagrar historisk raden versioner:

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

När du skapar en temporal systemversionstabell kan skapas automatiskt hello tillhörande historiktabellen med hello standardkonfiguration. hello standard historiktabellen innehåller ett grupperat index B-trädet på hello periodkolumner (end start) med sidan komprimering har aktiverats. Den här konfigurationen är optimal för hello merparten av scenarier där temporala tabeller används, särskilt för [data granskning](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

I det här fallet mål tooperform tidsbaserade trendanalys över en längre data historik och större datauppsättningar så hello lagring valet för hello historiktabellen är ett grupperat columnstore-index. Ett grupperat columnstore ger mycket bra komprimering och prestanda för analytiska frågor. Temporala tabeller ger du hello flexibilitet tooconfigure index för hello aktuella och temporala tabeller helt oberoende av varandra. 

> [!NOTE]
> Columnstore-index är bara tillgängliga i hello premiumnivån.
>

hello följande skript visar hur Standardindex för historiktabellen kan vara ändrade toohello grupperade columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Temporala tabeller representeras i hello Object Explorer med hello specifika ikon för att lättare identifiera när dess historiktabellen visas som en underordnad nod.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a>Ändra den befintliga tabellen tootemporal
Vi ska omfatta hello alternativa scenariot i vilka hello WebsiteUserInfo tabellen redan finns, men var inte utformad tookeep en historik över ändringarna. I det här fallet kan du helt enkelt utöka hello befintliga tabell toobecome temporala som visas i följande exempel hello:

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

## <a name="step-2-run-your-workload-regularly"></a>Steg 2: Kör din arbetsbelastning regelbundet
hello största fördelen med Temporala tabeller är att du inte behöver toochange eller justera webbplatsen i alla sätt tooperform ändringsspårning. När du skapat kvarstår Temporala tabeller transparent tidigare versioner av raden varje gång du genomför ändringar på dina data. 

I ordning tooleverage automatisk ändringsspårning för det här scenariot, vi bara uppdatera kolumnen **PagesVisited** varje gång när användaren slutar göra åtaganden session på hello webbplats:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Det är viktigt toonotice som hello update-fråga inte behöver tooknow hello exakt tid när hello faktiska åtgärden uppstod eller hur historiska data sparas för framtida analys. Båda aspekter hanteras automatiskt av hello Azure SQL Database. hello följande diagram illustrerar hur historikdata genereras på varje uppdatering.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>Steg 3: Analysera historiska data
Nu när temporala systemversionshanteringen är aktiverat, historiska dataanalys är bara en fråga från dig. I den här artikeln ska vi förse några exempel som löser vanliga scenarier för analys - toolearn alla uppgifter, utforska olika alternativ som introducerades med hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) satsen.

toosee hello översta 10 användare sorterade efter hello antal besökta webbsidor från och med en timme sedan, köra den här frågan:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Du kan enkelt ändra den här frågan besöker tooanalyze hello platsen från och med en dag sedan, för en månad sedan eller när som helst hello senaste du önskar.

tooperform grundläggande statistisk analys för hello föregående dag, Använd följande exempel hello:

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

toosearch för aktiviteter för en specifik användare inom en viss tidsperiod, Använd hello INNESLUTNA IN-sats:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafisk visualiseringen är särskilt användbart för temporala frågor som du kan visa trender och användningsmönster i ett intuitivt sätt enkelt:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Utvecklingen av tabellens schema
Vanligtvis behöver du toochange hello temporal tabellschemat medan du arbetar med app-utveckling. För att bara köra regelbundna ALTER TABLE-instruktioner och Azure SQL Database på lämpligt sätt kommer att sprida ändringarna toohello historiktabellen. hello följande skript visar hur du kan lägga till ytterligare attribut för spårning:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Du kan ändra kolumndefinitionen när din arbetsbelastning är aktiv:

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Slutligen kan du ta bort en kolumn som du inte behöver längre.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

Du kan också använda senaste [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange temporal tabell schema när du är ansluten toohello databasen (onlineläge) eller som en del av hello databasprojektet (offlineläge).

## <a name="controlling-retention-of-historical-data"></a>Kontrollera kvarhållning av historisk data
Med temporala systemversionstabeller öka hello historiktabellen hello databasstorleken mer än vanliga tabeller. En stor och ständigt växande historiktabellen kan bli ett problem som både förfallodatum toopure lagringskostnader som införs en prestanda skatt på temporala frågor. Därför kan är utveckla en databevarandeprincip för att hantera data i hello historiktabellen en viktig del av planering och hantering av hello livscykeln för alla temporala tabeller. Med Azure SQL Database har hello följande metoder för att hantera historiska data i hello temporal tabell:

* [Tabellpartitionering](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Rensning av anpassade skript](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Nästa steg
Mer detaljerad information om Temporala tabeller kolla [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn935015.aspx).
Besök Channel 9 toohear en [verkliga kunden temporal implemenation fungerat](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) och titta på en [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

