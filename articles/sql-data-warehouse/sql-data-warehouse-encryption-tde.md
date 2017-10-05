---
title: Transparent datakryptering i SQL Data Warehouse (Portal) | Microsoft Docs
description: Transparent datakryptering (TDE) i SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: b1db3bdfdfb54bda325c9b971cfcb4dd5efa333a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a><span data-ttu-id="04a98-103">Kom igång med Transparent Data kryptering (TDE) i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="04a98-103">Get started with Transparent Data Encryption (TDE) in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="04a98-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="04a98-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="04a98-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="04a98-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="04a98-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="04a98-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="04a98-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="04a98-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="04a98-108">Nödvändig behörighet</span><span class="sxs-lookup"><span data-stu-id="04a98-108">Required Permssions</span></span>
<span data-ttu-id="04a98-109">Om du vill aktivera Transparent Data kryptering (TDE) måste du vara administratör eller medlem i rollen dbmanager.</span><span class="sxs-lookup"><span data-stu-id="04a98-109">To enable Transparent Data Encryption (TDE), you must be an administrator or a member of the dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="04a98-110">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="04a98-110">Enabling Encryption</span></span>
<span data-ttu-id="04a98-111">Följ stegen nedan om du vill aktivera TDE för ett SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="04a98-111">To enable TDE for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="04a98-112">Öppna databasen i den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="04a98-112">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="04a98-113">I databasbladet klickar du på den **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="04a98-113">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="04a98-114">Välj den **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="04a98-114">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="04a98-115">Välj den **på** inställning![][2]</span><span class="sxs-lookup"><span data-stu-id="04a98-115">Select the **On** setting ![][2]</span></span>
5. <span data-ttu-id="04a98-116">Välj **spara**
   ![][3]</span><span class="sxs-lookup"><span data-stu-id="04a98-116">Select **Save**
![][3]</span></span>  

## <a name="disabling-encryption"></a><span data-ttu-id="04a98-117">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="04a98-117">Disabling Encryption</span></span>
<span data-ttu-id="04a98-118">Följ stegen nedan om du vill inaktivera TDE för ett SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="04a98-118">To disable TDE for a SQL Data Warehouse, follow the steps below:</span></span>

1. <span data-ttu-id="04a98-119">Öppna databasen i den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="04a98-119">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="04a98-120">I databasbladet klickar du på den **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="04a98-120">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="04a98-121">Välj den **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="04a98-121">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="04a98-122">Välj den **av** inställning![][4]</span><span class="sxs-lookup"><span data-stu-id="04a98-122">Select the **Off** setting ![][4]</span></span>
5. <span data-ttu-id="04a98-123">Välj **spara**
   ![][5]</span><span class="sxs-lookup"><span data-stu-id="04a98-123">Select **Save**
![][5]</span></span>  

## <a name="encryption-dmvs"></a><span data-ttu-id="04a98-124">Kryptering av DMV: er</span><span class="sxs-lookup"><span data-stu-id="04a98-124">Encryption DMVs</span></span>
<span data-ttu-id="04a98-125">Kryptering kan bekräftas med följande av DMV: er:</span><span class="sxs-lookup"><span data-stu-id="04a98-125">Encryption can be confirmed with the following DMVs:</span></span>

* <span data-ttu-id="04a98-126">[sys.Databases]</span><span class="sxs-lookup"><span data-stu-id="04a98-126">[sys.databases]</span></span>
* <span data-ttu-id="04a98-127">[sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="04a98-127">[sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
<span data-ttu-id="04a98-128">[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx</span><span class="sxs-lookup"><span data-stu-id="04a98-128">[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx</span></span>
<span data-ttu-id="04a98-129">[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx</span><span class="sxs-lookup"><span data-stu-id="04a98-129">[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
