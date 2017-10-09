---
title: "Hur tooback upp och återställa en server i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Lär dig hur tooback upp och återställning av en server i Azure-databas för PostgreSQL med hjälp av hello Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 0b9ed25e3e3a88dd5c7ffe2ae7c27f8eef9be710
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-hello-azure-cli"></a><span data-ttu-id="699ca-103">Hur tooback upp och återställning av en server i Azure-databas för PostgreSQL med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="699ca-103">How tooback up and restore a server in Azure Database for PostgreSQL by using hello Azure CLI</span></span>

<span data-ttu-id="699ca-104">Använd Azure-databas för PostgreSQL toorestore en server-databasen tooan tidigare datum som sträcker sig från 7 too35 dagar.</span><span class="sxs-lookup"><span data-stu-id="699ca-104">Use Azure Database for PostgreSQL toorestore a server database tooan earlier date that spans from 7 too35 days.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="699ca-105">Krav</span><span class="sxs-lookup"><span data-stu-id="699ca-105">Prerequisites</span></span>
<span data-ttu-id="699ca-106">toocomplete detta hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="699ca-106">toocomplete this how-tooguide, you need:</span></span>
- <span data-ttu-id="699ca-107">En [Azure-databas för PostgreSQL-server och databas](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="699ca-107">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> <span data-ttu-id="699ca-108">Om du installerar och använder hello Azure CLI lokalt, kräver den här hur tooguide att du använder Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="699ca-108">If you install and use hello Azure CLI locally, this how-tooguide requires that you use Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="699ca-109">Ange tooconfirm hello version Kommandotolken hello Azure CLI `az --version`.</span><span class="sxs-lookup"><span data-stu-id="699ca-109">tooconfirm hello version, at hello Azure CLI command prompt, enter `az --version`.</span></span> <span data-ttu-id="699ca-110">tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="699ca-110">tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="back-up-happens-automatically"></a><span data-ttu-id="699ca-111">Säkerhetskopiera sker automatiskt</span><span class="sxs-lookup"><span data-stu-id="699ca-111">Back up happens automatically</span></span>
<span data-ttu-id="699ca-112">När du använder Azure-databas för PostgreSQL har hello database-tjänsten gör en säkerhetskopia av hello service automatiskt var femte minut.</span><span class="sxs-lookup"><span data-stu-id="699ca-112">When you use Azure Database for PostgreSQL, hello database service automatically makes a backup of hello service every 5 minutes.</span></span> 

<span data-ttu-id="699ca-113">För grundläggande nivån är hello säkerhetskopior tillgängliga för 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="699ca-113">For Basic Tier, hello backups are available for 7 days.</span></span> <span data-ttu-id="699ca-114">För standardnivån är hello säkerhetskopior tillgängliga för 35 dagar.</span><span class="sxs-lookup"><span data-stu-id="699ca-114">For Standard Tier, hello backups are available for 35 days.</span></span> <span data-ttu-id="699ca-115">Mer information finns i [Azure-databas för PostgreSQL prisnivåer](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="699ca-115">For more information, see [Azure Database for PostgreSQL pricing tiers](concepts-service-tiers.md).</span></span>

<span data-ttu-id="699ca-116">Med funktionen om automatisk säkerhetskopiering kan du återställa hello-servern och dess databaser tooan tidigare datum eller tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="699ca-116">With this automatic backup feature, you can restore hello server and its databases tooan earlier date, or point in time.</span></span>

## <a name="restore-a-database-tooa-previous-point-in-time-by-using-hello-azure-cli"></a><span data-ttu-id="699ca-117">Återställa en databas tooa tidigare punkt i tiden med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="699ca-117">Restore a database tooa previous point in time by using hello Azure CLI</span></span>
<span data-ttu-id="699ca-118">Använd Azure-databas för PostgreSQL toorestore hello server tooa tidigare tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="699ca-118">Use Azure Database for PostgreSQL toorestore hello server tooa previous point in time.</span></span> <span data-ttu-id="699ca-119">hello återställa data är kopierade tooa ny server och hello befintlig server är kvar.</span><span class="sxs-lookup"><span data-stu-id="699ca-119">hello restored data is copied tooa new server, and hello existing server is left as is.</span></span> <span data-ttu-id="699ca-120">Om en tabell av misstag utelämnas kl. tolv idag så, kan du återställa toohello tid innan på dagen.</span><span class="sxs-lookup"><span data-stu-id="699ca-120">For example, if a table is accidentally dropped at noon today, you can restore toohello time just before noon.</span></span> <span data-ttu-id="699ca-121">Sedan kan du hämta hello saknas tabellen och data från hello återställts kopia av hello-server.</span><span class="sxs-lookup"><span data-stu-id="699ca-121">Then, you can retrieve hello missing table and data from hello restored copy of hello server.</span></span> 

<span data-ttu-id="699ca-122">Använd hello Azure CLI-toorestore hello servern [az postgres server återställning](/cli/azure/postgres/server#restore) kommando.</span><span class="sxs-lookup"><span data-stu-id="699ca-122">toorestore hello server, use hello Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore) command.</span></span>

### <a name="run-hello-restore-command"></a><span data-ttu-id="699ca-123">Kör hello restore-kommandot</span><span class="sxs-lookup"><span data-stu-id="699ca-123">Run hello restore command</span></span>

<span data-ttu-id="699ca-124">toorestore hello server Kommandotolken hello Azure CLI, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="699ca-124">toorestore hello server, at hello Azure CLI command prompt, enter hello following command:</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="699ca-125">Hej `az postgres server restore` kommandot kräver hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="699ca-125">hello `az postgres server restore` command requires hello following parameters:</span></span>
| <span data-ttu-id="699ca-126">Inställning</span><span class="sxs-lookup"><span data-stu-id="699ca-126">Setting</span></span> | <span data-ttu-id="699ca-127">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="699ca-127">Suggested value</span></span> | <span data-ttu-id="699ca-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="699ca-128">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="699ca-129">resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="699ca-129">resource-group</span></span> |  <span data-ttu-id="699ca-130">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="699ca-130">myResourceGroup</span></span> |  <span data-ttu-id="699ca-131">Resursgruppens namn där det finns hello källservern.</span><span class="sxs-lookup"><span data-stu-id="699ca-131">The resource group where hello source server exists.</span></span>  |
| <span data-ttu-id="699ca-132">namn</span><span class="sxs-lookup"><span data-stu-id="699ca-132">name</span></span> | <span data-ttu-id="699ca-133">mypgserver återställs</span><span class="sxs-lookup"><span data-stu-id="699ca-133">mypgserver-restored</span></span> | <span data-ttu-id="699ca-134">hello namnet på hello nya server som skapas av hello restore-kommandot.</span><span class="sxs-lookup"><span data-stu-id="699ca-134">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="699ca-135">Återställ punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="699ca-135">restore-point-in-time</span></span> | <span data-ttu-id="699ca-136">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="699ca-136">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="699ca-137">Välj en punkt i tiden toorestore till.</span><span class="sxs-lookup"><span data-stu-id="699ca-137">Select a point in time toorestore to.</span></span> <span data-ttu-id="699ca-138">Datumet och tiden måste vara inom hello källserverns säkerhetskopiera Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="699ca-138">This date and time must be within hello source server's back up retention period.</span></span> <span data-ttu-id="699ca-139">Använd hello ISO8601 format för datum och tid.</span><span class="sxs-lookup"><span data-stu-id="699ca-139">Use hello ISO8601 date and time format.</span></span> <span data-ttu-id="699ca-140">Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`.</span><span class="sxs-lookup"><span data-stu-id="699ca-140">For example, you can use your own local time zone, such as `2017-04-13T05:59:00-08:00`.</span></span> <span data-ttu-id="699ca-141">Du kan också använda hello UTC Zulu format, till exempel `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="699ca-141">You can also use hello UTC Zulu format, for example, `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="699ca-142">käll-servern</span><span class="sxs-lookup"><span data-stu-id="699ca-142">source-server</span></span> | <span data-ttu-id="699ca-143">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="699ca-143">mypgserver-20170401</span></span> | <span data-ttu-id="699ca-144">hello namnet eller ID för hello källa server toorestore från.</span><span class="sxs-lookup"><span data-stu-id="699ca-144">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="699ca-145">När du återställer en server tooan tidigare tidpunkt, skapas en ny server.</span><span class="sxs-lookup"><span data-stu-id="699ca-145">When you restore a server tooan earlier point in time, a new server is created.</span></span> <span data-ttu-id="699ca-146">hello ursprungliga servern och dess databaser från hello har angetts tidpunkten är kopierade toohello ny server.</span><span class="sxs-lookup"><span data-stu-id="699ca-146">hello original server and its databases from hello specified point in time are copied toohello new server.</span></span>

<span data-ttu-id="699ca-147">hello plats och prissättning nivåvärden hello för hello återställa servern vara samma som hello ursprungliga servern.</span><span class="sxs-lookup"><span data-stu-id="699ca-147">hello location and pricing tier values for hello restored server remain hello same as hello original server.</span></span> 

<span data-ttu-id="699ca-148">Hej `az postgres server restore` kommandot är synkront.</span><span class="sxs-lookup"><span data-stu-id="699ca-148">hello `az postgres server restore` command is synchronous.</span></span> <span data-ttu-id="699ca-149">När hello server har återställts, kan du använda det igen toorepeat hello process för en annan plats i tid.</span><span class="sxs-lookup"><span data-stu-id="699ca-149">After hello server is restored, you can use it again toorepeat hello process for a different point in time.</span></span> 

<span data-ttu-id="699ca-150">Återställa slutförs efter hello, leta upp hello ny server och kontrollera att hello data återställs som förväntat.</span><span class="sxs-lookup"><span data-stu-id="699ca-150">After hello restore process finishes, locate hello new server and verify that hello data is restored as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="699ca-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="699ca-151">Next steps</span></span>
[<span data-ttu-id="699ca-152">Anslutningsbibliotek för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="699ca-152">Connection libraries for Azure Database for PostgreSQL</span></span>](concepts-connection-libraries.md)
