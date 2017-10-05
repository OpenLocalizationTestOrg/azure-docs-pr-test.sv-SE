---
title: "Aktivera Transparent datakryptering för Stretch Database – Azure | Microsoft Docs"
description: "Aktivera Transparent datakryptering (TDE) för SQL Server Stretch Database på Azure"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: ceb355d2ba872ed5d3886c6dc82ca75b1854db0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a><span data-ttu-id="95d1b-103">Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure</span><span class="sxs-lookup"><span data-stu-id="95d1b-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95d1b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="95d1b-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="95d1b-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="95d1b-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="95d1b-106">Transparent Data kryptering (TDE) skyddar mot hot från skadlig aktivitet genom att utföra realtid kryptering och dekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar i programmet.</span><span class="sxs-lookup"><span data-stu-id="95d1b-106">Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application.</span></span>

<span data-ttu-id="95d1b-107">TDE krypterar lagring av en hel databas med hjälp av en symmetrisk nyckel som heter databaskrypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="95d1b-107">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="95d1b-108">Databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="95d1b-108">The database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="95d1b-109">Inbyggda certifikatet är unikt för varje Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="95d1b-109">The built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="95d1b-110">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="95d1b-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="95d1b-111">En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].</span><span class="sxs-lookup"><span data-stu-id="95d1b-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="95d1b-112">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="95d1b-112">Enabling Encryption</span></span>
<span data-ttu-id="95d1b-113">Om du vill aktivera TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="95d1b-113">To enable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="95d1b-114">Öppna databasen i den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="95d1b-114">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="95d1b-115">I databasbladet klickar du på den **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="95d1b-115">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="95d1b-116">Välj den **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="95d1b-116">Select the **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="95d1b-117">Välj den **på** inställningen och välj sedan **spara**
   ![][2]</span><span class="sxs-lookup"><span data-stu-id="95d1b-117">Select the **On** setting, and then select **Save**
![][2]</span></span>

## <a name="disabling-encryption"></a><span data-ttu-id="95d1b-118">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="95d1b-118">Disabling Encryption</span></span>
<span data-ttu-id="95d1b-119">Om du vill inaktivera TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="95d1b-119">To disable TDE for an Azure database that's storing the data migrated from a Stretch-enabled SQL Server database, do the following things:</span></span>

1. <span data-ttu-id="95d1b-120">Öppna databasen i den [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="95d1b-120">Open the database in the [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="95d1b-121">I databasbladet klickar du på den **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="95d1b-121">In the database blade, click the **Settings** button</span></span>
3. <span data-ttu-id="95d1b-122">Välj den **Transparent datakryptering** alternativet</span><span class="sxs-lookup"><span data-stu-id="95d1b-122">Select the **Transparent data encryption** option</span></span>
4. <span data-ttu-id="95d1b-123">Välj den **av** inställningen och välj sedan **spara**</span><span class="sxs-lookup"><span data-stu-id="95d1b-123">Select the **Off** setting, and then select **Save**</span></span>

<!--Anchors-->
<span data-ttu-id="95d1b-124">[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span><span class="sxs-lookup"><span data-stu-id="95d1b-124">[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx</span></span>


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
