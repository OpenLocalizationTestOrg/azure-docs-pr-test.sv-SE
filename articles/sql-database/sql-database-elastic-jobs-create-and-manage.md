---
title: aaaManage grupper med Azure SQL-databaser | Microsoft Docs
description: "Gå igenom skapande och hantering av ett elastiska jobb."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Skapa och hantera skaländras ut Azure SQL-databaser med elastiska jobb (förhandsgranskning)


**Den elastiska databasen jobb** förenkla hanteringen av grupper av databaser genom att utföra administrativa åtgärder, till exempel schemaändringar, hantering av autentiseringsuppgifter, referens uppdateringar, insamling av prestandadata eller klient (kunden) telemetri samling. Elastiska jobb i databasen är tillgänglig via hello Azure portal och PowerShell-cmdlets. Dock hello Azure portal hämtar nedsatt funktionalitet begränsad tooexecution över alla databaser i en [elastisk pool (förhandsgranskning)](sql-database-elastic-pool.md). ställa in tooaccess ytterligare funktioner och körning av skript i en grupp databaser inklusive en egendefinierat samling eller en Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-scale-introduction.md)), finns [skapa och hantera jobb med hjälp av PowerShell](sql-database-elastic-jobs-powershell.md). Mer information om jobb finns [elastisk databas översikt över](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* En elastisk pool. Se [om elastiska pooler](sql-database-elastic-pool.md).
* Installation av tjänstkomponenter för elastisk databas jobb. Se [installerar hello elastiska jobb databastjänsten](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Jobb
1. Med hjälp av hello [Azure-portalen](https://portal.azure.com), från en befintlig elastisk jobbet databaspool, klicka på **skapa jobbet**.
2. Ange hello användarnamnet och lösenordet för hello databasadministratören (skapas vid installationen av jobb) för databasen för hello jobb (metadata lagring för jobb).
   
    ![Hello jobb, Skriv eller klistra in koden och klicka på Kör][1]
3. I hello **skapa jobbet** bladet, ange ett namn för hello jobb.
4. Ange hello användarens namn och lösenord tooconnect toohello måldatabaserna med tillräcklig behörighet för toosucceed för körning av skript.
5. Klistra in eller ange hello T-SQL-skript.
6. Klicka på **spara** och klicka sedan på **kör**.
   
    ![Skapa jobb och köra][5]

## <a name="run-idempotent-jobs"></a>Kör jobb för idempotent
När du kör ett skript mot en uppsättning databaser måste du vara säker på att hello skript är idempotent. Det vill säga måste hello skriptet vara kan toorun flera gånger, även om den har misslyckats innan i ett ofullständigt tillstånd. Till exempel när ett skript inte hello jobb automatiskt görs tills den lyckas (inom intervallet, som hello försök logik slutligen upphöra hello du försöker). hello sätt toodo detta är toouse hello instruktionen ”om det finns” och ta bort alla hittade instansen innan du skapar ett nytt objekt. Här visas ett exempel:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Du kan också använda en ”om inte finns”-sats innan du skapar en ny instans:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Det här skriptet uppdaterar hello-tabell som skapats tidigare.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Kontrollera jobbstatus
När ett jobb har startat, du kan kontrollera förloppet.

1. Hello elastisk pool sidan klickar du på **hantera jobb**.
   
    ![Klicka på ”Hantera jobb”][2]
2. Klicka på hello namn (a) för ett jobb. Hej **STATUS** kan vara ”slutfört” eller ”misslyckades”. hello jobbinformation visas (b) med dess datum och tid för att skapa och köra. hello lista (c) under hello som visar hello fortskrider hello-skript mot varje databas i hello pool ger information om datum och tid.
   
    ![Kontrollen en klar][3]

## <a name="checking-failed-jobs"></a>Det gick inte att kontrollera jobb
Om ett jobb inte kan hitta en logg över exekveringen. Klicka på hello misslyckades jobb toosee hello namn information.

![Markera ett jobb som misslyckades][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


