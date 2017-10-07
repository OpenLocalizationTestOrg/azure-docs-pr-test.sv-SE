---
title: aaaCopy en Azure SQL database | Microsoft Docs
description: Skapa en kopia av en Azure SQL database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5aaf6bcd-3839-49b5-8c77-cbdf786e359b
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 64a297d819d6da89600fda60abe8394ae405abfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-an-azure-sql-database"></a>Kopiera en Azure SQL database

Azure SQL-databasen innehåller flera metoder för att skapa en konsekvent transaktionellt kopia av en befintlig Azure SQL-databas på antingen hello samma eller en annan server. Du kan kopiera en SQL-databas med hjälp av hello Azure-portalen, PowerShell eller T-SQL. 

## <a name="overview"></a>Översikt

En databaskopia är en ögonblicksbild av hello källdatabasen från och med hello tid för hello copy-begäran. Du kan välja hello samma server eller en annan server, dess servicenivå för tjänstnivå och prestandanivå eller en annan prestandanivå inom hello samma tjänstnivån (edition). När hello kopieringen är klar blir den en fullt fungerande oberoende databas. Nu kan du uppgradera eller nedgradera den tooany edition. hello-inloggningar, användare och behörigheter kan hanteras oberoende av varandra.  

## <a name="logins-in-hello-database-copy"></a>Inloggningar i hello databaskopian

När du kopierar en databas toohello samma logiska server, hello samma inloggningar som kan användas på båda databaserna. hello säkerhetsobjekt du använder toocopy hello databas blir hello databasägaren på hello ny databas. Alla användare och deras behörigheter säkerhet säkerhetsidentifierare (SID) är kopieras toohello databaskopian.  

När du kopierar en tooa olika logiska databasserver, blir hello säkerhetsobjekt på hello nya servern hello databasägaren på hello ny databas. Om du använder [innehöll databasanvändare](sql-database-manage-logins.md) för dataåtkomst, kontrollera att både hello primära och sekundära databaser har alltid hello samma autentiseringsuppgifter, så som när hello kopieringen är klar har direkt åtkomst till den med hello samma autentiseringsuppgifter. 

Om du använder [Azure Active Directory](../active-directory/active-directory-whatis.md), du kan helt eliminera hello behovet av att hantera autentiseringsuppgifter i hello kopia. Men när du kopierar hello tooa nya databasservern kanske hello inloggnings-baserad åtkomst inte fungerar, eftersom hello inloggningar inte finns på hello ny server. toolearn om hur du hanterar inloggningar när du kopierar en tooa olika logiska databasserver, se [hur toomanage Azure SQL-databas säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md). 

När hello kopiera lyckas och innan andra användare mappas om kan endast hello inloggning som initierade hello kopierar, hello databasägaren logga in toohello ny databas. tooresolve inloggningar när hello Kopiera åtgärden är klar finns [lösa inloggningar](#resolve-logins).

## <a name="copy-a-database-by-using-hello-azure-portal"></a>Kopiera en databas med hjälp av hello Azure-portalen

toocopy en databas med hjälp av hello Azure-portalen, öppna hello sidan för databasen, och klicka sedan på **kopiera**. 

   ![Databaskopian](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Kopiera en databas med hjälp av PowerShell

toocopy en databas med hjälp av PowerShell, Använd hello [ny AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

En fullständig exempelskript finns [kopiera en tooa ny databasserver](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Kopiera en databas med hjälp av Transact-SQL

Logga in toohello master databasen med hello huvudsaklig inloggning på servernivå eller hello-inloggning som skapade hello-databas som du vill toocopy. Kopiera toosucceed-databas måste inloggningar som inte är hello servernivå huvudnamn vara medlemmar i hello dbmanager roll. Läs mer om inloggningar och anslutande toohello [hantera inloggningar](sql-database-manage-logins.md).

Börja kopiera hello källdatabasen med hello [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) instruktionen. Kör den här instruktionen initierar hello databasen kopiera processen. Eftersom kopiera en databas är en asynkron åtgärd, returnerar hello CREATE DATABASE-instruktionen innan hello databasen kopieringen har slutförts.

### <a name="copy-a-sql-database-toohello-same-server"></a>Kopiera en SQL-databas toohello samma server
Logga in toohello master databasen med hello huvudsaklig inloggning på servernivå eller hello-inloggning som skapade hello-databas som du vill toocopy. Kopiera toosucceed-databas måste inloggningar som inte är hello servernivå huvudnamn vara medlemmar i hello dbmanager roll.

Det här kommandot kopieras Database1 tooa ny databas med namnet Database2 på hello samma server. Hello Kopiera åtgärden kan ta viss tid toocomplete beroende hello storleken på databasen.

    -- Execute on hello master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-tooa-different-server"></a>Kopiera en annan server för SQL-databasen tooa

Logga in toohello master databasen på målservern hello, där hello ny databas är toobe skapade hello-SQL-databasservern. Använd en inloggning som har hello samma namn och lösenord som hello databasägaren hello källdatabasens på hello källservern SQL-databasen. hello logga in på målservern för hello måste också vara medlem i rollen för hello dbmanager eller vara hello huvudsaklig inloggning på servernivå.

Det här kommandot kopieras Database1 på server1 tooa ny databas med namnet Database2 på server2. Hello Kopiera åtgärden kan ta viss tid toocomplete beroende hello storleken på databasen.

    -- Execute on hello master database of hello target server (server2)
    -- Start copying from Server1 tooServer2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-hello-progress-of-hello-copying-operation"></a>Övervaka hello hello Kopiera åtgärden

Övervaka hello kopieringen genom att fråga hello sys.databases och sys.dm_database_copies vyer. När hello kopiera pågår, hello **state_desc** kolumnen hello sys.databases vyn för hello ny databas har angetts för**kopiering**.

* Om hello kopiera misslyckas hello **state_desc** kolumnen hello sys.databases vyn för hello ny databas har angetts för**MISSTÄNKER**. Köra hello DROP-uttryck på hello ny databas och försök igen senare.
* Om hello kopiera lyckas hello **state_desc** kolumnen hello sys.databases vyn för hello ny databas har angetts för**ONLINE**. hello kopieringen har slutförts och hello ny databas är en vanlig databas som kan ändras oberoende av hello källdatabasen.

> [!NOTE]
> Om du väljer toocancel hello kopiera medan det pågår, köra hello [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) satsen på hello ny databas. Du kan också avbryter köra hello DROP DATABASE-instruktionen på hello källdatabasen också hello kopiera processen.
> 

## <a name="resolve-logins"></a>Lös inloggningar

När nya hello-databasen är online på målservern för hello använda hello [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) instruktionen tooremap hello användare från hello databasen nya toologins på hello-målservern. tooresolve ägarlösa användare se [felsöka ägarlösa användare](https://msdn.microsoft.com/library/ms175475.aspx). Se även [hur toomanage Azure SQL-databas säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).

Alla användare i hello ny databas behåller hello behörigheter som de hade i hello källdatabasen. hello-användaren som initierade hello databaskopian blir hello databasens ägare av den nya hello-databasen och tilldelas en ny säkerhetsidentifierare (SID). När hello kopiera lyckas och innan andra användare mappas om kan endast hello inloggning som initierade hello kopierar, hello databasägaren logga in toohello ny databas.

toolearn om att hantera användare och inloggningar när du kopierar en tooa olika logiska databasserver, se [hur toomanage Azure SQL-databas säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Nästa steg

* Information om inloggningar finns [hantera inloggningar](sql-database-manage-logins.md) och [hur toomanage Azure SQL-databas säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).
* tooexport en databas finns [exportera hello databasen tooa BACPAC](sql-database-export.md).
