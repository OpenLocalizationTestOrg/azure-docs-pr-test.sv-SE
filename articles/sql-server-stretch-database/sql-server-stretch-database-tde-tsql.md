---
title: "Aktivera Transparent datakryptering för Stretch Database TSQL - Azure | Microsoft Docs"
description: "Aktivera Transparent datakryptering (TDE) för SQL Server Stretch Database på Azure TSQL"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: ed26c2b386e08b76f78b4a05e12c46d2b97c20f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a><span data-ttu-id="4cad6-103">Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="4cad6-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure (Transact-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4cad6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4cad6-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="4cad6-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="4cad6-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="4cad6-106">Transparent Data kryptering (TDE) skyddar mot hot från skadlig aktivitet genom att utföra realtid kryptering och dekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar i programmet.</span><span class="sxs-lookup"><span data-stu-id="4cad6-106">Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application.</span></span>

<span data-ttu-id="4cad6-107">TDE krypterar lagring av en hel databas med hjälp av en symmetrisk nyckel som heter databaskrypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="4cad6-107">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="4cad6-108">Databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="4cad6-108">The database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="4cad6-109">Inbyggda certifikatet är unikt för varje Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="4cad6-109">The built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="4cad6-110">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="4cad6-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="4cad6-111">En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].</span><span class="sxs-lookup"><span data-stu-id="4cad6-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="4cad6-112">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="4cad6-112">Enabling Encryption</span></span>
<span data-ttu-id="4cad6-113">Om du vill aktivera TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="4cad6-113">To enable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="4cad6-114">Ansluta till den *master* databas på Azure server som värd för databasen med en inloggning som administratör eller medlem i den **dbmanager** roll i master-databasen</span><span class="sxs-lookup"><span data-stu-id="4cad6-114">Connect to the *master* database on the Azure server hosting the database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="4cad6-115">Kör följande sats för att kryptera databasen.</span><span class="sxs-lookup"><span data-stu-id="4cad6-115">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="4cad6-116">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="4cad6-116">Disabling Encryption</span></span>
<span data-ttu-id="4cad6-117">Om du vill inaktivera TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="4cad6-117">To disable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="4cad6-118">Ansluta till den *master* databasen med en inloggning som är administratör eller medlem i den **dbmanager** roll i master-databasen</span><span class="sxs-lookup"><span data-stu-id="4cad6-118">Connect to the *master* database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="4cad6-119">Kör följande sats för att kryptera databasen.</span><span class="sxs-lookup"><span data-stu-id="4cad6-119">Execute the following statement to encrypt the database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a><span data-ttu-id="4cad6-120">Verifiering av kryptering</span><span class="sxs-lookup"><span data-stu-id="4cad6-120">Verifying Encryption</span></span>
<span data-ttu-id="4cad6-121">Kontrollera krypteringsstatus för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="4cad6-121">To verify encryption status for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="4cad6-122">Ansluta till den *master* eller instans database med en inloggning som administratör eller medlem i den **dbmanager** roll i master-databasen</span><span class="sxs-lookup"><span data-stu-id="4cad6-122">Connect to the *master* or instance database using a login that is an administrator or a member of the **dbmanager** role in the master database</span></span>
2. <span data-ttu-id="4cad6-123">Kör följande sats för att kryptera databasen.</span><span class="sxs-lookup"><span data-stu-id="4cad6-123">Execute the following statement to encrypt the database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="4cad6-124">Ett resultat av ```1``` anger en krypterad databas ```0``` anger en icke-krypterad databas.</span><span class="sxs-lookup"><span data-stu-id="4cad6-124">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

<!--Anchors-->
<span data-ttu-id="4cad6-125">[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span><span class="sxs-lookup"><span data-stu-id="4cad6-125">[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span></span>


<!--Image references-->

<!--Link references-->
