---
title: aaaTransparent kryptering av Data i SQL Data Warehouse (T-SQL) | Microsoft Docs
description: Transparent datakryptering (TDE) i SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 3894431c76f14b217f3a6b9a42dbf2f4d216bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a><span data-ttu-id="29ad8-103">Kom igång med Transparent Data kryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="29ad8-103">Get started with Transparent Data Encryption (TDE)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29ad8-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="29ad8-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="29ad8-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="29ad8-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="29ad8-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="29ad8-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="29ad8-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="29ad8-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="29ad8-108">Nödvändig behörighet</span><span class="sxs-lookup"><span data-stu-id="29ad8-108">Required Permssions</span></span>
<span data-ttu-id="29ad8-109">tooenable Transparent Data kryptering (TDE), du måste vara administratör eller medlem i hello dbmanager roll.</span><span class="sxs-lookup"><span data-stu-id="29ad8-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="29ad8-110">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="29ad8-110">Enabling Encryption</span></span>
<span data-ttu-id="29ad8-111">Följ dessa steg tooenable TDE för ett SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="29ad8-111">Follow these steps tooenable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="29ad8-112">Ansluta toohello *master* databasen på värdservern för hello hello database med en inloggning som administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="29ad8-112">Connect toohello *master* database on hello server hosting hello database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="29ad8-113">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="29ad8-113">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a><span data-ttu-id="29ad8-114">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="29ad8-114">Disabling Encryption</span></span>
<span data-ttu-id="29ad8-115">Följ dessa steg toodisable TDE för ett SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="29ad8-115">Follow these steps toodisable TDE for a SQL Data Warehouse:</span></span>

1. <span data-ttu-id="29ad8-116">Ansluta toohello *master* databasen med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="29ad8-116">Connect toohello *master* database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="29ad8-117">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="29ad8-117">Execute hello following statement tooencrypt hello database.</span></span>

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> <span data-ttu-id="29ad8-118">Att måste återuppta ett pausat SQL Data Warehouse innan du ändrar toohello TDE inställningar.</span><span class="sxs-lookup"><span data-stu-id="29ad8-118">A paused SQL Data Warehouse must be resumed before making changes toohello TDE settings.</span></span>
> 
> 

## <a name="verifying-encryption"></a><span data-ttu-id="29ad8-119">Verifiering av kryptering</span><span class="sxs-lookup"><span data-stu-id="29ad8-119">Verifying Encryption</span></span>
<span data-ttu-id="29ad8-120">tooverify krypteringsstatus för en SQL Data Warehouse gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="29ad8-120">tooverify encryption status for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="29ad8-121">Ansluta toohello *master* eller instans database med en inloggning som administratör eller medlem i hello **dbmanager** roll i hello master-databasen</span><span class="sxs-lookup"><span data-stu-id="29ad8-121">Connect toohello *master* or instance database using a login that is an administrator or a member of hello **dbmanager** role in hello master database</span></span>
2. <span data-ttu-id="29ad8-122">Köra hello följande instruktion tooencrypt hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="29ad8-122">Execute hello following statement tooencrypt hello database.</span></span>

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

<span data-ttu-id="29ad8-123">Ett resultat av ```1``` anger en krypterad databas ```0``` anger en icke-krypterad databas.</span><span class="sxs-lookup"><span data-stu-id="29ad8-123">A result of ```1``` indicates an encrypted database, ```0``` indicates a non-encrypted database.</span></span>

## <a name="encryption-dmvs"></a><span data-ttu-id="29ad8-124">Kryptering av DMV: er</span><span class="sxs-lookup"><span data-stu-id="29ad8-124">Encryption DMVs</span></span>
* <span data-ttu-id="29ad8-125">[sys.Databases][sys.databases]</span><span class="sxs-lookup"><span data-stu-id="29ad8-125">[sys.databases][sys.databases]</span></span> 
* <span data-ttu-id="29ad8-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="29ad8-126">[sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
