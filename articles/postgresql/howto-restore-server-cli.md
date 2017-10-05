---
title: "Säkerhetskopiera och återställa en server i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Lär dig mer om att säkerhetskopiera och återställa en server i Azure-databas för PostgreSQL med hjälp av Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 871887e67d686a965a0648d2c6f0c72b3008db05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a><span data-ttu-id="d4dd8-103">Säkerhetskopiera och återställa en server i Azure-databas för PostgreSQL med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d4dd8-103">How to back up and restore a server in Azure Database for PostgreSQL by using the Azure CLI</span></span>

<span data-ttu-id="d4dd8-104">Använd Azure-databas för PostgreSQL för att återställa en server-databas till ett tidigare datum som sträcker sig från 7 till 35 dagar.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-104">Use Azure Database for PostgreSQL to restore a server database to an earlier date that spans from 7 to 35 days.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4dd8-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d4dd8-105">Prerequisites</span></span>
<span data-ttu-id="d4dd8-106">Du behöver följande för att slutföra den här instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d4dd8-106">To complete this how-to guide, you need:</span></span>
- <span data-ttu-id="d4dd8-107">En [Azure-databas för PostgreSQL-server och databas](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d4dd8-107">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> <span data-ttu-id="d4dd8-108">Om du installerar och använder Azure CLI lokalt, kräver den här instruktioner att du använder Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-108">If you install and use the Azure CLI locally, this how-to guide requires that you use Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d4dd8-109">Om du vill bekräfta version Kommandotolken Azure CLI, ange `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-109">To confirm the version, at the Azure CLI command prompt, enter `az --version`.</span></span> <span data-ttu-id="d4dd8-110">Om du vill installera eller uppgradera, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d4dd8-110">To install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="back-up-happens-automatically"></a><span data-ttu-id="d4dd8-111">Säkerhetskopiera sker automatiskt</span><span class="sxs-lookup"><span data-stu-id="d4dd8-111">Back up happens automatically</span></span>
<span data-ttu-id="d4dd8-112">När du använder Azure-databas för PostgreSQL görs databastjänsten en säkerhetskopia av tjänsten var femte minut.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-112">When you use Azure Database for PostgreSQL, the database service automatically makes a backup of the service every 5 minutes.</span></span> 

<span data-ttu-id="d4dd8-113">För grundläggande nivån finns säkerhetskopieringar för 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-113">For Basic Tier, the backups are available for 7 days.</span></span> <span data-ttu-id="d4dd8-114">För standardnivån är säkerhetskopior tillgängliga för 35 dagar.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-114">For Standard Tier, the backups are available for 35 days.</span></span> <span data-ttu-id="d4dd8-115">Mer information finns i [Azure-databas för PostgreSQL prisnivåer](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="d4dd8-115">For more information, see [Azure Database for PostgreSQL pricing tiers](concepts-service-tiers.md).</span></span>

<span data-ttu-id="d4dd8-116">Med funktionen om automatisk säkerhetskopiering kan du återställa servern och databaserna till ett tidigare datum eller tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-116">With this automatic backup feature, you can restore the server and its databases to an earlier date, or point in time.</span></span>

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a><span data-ttu-id="d4dd8-117">Återställa en databas till en tidigare tidpunkt med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d4dd8-117">Restore a database to a previous point in time by using the Azure CLI</span></span>
<span data-ttu-id="d4dd8-118">Använd Azure-databas för PostgreSQL för att återställa servern till en tidigare tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-118">Use Azure Database for PostgreSQL to restore the server to a previous point in time.</span></span> <span data-ttu-id="d4dd8-119">Återställda data kopieras till en ny server och den befintliga servern är kvar.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-119">The restored data is copied to a new server, and the existing server is left as is.</span></span> <span data-ttu-id="d4dd8-120">Till exempel kan en tabell av misstag utelämnas kl. tolv idag så, du återställa tiden innan på dagen.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-120">For example, if a table is accidentally dropped at noon today, you can restore to the time just before noon.</span></span> <span data-ttu-id="d4dd8-121">Du kan sedan hämta saknas tabellen och data från den återställda kopian av servern.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-121">Then, you can retrieve the missing table and data from the restored copy of the server.</span></span> 

<span data-ttu-id="d4dd8-122">Använd Azure CLI för att återställa servern [az postgres server återställning](/cli/azure/postgres/server#restore) kommando.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-122">To restore the server, use the Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore) command.</span></span>

### <a name="run-the-restore-command"></a><span data-ttu-id="d4dd8-123">Kör kommandot restore</span><span class="sxs-lookup"><span data-stu-id="d4dd8-123">Run the restore command</span></span>

<span data-ttu-id="d4dd8-124">Om du vill återställa servern Kommandotolken Azure CLI, anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d4dd8-124">To restore the server, at the Azure CLI command prompt, enter the following command:</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="d4dd8-125">Den `az postgres server restore` kommandot kräver följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="d4dd8-125">The `az postgres server restore` command requires the following parameters:</span></span>
| <span data-ttu-id="d4dd8-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="d4dd8-126">Setting</span></span> | <span data-ttu-id="d4dd8-127">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="d4dd8-127">Suggested value</span></span> | <span data-ttu-id="d4dd8-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d4dd8-128">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="d4dd8-129">resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-129">resource-group</span></span> |  <span data-ttu-id="d4dd8-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d4dd8-130">myResourceGroup</span></span> |  <span data-ttu-id="d4dd8-131">Resursgruppen där källservern finns.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-131">The resource group where the source server exists.</span></span>  |
| <span data-ttu-id="d4dd8-132">namn</span><span class="sxs-lookup"><span data-stu-id="d4dd8-132">name</span></span> | <span data-ttu-id="d4dd8-133">mypgserver återställs</span><span class="sxs-lookup"><span data-stu-id="d4dd8-133">mypgserver-restored</span></span> | <span data-ttu-id="d4dd8-134">Namnet på den nya servern som skapas med kommandot restore.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-134">The name of the new server that is created by the restore command.</span></span> |
| <span data-ttu-id="d4dd8-135">Återställ punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="d4dd8-135">restore-point-in-time</span></span> | <span data-ttu-id="d4dd8-136">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="d4dd8-136">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="d4dd8-137">Välj en punkt i tid att återställa till.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-137">Select a point in time to restore to.</span></span> <span data-ttu-id="d4dd8-138">Datumet och tiden måste vara inom källserverns säkerhetskopiera Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-138">This date and time must be within the source server's back up retention period.</span></span> <span data-ttu-id="d4dd8-139">Använd formatet ISO8601 datum och tid.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-139">Use the ISO8601 date and time format.</span></span> <span data-ttu-id="d4dd8-140">Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-140">For example, you can use your own local time zone, such as `2017-04-13T05:59:00-08:00`.</span></span> <span data-ttu-id="d4dd8-141">Du kan också använda Zulu UTC-format, till exempel `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-141">You can also use the UTC Zulu format, for example, `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="d4dd8-142">käll-servern</span><span class="sxs-lookup"><span data-stu-id="d4dd8-142">source-server</span></span> | <span data-ttu-id="d4dd8-143">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="d4dd8-143">mypgserver-20170401</span></span> | <span data-ttu-id="d4dd8-144">Namnet eller ID på källservern för att återställa från.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-144">The name or ID of the source server to restore from.</span></span> |

<span data-ttu-id="d4dd8-145">När du återställer en server till en tidigare tidpunkt, skapas en ny server.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-145">When you restore a server to an earlier point in time, a new server is created.</span></span> <span data-ttu-id="d4dd8-146">Den ursprungliga servern och dess databaser från den angivna tidpunkten kopieras till den nya servern.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-146">The original server and its databases from the specified point in time are copied to the new server.</span></span>

<span data-ttu-id="d4dd8-147">Plats och prisnivå nivåvärden för den återställda servern vara densamma som den ursprungliga servern.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-147">The location and pricing tier values for the restored server remain the same as the original server.</span></span> 

<span data-ttu-id="d4dd8-148">Den `az postgres server restore` kommandot är synkront.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-148">The `az postgres server restore` command is synchronous.</span></span> <span data-ttu-id="d4dd8-149">När servern har återställts kan använda du den för att upprepa processen för en annan tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-149">After the server is restored, you can use it again to repeat the process for a different point in time.</span></span> 

<span data-ttu-id="d4dd8-150">När återställningen är klar, leta upp den nya servern och kontrollera att data återställs som förväntat.</span><span class="sxs-lookup"><span data-stu-id="d4dd8-150">After the restore process finishes, locate the new server and verify that the data is restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4dd8-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4dd8-151">Next steps</span></span>
[<span data-ttu-id="d4dd8-152">Anslutningsbibliotek för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d4dd8-152">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
