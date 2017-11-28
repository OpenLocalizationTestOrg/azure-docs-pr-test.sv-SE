---
title: aaaTransparent kryptering av Data i SQL Data Warehouse (Portal) | Microsoft Docs
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
ms.openlocfilehash: 8233886ecf170844104e0d1459e2a829cafa9b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a><span data-ttu-id="2210d-103">Kom igång med Transparent Data kryptering (TDE) i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2210d-103">Get started with Transparent Data Encryption (TDE) in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2210d-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="2210d-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="2210d-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="2210d-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="2210d-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="2210d-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="2210d-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="2210d-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a><span data-ttu-id="2210d-108">Nödvändig behörighet</span><span class="sxs-lookup"><span data-stu-id="2210d-108">Required Permssions</span></span>
<span data-ttu-id="2210d-109">tooenable Transparent Data kryptering (TDE), du måste vara administratör eller medlem i hello dbmanager roll.</span><span class="sxs-lookup"><span data-stu-id="2210d-109">tooenable Transparent Data Encryption (TDE), you must be an administrator or a member of hello dbmanager role.</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="2210d-110">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="2210d-110">Enabling Encryption</span></span>
<span data-ttu-id="2210d-111">tooenable TDE för ett SQL Data Warehouse gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="2210d-111">tooenable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="2210d-112">Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="2210d-112">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="2210d-113">Klicka på hello i hello databasbladet **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="2210d-113">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="2210d-114">Välj hello **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="2210d-114">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="2210d-115">Välj hello **på** inställning![][2]</span><span class="sxs-lookup"><span data-stu-id="2210d-115">Select hello **On** setting ![][2]</span></span>
5. <span data-ttu-id="2210d-116">Välj **spara**
   ![][3]</span><span class="sxs-lookup"><span data-stu-id="2210d-116">Select **Save**
![][3]</span></span>  

## <a name="disabling-encryption"></a><span data-ttu-id="2210d-117">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="2210d-117">Disabling Encryption</span></span>
<span data-ttu-id="2210d-118">toodisable TDE för ett SQL Data Warehouse gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="2210d-118">toodisable TDE for a SQL Data Warehouse, follow hello steps below:</span></span>

1. <span data-ttu-id="2210d-119">Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="2210d-119">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="2210d-120">Klicka på hello i hello databasbladet **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="2210d-120">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="2210d-121">Välj hello **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="2210d-121">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="2210d-122">Välj hello **av** inställning![][4]</span><span class="sxs-lookup"><span data-stu-id="2210d-122">Select hello **Off** setting ![][4]</span></span>
5. <span data-ttu-id="2210d-123">Välj **spara**
   ![][5]</span><span class="sxs-lookup"><span data-stu-id="2210d-123">Select **Save**
![][5]</span></span>  

## <a name="encryption-dmvs"></a><span data-ttu-id="2210d-124">Kryptering av DMV: er</span><span class="sxs-lookup"><span data-stu-id="2210d-124">Encryption DMVs</span></span>
<span data-ttu-id="2210d-125">Kryptering kan bekräftas med hello följande av DMV: er:</span><span class="sxs-lookup"><span data-stu-id="2210d-125">Encryption can be confirmed with hello following DMVs:</span></span>

* <span data-ttu-id="2210d-126">[sys.Databases]</span><span class="sxs-lookup"><span data-stu-id="2210d-126">[sys.databases]</span></span>
* <span data-ttu-id="2210d-127">[sys.dm_pdw_nodes_database_encryption_keys]</span><span class="sxs-lookup"><span data-stu-id="2210d-127">[sys.dm_pdw_nodes_database_encryption_keys]</span></span>

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
