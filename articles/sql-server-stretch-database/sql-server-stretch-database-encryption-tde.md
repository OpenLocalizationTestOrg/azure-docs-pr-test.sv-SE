---
title: "aaaEnable Transparent datakryptering för Stretch Database - Azure | Microsoft Docs"
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
ms.openlocfilehash: 1d6bff455030ac8851b2184c1e8097afd61361d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a><span data-ttu-id="e6f47-103">Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure</span><span class="sxs-lookup"><span data-stu-id="e6f47-103">Enable Transparent Data Encryption (TDE) for Stretch Database on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6f47-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e6f47-104">Azure portal</span></span>](sql-server-stretch-database-encryption-tde.md)
> * [<span data-ttu-id="e6f47-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="e6f47-105">TSQL</span></span>](sql-server-stretch-database-tde-tsql.md)
>
>

<span data-ttu-id="e6f47-106">Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="e6f47-106">Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of hello database, associated backups, and transaction log files at rest without requiring changes toohello application.</span></span>

<span data-ttu-id="e6f47-107">TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen.</span><span class="sxs-lookup"><span data-stu-id="e6f47-107">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="e6f47-108">Hej databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="e6f47-108">hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="e6f47-109">hello inbyggda servercertifikatet är unikt för varje Azure-servern.</span><span class="sxs-lookup"><span data-stu-id="e6f47-109">hello built-in server certificate is unique for each Azure server.</span></span> <span data-ttu-id="e6f47-110">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="e6f47-110">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="e6f47-111">En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].</span><span class="sxs-lookup"><span data-stu-id="e6f47-111">For a general description of TDE, see [Transparent Data Encryption (TDE)].</span></span>

## <a name="enabling-encryption"></a><span data-ttu-id="e6f47-112">Aktivera kryptering</span><span class="sxs-lookup"><span data-stu-id="e6f47-112">Enabling Encryption</span></span>
<span data-ttu-id="e6f47-113">tooenable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="e6f47-113">tooenable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="e6f47-114">Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e6f47-114">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="e6f47-115">Klicka på hello i hello databasbladet **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="e6f47-115">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="e6f47-116">Välj hello **Transparent datakryptering** alternativet![][1]</span><span class="sxs-lookup"><span data-stu-id="e6f47-116">Select hello **Transparent data encryption** option ![][1]</span></span>
4. <span data-ttu-id="e6f47-117">Välj hello **på** inställningen och välj sedan **spara**
   ![][2]</span><span class="sxs-lookup"><span data-stu-id="e6f47-117">Select hello **On** setting, and then select **Save**
![][2]</span></span>

## <a name="disabling-encryption"></a><span data-ttu-id="e6f47-118">Om du inaktiverar kryptering</span><span class="sxs-lookup"><span data-stu-id="e6f47-118">Disabling Encryption</span></span>
<span data-ttu-id="e6f47-119">toodisable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="e6f47-119">toodisable TDE for an Azure database that's storing hello data migrated from a Stretch-enabled SQL Server database, do hello following things:</span></span>

1. <span data-ttu-id="e6f47-120">Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e6f47-120">Open hello database in hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="e6f47-121">Klicka på hello i hello databasbladet **inställningar** knappen</span><span class="sxs-lookup"><span data-stu-id="e6f47-121">In hello database blade, click hello **Settings** button</span></span>
3. <span data-ttu-id="e6f47-122">Välj hello **Transparent datakryptering** alternativet</span><span class="sxs-lookup"><span data-stu-id="e6f47-122">Select hello **Transparent data encryption** option</span></span>
4. <span data-ttu-id="e6f47-123">Välj hello **av** inställningen och välj sedan **spara**</span><span class="sxs-lookup"><span data-stu-id="e6f47-123">Select hello **Off** setting, and then select **Save**</span></span>

<!--Anchors-->
[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
