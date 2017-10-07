---
title: "aaaEnable Transparent datakryptering för Stretch Database TSQL - Azure | Microsoft Docs"
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
ms.openlocfilehash: a9ba23649656fb344480d79438a1115f0eb353bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a><span data-ttu-id="f5501-103">Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="f5501-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure (Transact-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5501-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5501-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="f5501-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="f5501-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="f5501-106">Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="f5501-106">Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of hello database, associated backups, and transaction log files at rest without requiring changes toohello application.</span></span>

<span data-ttu-id="f5501-107">TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen.</span><span class="sxs-lookup"><span data-stu-id="f5501-107">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="f5501-108">Hej databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="f5501-108">hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="f5501-109">hello inbyggda servercertifikatet är unikt för varje Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="f5501-109">hello built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="f5501-110">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="f5501-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="f5501-111">En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].</span><span class="sxs-lookup"><span data-stu-id="f5501-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="f5501-112">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="f5501-112">Enabling Encryption</span></span>
<span data-ttu-id="f5501-113">tooenable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="f5501-113">tooenable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="f5501-114">Ansluta toohello *master* databasen på hello Azure server värd hello-databas med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="f5501-114">Connect toohello *master* database on hello Azure server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="f5501-115">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f5501-115">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="f5501-116">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="f5501-116">Disabling Encryption</span></span>
<span data-ttu-id="f5501-117">toodisable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="f5501-117">toodisable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="f5501-118">Ansluta toohello *master* databasen med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="f5501-118">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="f5501-119">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f5501-119">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a><span data-ttu-id="f5501-120">Verifiering av kryptering</span><span class="sxs-lookup"><span data-stu-id="f5501-120">Verifying Encryption</span></span>
<span data-ttu-id="f5501-121">tooverify krypteringsstatus för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="f5501-121">tooverify encryption status for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="f5501-122">Ansluta toohello *master* eller instans database med en inloggning som administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="f5501-122">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="f5501-123">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f5501-123">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="f5501-124">Ett resultat av ```1``` anger en krypterad databas ```0``` anger en icke-krypterad databas.</span><span class="sxs-lookup"><span data-stu-id="f5501-124">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

<!--Anchors-->
[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
