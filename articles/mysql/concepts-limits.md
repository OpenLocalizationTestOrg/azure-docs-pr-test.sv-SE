---
title: "aaaLimitations i Azure-databas för MySQL | Microsoft Docs"
description: "Beskriver begränsningar i förhandsversionen i Azure-databas för MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 9c877c592bf640f62182d8761c9c51363882d706
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="b3dd9-103">Begränsningar i Azure-databas för MySQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="b3dd9-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="b3dd9-104">hello Azure-databas för MySQL-tjänsten är tillgänglig som förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-104">hello Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="b3dd9-105">hello följande avsnitt beskrivs kapacitet och funktionella gränser i hello database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-105">hello following sections describe capacity and functional limits in hello database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="b3dd9-106">Tjänsten nivå maxkapacitet</span><span class="sxs-lookup"><span data-stu-id="b3dd9-106">Service Tier Maximums</span></span>
<span data-ttu-id="b3dd9-107">Azure MySQL-databas har flera tjänstnivåer som du kan välja mellan när du skapar en server.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="b3dd9-108">Mer information finns i [förstå vad som är tillgängliga i varje tjänstnivå](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="b3dd9-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="b3dd9-109">Det finns ett maximalt antal anslutningar, enheter för beräkning och lagring i varje tjänstnivå hello service förhandsversionen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-109">There is a maximum number of connections, compute units, and storage in each service tier during hello service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="b3dd9-110">**Högsta antal anslutningar**</span><span class="sxs-lookup"><span data-stu-id="b3dd9-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="b3dd9-111">Grundläggande 50 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="b3dd9-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="b3dd9-112">50-anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-112">50 connections</span></span>    |
| <span data-ttu-id="b3dd9-113">Grundläggande 100 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="b3dd9-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="b3dd9-114">100-anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-114">100 connections</span></span>   |
| <span data-ttu-id="b3dd9-115">Standardenheter 100 beräkning</span><span class="sxs-lookup"><span data-stu-id="b3dd9-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="b3dd9-116">200-anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-116">200 connections</span></span>   |
| <span data-ttu-id="b3dd9-117">Standardenheter 200 beräkning</span><span class="sxs-lookup"><span data-stu-id="b3dd9-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="b3dd9-118">300 anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-118">300 connections</span></span>   |
| <span data-ttu-id="b3dd9-119">Standardenheter 400 beräkning</span><span class="sxs-lookup"><span data-stu-id="b3dd9-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="b3dd9-120">400-anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-120">400 connections</span></span>   |
| <span data-ttu-id="b3dd9-121">Standard 800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="b3dd9-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="b3dd9-122">500-anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-122">500 connections</span></span>   |
| <span data-ttu-id="b3dd9-123">**Max beräknings-enheter**</span><span class="sxs-lookup"><span data-stu-id="b3dd9-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="b3dd9-124">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="b3dd9-124">Basic service tier</span></span>         | <span data-ttu-id="b3dd9-125">100 enheter för beräkning</span><span class="sxs-lookup"><span data-stu-id="b3dd9-125">100 Compute Units</span></span> |
| <span data-ttu-id="b3dd9-126">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="b3dd9-126">Standard service tier</span></span>      | <span data-ttu-id="b3dd9-127">800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="b3dd9-127">800 Compute Units</span></span> |
| <span data-ttu-id="b3dd9-128">**Maximalt lagringsutrymme**</span><span class="sxs-lookup"><span data-stu-id="b3dd9-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="b3dd9-129">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="b3dd9-129">Basic service tier</span></span>         | <span data-ttu-id="b3dd9-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="b3dd9-130">1 TB</span></span>              |
| <span data-ttu-id="b3dd9-131">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="b3dd9-131">Standard service tier</span></span>      | <span data-ttu-id="b3dd9-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="b3dd9-132">1 TB</span></span>              |

<span data-ttu-id="b3dd9-133">När för många anslutningar har uppnåtts får hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-133">When too many connections are reached, you may receive hello following error:</span></span>
> <span data-ttu-id="b3dd9-134">FEL 1040 (08004): För många anslutningar</span><span class="sxs-lookup"><span data-stu-id="b3dd9-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="b3dd9-135">Funktionella begränsningar i förhandsversionen:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="b3dd9-136">Skalningsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-136">Scale operations:</span></span>
1.  <span data-ttu-id="b3dd9-137">Dynamisk skalning av servrar över tjänstnivåer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="b3dd9-138">Att växla mellan tjänstnivåerna Basic och Standard.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="b3dd9-139">Dynamisk på begäran ökningen av lagring i förväg skapade server stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="b3dd9-140">Minska lagringsstorleken för server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="b3dd9-141">Serveruppgraderingarna version:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-141">Server version upgrades:</span></span>
- <span data-ttu-id="b3dd9-142">Automatisk migrering mellan större database engine versioner stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="b3dd9-143">Prenumerationshantering av:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-143">Subscription management:</span></span>
- <span data-ttu-id="b3dd9-144">Dynamiskt flytta förskapade servrar över prenumeration och resursgrupp stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="b3dd9-145">Punkt-i-tid-återställning:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="b3dd9-146">Det går inte att återställa toodifferent tjänstnivå och/eller Compute enheter och lagringsstorlek.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-146">Restoring toodifferent service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="b3dd9-147">Återställa en släppt server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b3dd9-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3dd9-148">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="b3dd9-148">Next Steps:</span></span>
<span data-ttu-id="b3dd9-149">[Vad är tillgängliga i varje tjänstnivå](concepts-service-tiers.md)
[versioner som stöds av MySQL-databas](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="b3dd9-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
