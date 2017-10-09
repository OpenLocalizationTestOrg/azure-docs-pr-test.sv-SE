---
title: "aaaLimitations i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver begränsningar i Azure-databas för PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: f53dd240e55e0633bc1dfb8ad25e1818fa8ae18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a><span data-ttu-id="4a517-103">Begränsningar i Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4a517-103">Limitations in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="4a517-104">hello Azure-databas för PostgreSQL-tjänsten är tillgänglig som förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="4a517-104">hello Azure Database for PostgreSQL service is in public preview.</span></span> <span data-ttu-id="4a517-105">hello följande avsnitt beskrivs kapacitet och funktionella gränser i hello database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4a517-105">hello following sections describe capacity and functional limits in hello database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="4a517-106">Tjänsten nivå maxkapacitet</span><span class="sxs-lookup"><span data-stu-id="4a517-106">Service Tier Maximums</span></span>
<span data-ttu-id="4a517-107">Azure-databas för PostgreSQL har flera tjänstnivåer som du kan välja mellan när du skapar en server.</span><span class="sxs-lookup"><span data-stu-id="4a517-107">Azure Database for PostgreSQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="4a517-108">Mer information finns i [förstå vad som är tillgängliga i varje tjänstnivå](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="4a517-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="4a517-109">Det finns ett maximalt antal anslutningar, enheter för beräkning och lagring i varje tjänstnivå hello service förhandsversionen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a517-109">There is a maximum number of connections, compute units, and storage in each service tier during hello service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="4a517-110">**Högsta antal anslutningar**</span><span class="sxs-lookup"><span data-stu-id="4a517-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="4a517-111">Grundläggande 50 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="4a517-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="4a517-112">50-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-112">50 connections</span></span>    |
| <span data-ttu-id="4a517-113">Grundläggande 100 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="4a517-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="4a517-114">100-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-114">100 connections</span></span>   |
| <span data-ttu-id="4a517-115">Standardenheter 100 beräkning</span><span class="sxs-lookup"><span data-stu-id="4a517-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="4a517-116">200-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-116">200 connections</span></span>   |
| <span data-ttu-id="4a517-117">Standardenheter 200 beräkning</span><span class="sxs-lookup"><span data-stu-id="4a517-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="4a517-118">300 anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-118">300 connections</span></span>   |
| <span data-ttu-id="4a517-119">Standardenheter 400 beräkning</span><span class="sxs-lookup"><span data-stu-id="4a517-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="4a517-120">400-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-120">400 connections</span></span>   |
| <span data-ttu-id="4a517-121">Standard 800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="4a517-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="4a517-122">500-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4a517-122">500 connections</span></span>   |
| <span data-ttu-id="4a517-123">**Max beräknings-enheter**</span><span class="sxs-lookup"><span data-stu-id="4a517-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="4a517-124">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="4a517-124">Basic service tier</span></span>         | <span data-ttu-id="4a517-125">100 enheter för beräkning</span><span class="sxs-lookup"><span data-stu-id="4a517-125">100 Compute Units</span></span> |
| <span data-ttu-id="4a517-126">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="4a517-126">Standard service tier</span></span>      | <span data-ttu-id="4a517-127">800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="4a517-127">800 Compute Units</span></span> |
| <span data-ttu-id="4a517-128">**Maximalt lagringsutrymme**</span><span class="sxs-lookup"><span data-stu-id="4a517-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="4a517-129">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="4a517-129">Basic service tier</span></span>         | <span data-ttu-id="4a517-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="4a517-130">1 TB</span></span>              |
| <span data-ttu-id="4a517-131">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="4a517-131">Standard service tier</span></span>      | <span data-ttu-id="4a517-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="4a517-132">1 TB</span></span>              |

<span data-ttu-id="4a517-133">När för många anslutningar har uppnåtts får hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="4a517-133">When too many connections are reached, you may receive hello following error:</span></span>
> <span data-ttu-id="4a517-134">Oåterkalleligt fel: Det gick tyvärr redan för många klienter</span><span class="sxs-lookup"><span data-stu-id="4a517-134">FATAL:  sorry, too many clients already</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="4a517-135">Funktionella begränsningar i förhandsversionen</span><span class="sxs-lookup"><span data-stu-id="4a517-135">Preview functional limitations</span></span>
### <a name="scale-operations"></a><span data-ttu-id="4a517-136">Skalningsåtgärder</span><span class="sxs-lookup"><span data-stu-id="4a517-136">Scale operations</span></span>
1.  <span data-ttu-id="4a517-137">Dynamisk skalning av servrar över tjänstnivåer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4a517-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="4a517-138">Att växla mellan tjänstnivåerna Basic och Standard.</span><span class="sxs-lookup"><span data-stu-id="4a517-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="4a517-139">Dynamisk på begäran ökningen av lagring i förväg skapade server stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4a517-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="4a517-140">Minska lagringsstorleken för server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="4a517-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="4a517-141">Version serveruppgraderingarna</span><span class="sxs-lookup"><span data-stu-id="4a517-141">Server version upgrades</span></span>
- <span data-ttu-id="4a517-142">Automatisk migrering mellan större database engine versioner stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4a517-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="4a517-143">Prenumerationshantering</span><span class="sxs-lookup"><span data-stu-id="4a517-143">Subscription management</span></span>
- <span data-ttu-id="4a517-144">Dynamiskt flytta förskapade servrar över prenumeration och resursgrupp stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4a517-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="4a517-145">Återställning till tidpunkt</span><span class="sxs-lookup"><span data-stu-id="4a517-145">Point-in-time-restore</span></span>
1.  <span data-ttu-id="4a517-146">Det går inte att återställa toodifferent tjänstnivå och/eller Compute enheter och lagringsstorlek.</span><span class="sxs-lookup"><span data-stu-id="4a517-146">Restoring toodifferent service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="4a517-147">Återställa en släppt server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="4a517-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a517-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a517-148">Next steps</span></span>
- <span data-ttu-id="4a517-149">Förstå [vad som är tillgängligt i varje prisnivå](concepts-service-tiers.md)</span><span class="sxs-lookup"><span data-stu-id="4a517-149">Understand [What’s available in each pricing tier](concepts-service-tiers.md)</span></span>
- <span data-ttu-id="4a517-150">Förstå [PostgreSQL-databas-versioner som stöds](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="4a517-150">Understand [Supported PostgreSQL Database Versions](concepts-supported-versions.md)</span></span>
- <span data-ttu-id="4a517-151">Granska [hur tooBack upp och återställa en server i Azure-databas för att använda PostgreSQL hello Azure-portalen](howto-restore-server-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4a517-151">Review [How tooBack up and Restore a server in Azure Database for PostgreSQL using hello Azure portal](howto-restore-server-portal.md)</span></span>
