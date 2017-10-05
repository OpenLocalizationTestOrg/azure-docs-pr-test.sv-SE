---
title: Kopiera en Azure SQL database | Microsoft Docs
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
ms.openlocfilehash: 8c1e3c80b9f24089dc99463d6ea8ae5d0ea7b19d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="copy-an-azure-sql-database"></a><span data-ttu-id="cdc3f-103">Kopiera en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="cdc3f-103">Copy an Azure SQL database</span></span>

<span data-ttu-id="cdc3f-104">Azure SQL Database erbjuder flera metoder för att skapa en konsekvent transaktionellt kopia av en befintlig Azure SQL-databas på samma server eller en annan server.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-104">Azure SQL Database provides several methods for creating a transactionally consistent copy of an existing Azure SQL database on either the same server or a different server.</span></span> <span data-ttu-id="cdc3f-105">Du kan kopiera en SQL-databas med hjälp av Azure-portalen, PowerShell eller T-SQL.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-105">You can copy a SQL database by using the Azure portal, PowerShell, or T-SQL.</span></span> 

## <a name="overview"></a><span data-ttu-id="cdc3f-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="cdc3f-106">Overview</span></span>

<span data-ttu-id="cdc3f-107">En databaskopia är en ögonblicksbild av källdatabasen vid tiden för copy-begäran.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-107">A database copy is a snapshot of the source database as of the time of the copy request.</span></span> <span data-ttu-id="cdc3f-108">Du kan välja samma server eller en annan server, dess tjänstnivå och prestandanivå eller en annan prestandanivå inom samma tjänstnivån (edition).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-108">You can select the same server or a different server, its service tier and performance level, or a different performance level within the same service tier (edition).</span></span> <span data-ttu-id="cdc3f-109">När kopieringen är klar, blir den en fullt fungerande oberoende databas.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-109">After the copy is complete, it becomes a fully functional, independent database.</span></span> <span data-ttu-id="cdc3f-110">Nu kan du uppgradera eller nedgradera till en utgåva.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-110">At this point, you can upgrade or downgrade it to any edition.</span></span> <span data-ttu-id="cdc3f-111">Inloggningar, användare och behörigheter kan hanteras oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-111">The logins, users, and permissions can be managed independently.</span></span>  

## <a name="logins-in-the-database-copy"></a><span data-ttu-id="cdc3f-112">Inloggningar i databaskopian</span><span class="sxs-lookup"><span data-stu-id="cdc3f-112">Logins in the database copy</span></span>

<span data-ttu-id="cdc3f-113">När du kopierar en databas till samma logiska server kan samma inloggningar användas på båda databaserna.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-113">When you copy a database to the same logical server, the same logins can be used on both databases.</span></span> <span data-ttu-id="cdc3f-114">Säkerhetsobjektet du använda för att kopiera databasen blir databasägaren på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-114">The security principal you use to copy the database becomes the database owner on the new database.</span></span> <span data-ttu-id="cdc3f-115">Alla användare och deras behörigheter sina säkerhetsidentifierare (SID) kopieras till databaskopian.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-115">All database users, their permissions, and their security identifiers (SIDs) are copied to the database copy.</span></span>  

<span data-ttu-id="cdc3f-116">När du kopierar en databas till en annan logisk server blir säkerhetsobjekt på den nya servern databasägaren på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-116">When you copy a database to a different logical server, the security principal on the new server becomes the database owner on the new database.</span></span> <span data-ttu-id="cdc3f-117">Om du använder [innehöll databasanvändare](sql-database-manage-logins.md) för dataåtkomst, se till att både de primära och sekundära databaserna alltid har samma autentiseringsuppgifter så att när kopieringen är klar du omedelbart åt den med samma autentiseringsuppgifter .</span><span class="sxs-lookup"><span data-stu-id="cdc3f-117">If you use [contained database users](sql-database-manage-logins.md) for data access, ensure that both the primary and secondary databases always have the same user credentials, so that after the copy is complete you can immediately access it with the same credentials.</span></span> 

<span data-ttu-id="cdc3f-118">Om du använder [Azure Active Directory](../active-directory/active-directory-whatis.md), du kan helt eliminera behovet av att hantera autentiseringsuppgifter i kopian.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-118">If you use [Azure Active Directory](../active-directory/active-directory-whatis.md), you can completely eliminate the need for managing credentials in the copy.</span></span> <span data-ttu-id="cdc3f-119">Men när du kopierar databasen till en ny server kanske inloggnings-baserad åtkomst inte fungerar, eftersom inloggningar inte finns på den nya servern.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-119">However, when you copy the database to a new server, the login-based access might not work, because the logins do not exist on the new server.</span></span> <span data-ttu-id="cdc3f-120">Läs om hur du hanterar inloggningar när du kopierar en databas till en annan logisk server i [hur du hanterar Azure SQL database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-120">To learn about managing logins when you copy a database to a different logical server, see [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span> 

<span data-ttu-id="cdc3f-121">När kopieringen lyckas och innan andra användare mappas om logga inloggningen som initierade kopierar, databasägaren, in på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-121">After the copying succeeds and before other users are remapped, only the login that initiated the copying, the database owner, can log in to the new database.</span></span> <span data-ttu-id="cdc3f-122">Lös inloggningar efter kopiering åtgärden har slutförts, se [lösa inloggningar](#resolve-logins).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-122">To resolve logins after the copying operation is complete, see [Resolve logins](#resolve-logins).</span></span>

## <a name="copy-a-database-by-using-the-azure-portal"></a><span data-ttu-id="cdc3f-123">Kopiera en databas med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="cdc3f-123">Copy a database by using the Azure portal</span></span>

<span data-ttu-id="cdc3f-124">Öppna sidan för databasen om du vill kopiera en databas med hjälp av Azure portal och klicka sedan på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-124">To copy a database by using the Azure portal, open the page for your database, and then click **Copy**.</span></span> 

   ![Databaskopian](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a><span data-ttu-id="cdc3f-126">Kopiera en databas med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cdc3f-126">Copy a database by using PowerShell</span></span>

<span data-ttu-id="cdc3f-127">Om du vill kopiera en databas med hjälp av PowerShell, Använd den [ny AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-127">To copy a database by using PowerShell, use the [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet.</span></span> 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

<span data-ttu-id="cdc3f-128">En fullständig exempelskript finns [kopiera en databas till en ny server](scripts/sql-database-copy-database-to-new-server-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-128">For a complete sample script, see [Copy a database to a new server](scripts/sql-database-copy-database-to-new-server-powershell.md).</span></span>

## <a name="copy-a-database-by-using-transact-sql"></a><span data-ttu-id="cdc3f-129">Kopiera en databas med hjälp av Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="cdc3f-129">Copy a database by using Transact-SQL</span></span>

<span data-ttu-id="cdc3f-130">Logga in på master-databasen med de huvudsaklig inloggningen på servernivå eller inloggning som skapade databasen som du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-130">Log in to the master database with the server-level principal login or the login that created the database you want to copy.</span></span> <span data-ttu-id="cdc3f-131">För kopiering av databasen ska lyckas, måste inloggningar som inte är servernivå huvudnamn vara medlemmar i rollen dbmanager.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-131">For database copying to succeed, logins that are not the server-level principal must be members of the dbmanager role.</span></span> <span data-ttu-id="cdc3f-132">Mer information om inloggningar och ansluter till servern finns [hantera inloggningar](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-132">For more information about logins and connecting to the server, see [Manage logins](sql-database-manage-logins.md).</span></span>

<span data-ttu-id="cdc3f-133">Börja kopiera källdatabasen med den [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) instruktionen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-133">Start copying the source database with the [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) statement.</span></span> <span data-ttu-id="cdc3f-134">Kör den här instruktionen initierar databasen kopiera processen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-134">Executing this statement initiates the database copying process.</span></span> <span data-ttu-id="cdc3f-135">Eftersom kopiera en databas är en asynkron åtgärd, returnerar instruktionen CREATE DATABASE innan databasen kopieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-135">Because copying a database is an asynchronous process, the CREATE DATABASE statement returns before the database copying is complete.</span></span>

### <a name="copy-a-sql-database-to-the-same-server"></a><span data-ttu-id="cdc3f-136">Kopiera en SQL-databas på samma server</span><span class="sxs-lookup"><span data-stu-id="cdc3f-136">Copy a SQL database to the same server</span></span>
<span data-ttu-id="cdc3f-137">Logga in på master-databasen med de huvudsaklig inloggningen på servernivå eller inloggning som skapade databasen som du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-137">Log in to the master database with the server-level principal login or the login that created the database you want to copy.</span></span> <span data-ttu-id="cdc3f-138">För kopiering av databasen ska lyckas, måste inloggningar som inte är servernivå huvudnamn vara medlemmar i rollen dbmanager.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-138">For database copying to succeed, logins that are not the server-level principal must be members of the dbmanager role.</span></span>

<span data-ttu-id="cdc3f-139">Det här kommandot kopieras Database1 till en ny databas med namnet Database2 på samma server.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-139">This command copies Database1 to a new database named Database2 on the same server.</span></span> <span data-ttu-id="cdc3f-140">Kopiera åtgärden kan ta lite tid att slutföra beroende på storleken på databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-140">Depending on the size of your database, the copying operation might take some time to complete.</span></span>

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a><span data-ttu-id="cdc3f-141">Kopiera en SQL-databas till en annan server</span><span class="sxs-lookup"><span data-stu-id="cdc3f-141">Copy a SQL database to a different server</span></span>

<span data-ttu-id="cdc3f-142">Logga in på master-databasen på målservern, SQL database-server där den nya databasen ska skapas.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-142">Log in to the master database of the destination server, the SQL database server where the new database is to be created.</span></span> <span data-ttu-id="cdc3f-143">Använd en inloggning som har samma namn och lösenord som databasägaren källdatabasens på källservern för SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-143">Use a login that has the same name and password as the database owner of the source database on the source SQL database server.</span></span> <span data-ttu-id="cdc3f-144">Logga in på målservern måste också vara medlem i rollen dbmanager eller att den huvudsaklig inloggningen på servernivå.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-144">The login on the destination server must also be a member of the dbmanager role or be the server-level principal login.</span></span>

<span data-ttu-id="cdc3f-145">Det här kommandot kopieras Database1 på server1 till en ny databas med namnet Database2 på server2.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-145">This command copies Database1 on server1 to a new database named Database2 on server2.</span></span> <span data-ttu-id="cdc3f-146">Kopiera åtgärden kan ta lite tid att slutföra beroende på storleken på databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-146">Depending on the size of your database, the copying operation might take some time to complete.</span></span>

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a><span data-ttu-id="cdc3f-147">Övervaka förloppet för kopiering igen</span><span class="sxs-lookup"><span data-stu-id="cdc3f-147">Monitor the progress of the copying operation</span></span>

<span data-ttu-id="cdc3f-148">Övervaka kopieringen genom att fråga vyerna sys.databases och sys.dm_database_copies.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-148">Monitor the copying process by querying the sys.databases and sys.dm_database_copies views.</span></span> <span data-ttu-id="cdc3f-149">När kopieringen pågår, den **state_desc** kolumnen i vyn sys.databases för den nya databasen är inställd på **kopiering**.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-149">While the copying is in progress, the **state_desc** column of the sys.databases view for the new database is set to **COPYING**.</span></span>

* <span data-ttu-id="cdc3f-150">Om kopiering misslyckas den **state_desc** kolumnen i vyn sys.databases för den nya databasen är inställd på **MISSTÄNKER**.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-150">If the copying fails, the **state_desc** column of the sys.databases view for the new database is set to **SUSPECT**.</span></span> <span data-ttu-id="cdc3f-151">Kör instruktionen släpp på den nya databasen och försök igen senare.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-151">Execute the DROP statement on the new database, and try again later.</span></span>
* <span data-ttu-id="cdc3f-152">Om kopiering lyckas den **state_desc** kolumnen i vyn sys.databases för den nya databasen är inställd på **ONLINE**.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-152">If the copying succeeds, the **state_desc** column of the sys.databases view for the new database is set to **ONLINE**.</span></span> <span data-ttu-id="cdc3f-153">Kopieringen har slutförts och den nya databasen är en vanlig databas som kan ändras oberoende av källdatabasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-153">The copying is complete, and the new database is a regular database that can be changed independent of the source database.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc3f-154">Om du vill avbryta kopieringen medan det pågår kan köra den [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) satsen på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-154">If you decide to cancel the copying while it is in progress, execute the [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) statement on the new database.</span></span> <span data-ttu-id="cdc3f-155">Du kan också avbryter köra instruktionen DROP DATABASE på källdatabasen också kopieringen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-155">Alternatively, executing the DROP DATABASE statement on the source database also cancels the copying process.</span></span>
> 

## <a name="resolve-logins"></a><span data-ttu-id="cdc3f-156">Lös inloggningar</span><span class="sxs-lookup"><span data-stu-id="cdc3f-156">Resolve logins</span></span>

<span data-ttu-id="cdc3f-157">När den nya databasen är online på målservern, använda den [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) instruktionen för att mappa användare från den nya databasen inloggningar på målservern.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-157">After the new database is online on the destination server, use the [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) statement to remap the users from the new database to logins on the destination server.</span></span> <span data-ttu-id="cdc3f-158">Du löser ägarlösa användare se [felsöka ägarlösa användare](https://msdn.microsoft.com/library/ms175475.aspx).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-158">To resolve orphaned users, see [Troubleshoot Orphaned Users](https://msdn.microsoft.com/library/ms175475.aspx).</span></span> <span data-ttu-id="cdc3f-159">Se även [hur du hanterar Azure SQL database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-159">See also [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>

<span data-ttu-id="cdc3f-160">Alla användare i den nya databasen behålla de behörigheter som de hade i källdatabasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-160">All users in the new database retain the permissions that they had in the source database.</span></span> <span data-ttu-id="cdc3f-161">Användaren som initierade databaskopian blir databasens ägare för den nya databasen och tilldelas en ny säkerhetsidentifierare (SID).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-161">The user who initiated the database copy becomes the database owner of the new database and is assigned a new security identifier (SID).</span></span> <span data-ttu-id="cdc3f-162">När kopieringen lyckas och innan andra användare mappas om logga inloggningen som initierade kopierar, databasägaren, in på den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="cdc3f-162">After the copying succeeds and before other users are remapped, only the login that initiated the copying, the database owner, can log in to the new database.</span></span>

<span data-ttu-id="cdc3f-163">Läs om hur du hanterar användare och inloggningar när du kopierar en databas till en annan logisk server i [hur du hanterar Azure SQL database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-163">To learn about managing users and logins when you copy a database to a different logical server, see [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdc3f-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdc3f-164">Next steps</span></span>

* <span data-ttu-id="cdc3f-165">Information om inloggningar finns [hantera inloggningar](sql-database-manage-logins.md) och [hur du hanterar Azure SQL database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-165">For information about logins, see [Manage logins](sql-database-manage-logins.md) and [How to manage Azure SQL database security after disaster recovery](sql-database-geo-replication-security-config.md).</span></span>
* <span data-ttu-id="cdc3f-166">Om du vill exportera en databas finns [exportera databasen till en BACPAC](sql-database-export.md).</span><span class="sxs-lookup"><span data-stu-id="cdc3f-166">To export a database, see [Export the database to a BACPAC](sql-database-export.md).</span></span>
