---
title: "Begränsningar i Azure-databas för MySQL | Microsoft Docs"
description: "Beskriver begränsningar i förhandsversionen i Azure-databas för MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c61d70897b66c2ffee819ac98c38ab75000db907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a><span data-ttu-id="585f6-103">Begränsningar i Azure-databas för MySQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="585f6-103">Limitations in Azure Database for MySQL (Preview)</span></span>
<span data-ttu-id="585f6-104">Azure-databasen för MySQL-tjänsten är tillgänglig som förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="585f6-104">The Azure Database for MySQL service is in public preview.</span></span> <span data-ttu-id="585f6-105">I följande avsnitt beskrivs kapacitet och funktionella gränser i databastjänsten för.</span><span class="sxs-lookup"><span data-stu-id="585f6-105">The following sections describe capacity and functional limits in the database service.</span></span>

## <a name="service-tier-maximums"></a><span data-ttu-id="585f6-106">Tjänsten nivå maxkapacitet</span><span class="sxs-lookup"><span data-stu-id="585f6-106">Service Tier Maximums</span></span>
<span data-ttu-id="585f6-107">Azure MySQL-databas har flera tjänstnivåer som du kan välja mellan när du skapar en server.</span><span class="sxs-lookup"><span data-stu-id="585f6-107">Azure Database for MySQL has multiple service tiers you can choose from when creating a server.</span></span> <span data-ttu-id="585f6-108">Mer information finns i [förstå vad som är tillgängliga i varje tjänstnivå](concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="585f6-108">For more information, see [Understand what’s available in each service tier](concepts-service-tiers.md).</span></span>  

<span data-ttu-id="585f6-109">Det finns ett maximalt antal anslutningar, enheter för beräkning och lagring i varje tjänstnivå under förhandsgranskningen tjänsten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="585f6-109">There is a maximum number of connections, compute units, and storage in each service tier during the service preview, as follows:</span></span> 

|                            |                   |
| :------------------------- | :---------------- |
| <span data-ttu-id="585f6-110">**Högsta antal anslutningar**</span><span class="sxs-lookup"><span data-stu-id="585f6-110">**Max connections**</span></span>        |                   |
| <span data-ttu-id="585f6-111">Grundläggande 50 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="585f6-111">Basic 50 Compute Units</span></span>     | <span data-ttu-id="585f6-112">50-anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-112">50 connections</span></span>    |
| <span data-ttu-id="585f6-113">Grundläggande 100 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="585f6-113">Basic 100 Compute Units</span></span>    | <span data-ttu-id="585f6-114">100-anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-114">100 connections</span></span>   |
| <span data-ttu-id="585f6-115">Standardenheter 100 beräkning</span><span class="sxs-lookup"><span data-stu-id="585f6-115">Standard 100 Compute Units</span></span> | <span data-ttu-id="585f6-116">200-anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-116">200 connections</span></span>   |
| <span data-ttu-id="585f6-117">Standardenheter 200 beräkning</span><span class="sxs-lookup"><span data-stu-id="585f6-117">Standard 200 Compute Units</span></span> | <span data-ttu-id="585f6-118">300 anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-118">300 connections</span></span>   |
| <span data-ttu-id="585f6-119">Standardenheter 400 beräkning</span><span class="sxs-lookup"><span data-stu-id="585f6-119">Standard 400 Compute Units</span></span> | <span data-ttu-id="585f6-120">400-anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-120">400 connections</span></span>   |
| <span data-ttu-id="585f6-121">Standard 800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="585f6-121">Standard 800 Compute Units</span></span> | <span data-ttu-id="585f6-122">500-anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-122">500 connections</span></span>   |
| <span data-ttu-id="585f6-123">**Max beräknings-enheter**</span><span class="sxs-lookup"><span data-stu-id="585f6-123">**Max Compute Units**</span></span>      |                   |
| <span data-ttu-id="585f6-124">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="585f6-124">Basic service tier</span></span>         | <span data-ttu-id="585f6-125">100 enheter för beräkning</span><span class="sxs-lookup"><span data-stu-id="585f6-125">100 Compute Units</span></span> |
| <span data-ttu-id="585f6-126">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="585f6-126">Standard service tier</span></span>      | <span data-ttu-id="585f6-127">800 beräknings-enheter</span><span class="sxs-lookup"><span data-stu-id="585f6-127">800 Compute Units</span></span> |
| <span data-ttu-id="585f6-128">**Maximalt lagringsutrymme**</span><span class="sxs-lookup"><span data-stu-id="585f6-128">**Max storage**</span></span>            |                   |
| <span data-ttu-id="585f6-129">Grundläggande tjänstenivå</span><span class="sxs-lookup"><span data-stu-id="585f6-129">Basic service tier</span></span>         | <span data-ttu-id="585f6-130">1 TB</span><span class="sxs-lookup"><span data-stu-id="585f6-130">1 TB</span></span>              |
| <span data-ttu-id="585f6-131">Standardtjänstenivå</span><span class="sxs-lookup"><span data-stu-id="585f6-131">Standard service tier</span></span>      | <span data-ttu-id="585f6-132">1 TB</span><span class="sxs-lookup"><span data-stu-id="585f6-132">1 TB</span></span>              |

<span data-ttu-id="585f6-133">När för många anslutningar har uppnåtts, får du följande fel:</span><span class="sxs-lookup"><span data-stu-id="585f6-133">When too many connections are reached, you may receive the following error:</span></span>
> <span data-ttu-id="585f6-134">FEL 1040 (08004): För många anslutningar</span><span class="sxs-lookup"><span data-stu-id="585f6-134">ERROR 1040 (08004): Too many connections</span></span>

## <a name="preview-functional-limitations"></a><span data-ttu-id="585f6-135">Funktionella begränsningar i förhandsversionen:</span><span class="sxs-lookup"><span data-stu-id="585f6-135">Preview functional limitations:</span></span>
### <a name="scale-operations"></a><span data-ttu-id="585f6-136">Skalningsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="585f6-136">Scale operations:</span></span>
1.  <span data-ttu-id="585f6-137">Dynamisk skalning av servrar över tjänstnivåer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="585f6-137">Dynamic scaling of servers across service tiers is currently not supported.</span></span> <span data-ttu-id="585f6-138">Att växla mellan tjänstnivåerna Basic och Standard.</span><span class="sxs-lookup"><span data-stu-id="585f6-138">That is, switching between Basic and Standard service tiers.</span></span>
2.  <span data-ttu-id="585f6-139">Dynamisk på begäran ökningen av lagring i förväg skapade server stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="585f6-139">Dynamic on-demand increase of storage on pre-created server is currently not supported.</span></span>
3.  <span data-ttu-id="585f6-140">Minska lagringsstorleken för server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="585f6-140">Decreasing server storage size is not supported.</span></span>

### <a name="server-version-upgrades"></a><span data-ttu-id="585f6-141">Serveruppgraderingarna version:</span><span class="sxs-lookup"><span data-stu-id="585f6-141">Server version upgrades:</span></span>
- <span data-ttu-id="585f6-142">Automatisk migrering mellan större database engine versioner stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="585f6-142">Automated migration between major database engine versions is currently not supported.</span></span>

### <a name="subscription-management"></a><span data-ttu-id="585f6-143">Prenumerationshantering av:</span><span class="sxs-lookup"><span data-stu-id="585f6-143">Subscription management:</span></span>
- <span data-ttu-id="585f6-144">Dynamiskt flytta förskapade servrar över prenumeration och resursgrupp stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="585f6-144">Dynamically moving pre-created servers across subscription and resource group is currently not supported.</span></span>

### <a name="point-in-time-restore"></a><span data-ttu-id="585f6-145">Punkt-i-tid-återställning:</span><span class="sxs-lookup"><span data-stu-id="585f6-145">Point-in-time-restore:</span></span>
1.  <span data-ttu-id="585f6-146">Är inte tillåtet att återställa till olika tjänstnivå och/eller Compute enheter och lagringsstorlek.</span><span class="sxs-lookup"><span data-stu-id="585f6-146">Restoring to different service tier and/or Compute Units and Storage size is not allowed.</span></span>
2.  <span data-ttu-id="585f6-147">Återställa en släppt server stöds inte.</span><span class="sxs-lookup"><span data-stu-id="585f6-147">Restoring a dropped server is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="585f6-148">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="585f6-148">Next Steps:</span></span>
<span data-ttu-id="585f6-149">[Vad är tillgängliga i varje tjänstnivå](concepts-service-tiers.md)
[versioner som stöds av MySQL-databas](concepts-supported-versions.md)</span><span class="sxs-lookup"><span data-stu-id="585f6-149">[What’s available in each service tier](concepts-service-tiers.md)
[Supported MySQL Database Versions](concepts-supported-versions.md)</span></span>
